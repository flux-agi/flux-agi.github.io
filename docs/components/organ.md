# Organ

Organ a way to extend the network with off-circuit computations. Think of it as a module, or plugin, that receives a signal in the form of a spike trains, does something with this data, converts it back in the form of the spike trains and send back to the network. 

Organs use neurons as an input and output interface. Its totally up to developer on how to structure the interface, in can have any amount of input and output neurons, depending on the use case. Signal in the form of spike trains can be used to whatever purposes.


## Structure of the organ

### Definition
The root of the organ is organ definition class. In creates a definition for the module, that would be displayed in the interface and let you create organ instances from this class.

~~~js
interface OrganDefinition {
  /** Display name*/
  name: string;
  /** Alias by which you will be able to call the organ from the engine interface */
  alias: string;
  /** Current version in the form of "0.0.1" */
  version: string;
  /** Supported engine versions */
  engine: string;
  /** Path to repo if exists */
  git?: string;
  description?: string;

  /**
   * Returns and array of settings of the organ. Will be used to collect user input for the organ before
   * being added to the circuit
   */
  getSettingsScheme?(): OrganSettingScheme[];

  /**
   * Returns an array of neurons, that should be created for the organ. Neurons serve as input and/or output
   * interface of the organ
   */
  getNeuronsScheme?(settings: any): any[];

  /** Returns an organ class, that would be created once the circuit is initialized, and added to the engine. */
  getOrganClass(): any;
}
~~~

In the `getSettingsScheme` you can define the settings, that are needed to create an organ instance. Inside the settings body you can access the engine API, meaning that the settings can vary, depending on the circuit architecture if needed. This function returns an array of setting object of the following form:
~~~js
interface OrganSetting {
  title: string;
  description?: string;
  name: string;
  /** Any built in javascript type */
  type: any;
  required: boolean;
  default: any;
}
~~~

This is used to build a form in the editor interface.

In the `getNeuronsScheme` you define the neuron interface of the organ in the form of array. Later you can subscribe to the firing of this neurons, or fire them programmatically frm the organ class.

`getOrganClass` returns the actual Organ class, that handles implements organs logic.

### Organ class

Organ class is where the organ logic lives. It should extend the `Organ` class, and it gives the whole API to work with neurons, settings and env variables, provided in the definition and by user in the moment of organ added to the circuit. Here is the Organ base class:

~~~js
class Organ {
  id = "";
  neurons: any[] = [];
  settings: any = {};
  engine: any = null;

  constructor(
    {
      id,
      neurons,
      settings,
    }: {
      id: string;
      neurons: any[];
      settings: any;
    },
    engine: any
  ) {
    this.id = id;
    this.neurons = neurons;
    this.settings = settings;
    this.engine = engine;
  }

  state = {};

  renderTitle: null | (() => any) = null;

  onStateUpdate: null | ((prevProps: any) => void) = null;

  forceUpdate: null | (() => any) = null;

  render: null | (() => any) = null;

  setState = (modifier: any, callback?: () => void) => {
    const prevState = { ...this.state };
    const nextState = { ...this.state, ...modifier };
    this.state = nextState;
    if (this.onStateUpdate) {
      this.onStateUpdate(prevState);
    }
    if (callback) {
      callback();
    }
    if (this.forceUpdate) {
      this.forceUpdate();
    }
  };

  get = (name: string) => {
    return (this as any)[name];
  };

  getNeuronByAlias = (alias: string) => {
    console.log("neurons", this.neurons);
    return this.neurons.find((n) => n.alias === alias);
  };

  renderUI = (forceUpdate: any) => {
    this.forceUpdate = forceUpdate;
    if (this.render) {
      return this.render();
    }
    return null;
  };

  unmountUI = () => {
    this.forceUpdate = null;
  };
}

export default Organ;
~~~

### Organ logic

Once this organ is added to the circuit, it calls the `initialize` method. This is used to subscribe to its output and input neurons event, or any other events in the organ, like GraphQL subscriptions or other API calls. Its up to you how to structure the code. You can import some more functions from other files, use NPM libraries or write your own. Here is the example:

~~~js
initialize = async () => {
    // Accessing organs neurons by alias
    this.subs.push(
        this.getNeuronByAlias('input_1').on('fire', (e: NeuronEvent) => {
            this.firingCount = this.firingCount + 1;
        })
    );
    // Accessing engine neurons. All the engine API is accessible inside the organ class
    this.subs.push(
        this.engine.getNeuronById('111').on('level_change', (e: NeuronEvent) => {
            this._neuronsData[e.target.id] = e.target.level;
        })
    );
    // Firing output neuron when promise resolves
    const result = await callSomeApi('https://api.url', { settings: this.settings });
    this._convertToSpikes(result);
};
~~~

Don't forget to provide the cleanup functions in order to cancel all active subscriptions and do some other cleanup logic.

~~~js
cleanup = () => {
    this.subs.forEach((s: Subscription) => s.remove());
};
~~~

## Interface

Organs can have an interface, that is displayed in the organs rack of the editor. Interface is built as a React.js component. If you are familiar with React class components, it should be intuitively understood.

Organs can define `render` method, which should return a React.js component:
~~~js
render = () => {
    return (<p>My fancy interface</p>);
};
~~~

In case interface needs to be reactive to display the changes, get user input etc. For this case, you can define the state of the organ. State is reactive, meaning that whenever the state changes with `setState` method, the render method will be executed, and you will get updated UI in the interface.
~~~js
state = {
    count: 1
};

increment = () => {
    // Don't worry about race conditions, the changes are sequential
    this.setState({ count: count + 1 });
};

render = () => {
    return (
        <div>
            <p>Count: {this.state.count}</p>
            <button onClick={increment}>Increment</button>
        </div>   
    );
};
~~~

In case you need to run some action on state update, you can use `onStateUpdate` method:

~~~js
state = {
    count: 1
};

onStateUpdate = (prevState) => {
    console.log(`It was ${prevState.count} and now it is ${this.state.count}`);
};

increment = () => {
    // Don't worry about race conditions, the changes are sequential
    this.setState({ count: count + 1 });
};

render = () => {
    return (
        <div>
            <p>Count: {this.state.count}</p>
            <button onClick={increment}>Increment</button>
        </div>   
    );
};
~~~

You can use whatever tools in the render method. It can do pretty much the same as react does. You can import various interface libraries, use canvas or svg. Off course it is limited with the scope as the component renders in the Editors component tree, but this is probably out of scope. Organs need to be atomic and solve one thing at a time. If the logic is too complex, then they better be combined in a circuit. The flux philosophy is to keep things modular.

## Environment variables

Organs can rely on environment variables. For example, you might have a few organs, that control the robot's servo motors, and they all share the same IP address of your Arduino board. In this case you can define an environment variable in the organ definition class. This will merge in the base circuit globals, and you can set it in there.

Then you can use it in the organ class under `env` namespace:
~~~js
setServoPosition = () => {
    this.send(this.env.ARDUINO_IP, { some: data })
};
~~~

## Publishing module

Modules can be packed as a regular npm module. In this case they will be visible to the community and can be used in online version of the editor by you or another developers, if you make it public. You can have any NPM dependencies needed, as organs are loaded dynamically to the editor. You can query APIs, do some computations, whenever is needed. There can be organs that are related to sensors, actuators, that query APIs, perform some non spiking ML, send data to another services. The only limit is your imagination.

## Share organs

We highly encourage you to share organs with Flux community. The more valuable code is shared, the fasted we get to our goal - transcend human intelligence.


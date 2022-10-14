# Generator

What if you need to create some repetative circuit, lets say a reservoir of 100 randomly connected neurons. This is where generators kick in. Basically, generator is a function, that returns a json description of the circuit and lets you programatically define circuit nodes and connections between them.

Generator can accept settings to collect user input and also has an access to the current network via Engine API. Here is the example code for the generator that creates a so called reservoir (collection of randomly recurrently connected neurons with fixed synaptic weights):

~~~js
import Generator from "@flux-agi/generator";

class ReservoirDefinition extends Generator {
  name = "Reservoir";
  version = "0.0.1";
  engine = "0.0.1";
  alias = "reservoir";

  getSettingsScheme = () => [
    {
      label: "title:",
      name: "title",
      type: String,
      default: this.name,
    },
    {
      label: "Number of neurons:",
      name: "numberOfNeurons",
      type: Number,
      default: 100,
    },
  ];

  generate = () => {
    const {
      numberOfNeurons,
      connectionsDensity,
      inputNeuronsNumber,
      outputNeuronsNumber,
    } = this.settings;
    const neuronsPerLine = Math.ceil(Math.sqrt(numberOfNeurons));
    const neurons = Array(numberOfNeurons).map((_, i: number) => {
      const x = i * 60;
      const y = Math.floor(i / neuronsPerLine) * 60;
      return {
        x,
        alias: `${this.alias}_neuron_${i}`,
        y,
        neuronType: "IAF",
        autoFiring: 0,
        threshold: 100,
        maxSum: 100,
        leak: 20,
        refractoryPeriod: 5,
        isOutput: true,
      };
    });

    return neurons;
  };
}

export default ReservoirDefinition;

~~~

So generator is basically a boilerplate that helps you create configurable circuits on the fly, depending on the parameters that you provide. It can be very useful in many situations.

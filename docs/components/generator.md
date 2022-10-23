# Generator

If you need to create a circuit based on a set of parameters (let's say a reservoir of 100 randomly connected neurons), you can use generators. Basically, a generator is a function that accepts a set of user-defined parameters and returns a list of circuit nodes in a json format.

Generators have access to the current network via the Engine API. Here is an example code for the generator that creates a "reservoir" (a collection of randomly recurrently connected neurons with fixed synaptic weights):

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

A generator is basically a boilerplate that helps you create configurable circuits on the fly, based on a set of parameters that you provide.

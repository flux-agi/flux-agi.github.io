# Engine

Engine is the root of the network. It handles the interaction between neurons, and provides a general API for accessing and manipulating network state. In can be treated as the root of the Flux networks.

On the engine level then nesting and hierarchy is ignored, and all the connectome is converged to flat list of neurons and connections between them. Organs are also treated as collection of neurons, with an exception that the organ code runs some arbirtary operations, but in the end, they alweays result in the set of activity of interace neurons.

Engine exposes an API to control the execution and access the components up to individual neurons and synapses, and call methods on them. This is mostly usful in Organs, where engine instance is passed to the constructor and accessible in organ methods.

Engine works with a single root circuit instance:

In case you change the root circuit, the new one will be loaded to the engine and executed.

## Methods

| Name                             | Type                                        | Description                                                                                                                                |
| -------------------------------- | -------------------- |  ------------------------------------------------------------------------------------------------------------------------------------------ |
| `init`                    | (circuit: CircuitType) => void                  | Loads root circuit and prepares it for execution        |
| `on`                    | (node: NodeType, id: string, callback) => void                 | Subscribes to a change on a node  |
| `emit`                    | (node: NodeType, id: string, payload: any) =>void                  | Subscribes to a change on a node  |
| `registerOrgans`                    | (organs: [OrganInstanceType]) => void               | Loads a list of local organs and makes them accessible in the explorer    |
| `registerGenerators`                    | (generators: [OrganInstanceType]) => void              | Loads a list of local generators and makes them accessible in the explorer      |
| `getOrganById`                    | (id: String) => OrganInstanceType                 | Returns organ instance by id |
| `getOrganByAlias`                    | (alias: String) => OrganInstanceType                  | Returns organ instance by alias |
| `getNeuronById`                    | (id: String) => OrganInstanceType                  | Returns neuron by id |
| `getNeuronByAlias`                   | (alias: String) => OrganInstanceType                  | Returns neuron by alias |
| `addNeuron`                    | (neuron: NeuronTypes) => OrganInstanceType                  | Adds neuron to the engine |
| `addOrgan`                    | (organ: OrganInstance) => OrganInstanceType                 | Adds organ instance to the engine |
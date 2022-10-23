# Engine

The engine is the root of the FLUX artificial nervous system. It handles the interaction between neurons, performs computations, orchestrates the activity and provides a rich API for accessing and manipulating network state and individual nodes.

At the engine level, nesting and hierarchy are ignored, and all the network components are converged to a flat list of neurons and connections between them. Organs represent a standalone computational unit that interacts with the network via its neuronal interface.

The engine exposes an API to control the execution and access the components up to individual neurons and synapses, and call methods on them. This is mostly useful in organs. An Engine instance is passed to the constructor and accessible in the Organs and Generators classes.

The engine works with a single root circuit instance:

If you change the root circuit, the new one will be loaded into the engine and executed.

## Methods

| Name                             | Type                                        | Description                                                                                                                                |
| -------------------------------- | -------------------- |  ------------------------------------------------------------------------------------------------------------------------------------------ |
| `init`                    | (circuit: CircuitType) => void                  | Loads root circuit and prepares it for execution        |
| `on`                    | (node: NodeType, id: string, callback) => void                 | Subscribes to a change on a selected node  |
| `emit`                    | (node: NodeType, id: string, payload: any) =>void                  | Subscribes to a change on a node  |
| `registerOrgans`                    | (organs: [OrganInstanceType]) => void               | Loads a list of local organs and makes them accessible in the explorer    |
| `registerGenerators`                    | (generators: [OrganInstanceType]) => void              | Loads a list of local generators and makes them accessible in the explorer      |
| `getOrganById`                    | (id: String) => OrganInstanceType                 | Returns organ instance by id |
| `getOrganByAlias`                    | (alias: String) => OrganInstanceType                  | Returns organ instance by alias |
| `getNeuronById`                    | (id: String) => OrganInstanceType                  | Returns neuron by id |
| `getNeuronByAlias`                   | (alias: String) => OrganInstanceType                  | Returns neuron by alias |
| `addNeuron`                    | (neuron: NeuronTypes) => OrganInstanceType                  | Adds neuron to the engine |
| `addOrgan`                    | (organ: OrganInstance) => OrganInstanceType                 | Adds organ instance to the engine |
# Architecture overview

## Circuit

Circuit is a root unit of what we work with. It can contain various nodes, like neurons, organs and another circuits and connections between them. You can easily group nodes into circuits and create deep nested architectures. Circuits can be saved to prototypes, for use in another circuits from the explorer. Circuits can be duplicated and grouped, so it is easy to make modifications (for example the control of the hexapod leg).

## Nodes

Nodes are the builing blocks of the component. Each circuit can be a node of another circuit, alongside with neurons and organs.

#### Neuron

Neuron is the basic computational unit of the system. Currently it follows the Belkin-Kaygorodov Leaky linear neuron, but in future we would expand to different SNN engines. Neuron is described here.

#### Organ

Organ is a way to extend the circuit functionality by adding non-neuronal computation. There can be input, output or mixed organs. Ograns communicate with the circuit using neuron interface. Input organs receive spike trains with its input neurons, and do arbitaty computations with the data. Output organs serialize its data into the spike trains and send it over its output neural interface. You can do whatever you want in the organ body, it is not limited. Organs can have an interface.

## Engine

Engine is the global api, that can be accessible in organs. By this it can read the activity of the whole network and manipulate this.


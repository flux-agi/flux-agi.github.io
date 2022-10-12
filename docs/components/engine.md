# Engine

Engine is the root of the network. It handles the interaction between neurons, and provides a general API for accessing and manipulating network state. In can be treated as the root of the Flux networks.

In gives a powerful API to control the execution and access the components up to individual neurons and synapses, and call methods on them. This is mostly usful in Organs, where engine instance is passed to the contructor and accessible in organ methods.

Engine works with a single root circuit instance:

In case you change the root circuit, the new one will be loaded to the engine and executed.
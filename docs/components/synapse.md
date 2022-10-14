# Synapse

Synapse it a way of neuron to communicate with another neuron. It can be thought as a channel of communication between neurons. There are three types of synapses:

| Type                             | Description                 | 
| -------------------------------- | -------------------- |
| `Direct`                 | Alters the active level of neurons, and causes neuron to fire in case threshold is exceeded                 |
| `Modulator`                             | Alters the modulation level, that controls the neurons properties. Does not participate in direct firing               |
| `Electric`                             | Causes receptor neuron to fire unconditionally             |

![Synapse](../_media/synapse_types.svg)

In the interface, direct synapses are displayed as a guide from one neuron to another. The width of synapse is proportional to its weight.
Synapses can have both positive and negative weight. In case it is positive, it is colored as green. In case it is negative - blue.
The gradient of the line indicates the direction - the color is more vivid closer to the emitter cell, and desaturates closer to the receiver cell.
Synapses can be a subject to hebbian spike timing dependent learning. The learned weight is displayed as a border. In case it is learned to bigger weight, it is displayed as white, otherwise, as yellow:


## Properties

| Name                             | Type                 | Default                        | Description                                                                                                                                |
| -------------------------------- | -------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `weight`                    | `Int8` | `20`                    | Default synaptic weight. CAn be alrred by both short-term and long-term learning.                                                                                                                       |
| `modulation`              | `Int8`             | `20`                            | Rate at which the level is decreasing.                                                                                               |



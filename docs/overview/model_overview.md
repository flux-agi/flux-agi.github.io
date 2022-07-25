# Model overview

Flux follows its own implementation of spiking neural network, based on Belkin-Kaygorodov model. Its aim is to simplify the neural activity to the primitives and keep the only operation as addition. This model is most computationally cheap and also is easier to represent in silicon. Basic concepts are:

## Leaky linear neuron.

LLN is a model of the neuron that aims to mimic the behavior of the biological neuron avoiding the complexity of the biological neuron. The only operations that we have in the neuron is addition. No differential equations to mimic the membrane potential or any other biologically inspired computation. It works as a summator. Neuron in the model can be represented as a leaky barrel with liquid. Each afferent synapse adds some water into the barrel. It also has a leak rate, by which the water is pulled of it. Once the level of water reaches a certain threshold, it fires a spike over all of its efferent connections and the water comes to a baseline, followed by a refractory period in which it ignores any signals. The level increase is equal to the weight of the synapse. Think of it as a pipe diameter. If the level of neuron is increased by a big amount, it can burst spikes. Another feature of neuron is adaptation. If too much signals are coming in during refractory period, it increases its threshold. Habitution is another process by which neuron increases its firing threshold.

## Synapse.

There are three types of synapses in the network, inspired by biological counterparts:
* Direct. This type of synapses are either inhibiting a neuron or exciting it. In the water barrel analogy, they either increase the water level, or decrease it by its weight. It can also be thought as glutamate or GABA synapses in the biological networks.
* Modulator. Do not directly affect the neuron properties, but modulate its properties, like threshold, adaptation rate, habituation rate and LTP and STP rate. This is inspired by various neuromodulators in the brain, like dopamine, seratorin etc, but simplified to a single type of synapse.
* Electric. Directly fires the recepient neuron, bypassing the summation process. In biology this is represented by a junction synapse, that passes discharge right to the neurons membrane.

Synapses have a special properties called LTP and STP, which stand for Short-term potentiation and Long-term potentiation. This follows a Hebbian learning paradigm and allows the network to learn from its activity. This is a configurable parameter, that can also be affected by the modulatory synapses.

## No central clock

There is no central clock in the network. We rely on a javascript event loop. Each neuron manages its own timing. The only thing that syncronizes the network is the firing of neurons.


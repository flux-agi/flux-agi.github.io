# Neural model overview

Flux engine is built on its own implementation of modulated spiking neural network, that aims to find a perfect balance between biological accuracy and computational performance. It reduces the nervous system to its core functional features, avoiding recreating the biological complexity. Its aim is to simplify the neural activity to the primitives and keep the only operation as addition. This model is most computationally cheap and also is easier to represent in silicon. Basic concepts are:

## Leaky modulated integrate-and-fire neuron

Neuron that aims to mimic the behavior of the biological neuron avoiding the complexity of the biological counterpart. Neuron in the model works as a simple summator, avoiding the complexity of biological electrodynamics properties, in favor on its functional sense and computational efficiency. Neuron in the model can be represented as a leaky vessel with liquid:

![Neuron model](../_media/neuron_model.svg)

Each incoming synapse adds or subtracts some liquid from the vessel. Awhile, liquid also leaks from the vessel at a constant rate, that is controlled by a `leak` parameter. Once the level of liquid reaches a certain `threshold`, neuron fires a spike: meaning it propagates a signal over all of its active synapses, so they affect liquid levels of connected neurons. After this, the level drops to the baseline. Right after firing, neuron enters a `refractory period`, during which neurons closes its gates and does not receive any signals.
If the level of neuron overshoots threshold by a significant amount, it can burst a series of spikes.

If too much signals are coming in during refractory period, it increases its threshold. This process is called habituation, and can be modified by a `habituation rate`.

Vice-verse, if a neuron does not receive stimulation for a long time, it decreases its threshold. This process is called potentiation, and is controlled by `potentiation rate`.

Apart from the summation, leak, habituation and potentiation, neurons can be modulated. Modulation does not affect the neuron level itself, but alters its parameters, like threshold, learning rate, adaptation etc.

Beside the level mechanism, that is mediated by `direct synapses`, neuron has a modulation mechanism, that is controlled by `modulatory synapses`. Modulation level does not participate in direct firing, but alters the neuron properties, like threshold and learning rate.
Modulation also has its level, but it acts a bit differently: modulation level seek balance, and modulation synapses are displacing the balance to either negative and positive side:

![Modulation model](../_media/modulation_model.svg)

In case modulation is on negative, it dampens the related params. For example, if the threshold is modulated, it will temporarily increase it if the modulation level is shifted to negative, so the neuron gets less responsive. Otherwise, if the shift is to positive, it lowers its threshold, and gets more responsive. If modulation level is balanced, it does not have any effect.

## 3 types of synapses

There are three types of synapses in the model, inspired by biological counterparts:
* `Direct`. This type of synapses are either inhibiting a neuron or exciting it. In the vessel with liquid analogy, they either increase the liquid level, or decrease it by its weight. It can also be thought as glutamate or GABA synapses in the biological networks.
* `Modulator`. Do not directly affect the neuron properties, but modulate its properties, like threshold, adaptation rate, habituation rate and LTP and STP rate. This is inspired by various neuromodulators in the brain, like dopamine, seratorin etc, but simplified to a single type of synapse.
* `Electric`. Directly fires the recepient neuron, bypassing the summation process. In biology this is represented by a junction synapse, that passes discharge right to the neurons membrane.

Synapse has a weight, that can be thought of as a pipe diameter. If a synapse is `plastic`, it can be altered, either temporarily, or persistently, by the mechanisms of short-term and long-term memory.

Here is the simple implementation of simple adjustable [Central Pattern Generator](https://en.wikipedia.org/wiki/Central_pattern_generator) using the model:

![Generator model](../_media/generator_model.svg)

In this minimalist model, the generator produces rythmic output at a constant rate. By exciting neurons with inhibitory or excitatory modulatory synapses, that alter target neurons firing threshold, we can manipulate the output rate of the generator. In the real word example, this can be a network that maintains robots level or arousal, that affects its behavior.

## Short and long term memory

Model implements both short-term and long-term momery mechanism, based on spike timing dependent Hebbian process. It maintains a "tail" of neuronal activity, which is basically a registry of neurons that fire with a timestamp, and in case the neuron fires just before another neurons, model increases the synaptic weight of the connecting synapse. In case the neuron fires just before another neuron, it decreases synaptic weight. This is an implementation of the well known Hebbian postulate: 
> Neurons that fire together, wire together. Neurons that fire apart, wire apart. 

Depending on the neuron type, this can happen only when two neurons have are connected by synapse, or if they are related topologically.

Plasticity is also a target for modulation. Modulation is the main tool for persisting the weight change, moving memory from short to long term. This is basically the functionally simple way of how plasticity works in the brain, and is sufficient for the artificial nervous system to learn and adapt.

## Asyncronous communication 

There is no central clock in the network. Each neuron manages its own timing. The only thing that syncronizes the network is the firing of neurons. Inconsistencies in event loop add a natural noise to the network dynamics.


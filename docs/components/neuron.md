# Neuron

The fundamental computational unit of FLUX artificial nervous systems is the neuron. We rely on WebGPU to carry out quick parallel computations on any machine. To make WebGPU support available, you must perform some special actions. We fall back to a CPU version if it is not enabled, although it is significantly less efficient (up to 200 times slower).


![Neuron](../_media/neuron_types.svg)


A neuron has 2 internal "levels": the threshold level and the modulation level. Threshold level is controlled by `direct` synapses, while modulation level is controlled by `modulatory` synapses.

Upon firing, a neuron can either generate a single spike or a spike train. It depends on a threshold overshoot.

After firing, a neuron enters a refractory period, meaning it will not react to any incoming signals.

Other important properties are adaptation and potentiation. If a neuron is stimulated too strongly during the refractory period, it will increase its threshold. This process is called `adaptation` and it is very important for network stability. If a neuron does not get much stimulation for a long time, it reduces its threshold in order to be more reactive. This process is called `potentiation`.

## Properties

| Name                             | Type                 | Default                        | Description                                                                                                                                |
| -------------------------------- | -------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `threshold`                    | `Int8` | `127`                    | Default firing threshold.                                                                                                                       |
| `leak`              | `Int8`             | `20`                            | The rate at which the level is decreasing.                                                                                             |
| `modulationLeak`              | `Int8`             | `0`                          | The rate at which the modulation level returns to equilibrium. If `0`, neuron is not sensitive to modulation.                                                                                   |
| `burstingRate`              | `Int8`             | `0`                          | If value is `> 0` and threshold is overshot significantly before firing, the neuron will respond with a spike train instead of a single spike. If `0, the neuron always responds with a single spike.                                                                                     |
| `autoFiringRate`              | `Int16`             | `0`                          | The interval in milliseconds, which determines the firing frequency of a neuron without any external stimulation. If `0`, auto-firing is off.                                                                                    |
| `inputAlias`              | `String`             | `null`                          | Neuron will be designated as input if set (with inward pointing an arrow and alias).                                                                |
| `inputDescription`              | `String`             | `null`                          | Additional information related to the input will be displayed on hover.                                                                              |
| `outputAlias`              | `String`             | `null`                          | Neuron will be designated as output if set (with outward pointing an arrow and alias)                                                                              |
| `outputDescription`              | `String`             | `null`                          | Additional information related to the output will be displayed on hover.                                                                                       |

These properties are used as the default characteristics of a neuron. Modulation is a way to alter those properties. The modulation level, in the same way as the regular level, can be either positive or negative, though there is a difference in level handling: for the direct level, the `leak` always decreases the level to `0`. In the case of modulation level, it seeks balance: a center point. Modulation is displaced to either negative or positive side by incoming `modulatory synapses`. For instance, if the threshold is modulated and the modulation level is changed to negative, the active threshold is raised and the neuron becomes less sensitive. Threshold will be reduced if modulation is displaced to the positive side.

When accessed via the engine API (for example, in an organ body), neuron exposes several methods.

## Methods

| Name                             | Type                                        | Description                                                                                                                                |
| -------------------------------- | -------------------- |  ------------------------------------------------------------------------------------------------------------------------------------------ |
| `fire`                    | `() => void`                  | Emits a spike, activating all efferent synapses        |
| `summation`                    | `(weight: number) => void`                  | Adds a weight to neurons direct level and initiates re-calculation        |
| `modulate`                    | `(weight: number) => void`                  | Adds a weight to neurons modulation level and initiates re-calculation        |
| `update`                    | `(neuron: NeuronType) => void`                  | Updates neuron properties in real-time       |
| `updateSynapse`                    | `(synapse: SynapseType) => void`                  | Updates neurons synapse properties in real-time       |
| `getAfferents`                    | `() => [SynapseType]`                  | Returns a list of all afferent (incoming) synapses       |
| `getEfferents`                    | `() => [SynapseType]`                  | Returns a list of all efferents (outcoming) synapses       |

In order not to over-complicate the code and editor, we move with rates instead of static values that can be tuned using sliders. In most cases, you don't need this level of precision. There is also not much precision in the biological networks.
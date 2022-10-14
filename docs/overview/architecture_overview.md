# Implementation overview

The goal of FLUX is to utilize the mass-market hardware. The requirements are:
* cross-platform
* P2P connectivity
* performance
* working on mass-market machines

FLUX engine is implemented in `Typescript`, `Rust` and `WebGPU Shading Language`. It runs as a browser application, and utilizes `WebAssembly` and `WebGPU` standard, to access maximum performance. Being a browser app, it lets Flux be cross-platroms and communication centered from the ground up.
## Engine

Engine maintains a flat list of neurons, and connections between them. To keep this in memory, we utilize WebAssembly and Rust language, to optimize memory usage and be able to maintain a network of over 1000000 neurons on consumer machines. Engine optimally splits load between CPU, GPU and RAM, making the best of mass-market consumer machines.

![Architecture](../_media/architecture.svg)

Engine consists of 3 core parts:
* Neurons graph
* Computational core
* Event loop

Neurons graph is a flat list of all the neurons in the system, with their state, properties and an afferent synapses. It is implemented in `WebAssembly` and `Rust`, to guarantee the most optimal memory usage. This utilizes RAM. To save memory, we try to keep most of the numeric values in Uint8 type.

Computational core is where the neuronal processing goes on. It calculates ones neuron accepts a stimulation over synapses, and determines whether it needs to fire or not, by comparing its state and properties. with respect to time. This runs on the GPU, via `WebGPU` and `WebGPU Shading Language`.

Event loop is something that maintains the action potential processing and orchestrates the activity of the network. It is implemented in `Typescript`, which has an event loop implementation in its core, and utilizes CPU. Modern javascript interpreters like Googles V8 make sure that program runs on the performance close to compilable languages.

Knowing the fact that only 2% on brain neurons are actually active, to optimize memory usage of the pub-sub, the process of mounting and unmounting neurons can be dynamic. Our goal is to be able to run a network of 10 billion neurons on a regular laptop. And extend it by any number of devices on network with Flux Macro RTC protocol and/or add remote VPS servers on demand. So that researches are not limited in scale.

## Non-neuronal computations

Any system in the end is I/O, meaning it should at least accept input and generate output. Also it may need to run an off-circuit computations. Flux networks are extendable by nature.
This is intended to be created by community, so the performance is up to developers.
## Core components and terminology

### Circuit

Circuit is a functionally complete set of neurons and connections between them. It is the main building block of Flux artificial nervous systems. Circuit can contain nodes, like neurons, organs and another circuits and connections between them. Circuits can be saved to prototypes, grouped, nested, and inherited by another circuits. Intelligent circuit hierarchy can be beneficial in terms of network maintainability and growth.

### Neuron

Neuron is the basic computational unit of FLUX. It implements a leaky modulated integrate-and-fire neuron model, with habituation and potentiation properties. On firing neuron can either emit a single spike, or a spike train, depending on the threshold overshoot.

### Synapse

Synapse is the connection between neurons. Synapse weight determines the level of influence to the target neuron. Weight sign determines whether synapse is inhibitory or excitatory.
There are three types of synapses: direct, modulator and electric.
Synapse can be plastic, and change their weight in the process of Hebbian learning. Learning is mediated by modulation and firing activity.

### Organ

Organ is a code that runs alonglide with the engine, and utilize neurons as its input and output interface. Input organs accept signal in the form of its input neurons firing. Output organs generate signals in the form of spiking patterns via its output neurons.
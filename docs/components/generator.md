# Generator

What if you need to create some repetative circuit, lets say a reservoir of 100 randomly connected neurons. This is where generators kick in. Basically, generator is a function, that returns a json description of the circuit and lets you programatically define circuit nodes and connections between them.

Generator can accept settings to collect the user input and also has an access to the current network via Engine API.
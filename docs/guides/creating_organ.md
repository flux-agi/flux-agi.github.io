# Creating organ

In this guide, we will cover the process of creating an organ, that serves as an artificial muscle to control the servo motor of the Hexapod robot. Basically, as every output organ, it takes the signal in the form of spike trains, and computes it to the servo angle.

In will have 2 input neurons: one for muscle contraction, and another one for muscle expantion. Also it has an output neuron that propagates the resistance level.
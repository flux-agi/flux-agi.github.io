# Flux editor

Editor is designed to simplify the creation of the networks on any sizes, with scale in mind. By nesting and grouping circuits you can build interfaces of any complexity without sacrifising displayability. Lets dive into the main interface components. Our goal was to make ANS development simple and intuitive, esoecially to people familiar with regular IDEs like VSCode.

## Main Window
This is the main editor interface. 

### Top controls
Here you have a global controls, like managing your account and organization, create new circuits and access community explorer.

### Bottom controls
This lets you control the execution of the current ANS. 

### Circuit Tree
This displays the hierarchy of the current ANS. You can open sub-circuits in the new tabs or highlight it.

### Organs rack
Here you have a list of the organs, connected to the current ANS, and a corresponding organ UI and settings. Organs can be expanded or collapsed, or filtered from global to the current active tab.

## Editor
THis is the main ANS editing interface. This of it as Figma for designing nervous systems. 

### Right controls
There are three modes: edit mode, selection mode and comments mode.

Click on any place on the canvas to create new node: neuron, circuit or organ, and set corresponding properties.

To create a synapse, select a source neuron by clicking it, and then choose target neuron, this will open a new synapse dialog.

Right click on each node opens an edit dialog. You can also edit synapse by selecting two connected neurons.

You can remove nodes either from dialog menu, or by selecting nodes and hitting backspace. All the corresponding synapses will be removed.

Hold and drag nodes to arrange the network graph. You can also look for the guides to keep the structure organized.
## Explorer
In the explorer, you can access various community networks and organs and generators, and use it in your current ANS.

## Organization settings
Edit your organization page, access all your projects and manage members and access rights.

## Comments
You can leave comments at any place in the network graph. By clicking the comment, you can open a discussion thread.
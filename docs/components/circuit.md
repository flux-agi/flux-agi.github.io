# Circuit

A circuit is the collection of nodes (neurons, organs, other circuits) and connections between them. At the engine level, it is represented as a flat collection of neurons and synapses. Circuits can be grouped, nested, shared, and saved as prototypes. It is a potent tool for building deeply layered systems using the component design methodology. The root circuit is the top-level circuit, executed by the computational core of the engine.

## Display
Circuits are displayed as a set of neurons, arranged in a grid within a circuit boundary. Depending on the number of neurons, circuit thumbnails are displayed differently:

![Circuit](../_media/circuit_types.svg)

Input and output neurons are shown on the top and bottom of the thumbnail, respectively.
## Properties

| Name                             | Type                 | Default                        | Description                                                                                                                                |
| -------------------------------- | -------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `title`                    | `String` | ""                    | Display name of the circuit                   |
| `description`              | `String`             | ""                            | Markdown string                           |
| `isReusable`              | `Boolean`             | `false`                            | If true, circuit will appear in the explorer as a reusable component                           |
| `isShared`              | `Boolean`             | `false`                            | If true, circuit will be accessible by other users in the global explorer.                          |

## Creating circuits

You can create a circuit by choosing neurons with the selection tool and providing the circuit properties in the dialog:

Root circuits can be created by hitting `+ Create` in the editor header.

## Nesting circuits

Circuits can be the nodes of other circuits. We highly recommend keeping your circuits clean and self-descriptive by providing input and output neurons. The node tree of the root circuit can be viewed in the left pane. You can extend and collapse nodes, and open them in a new editor tab.

## Grouping circuits

Sometimes you need to have your circuits grouped, so that the changes on a single circuit will propagate to all others within the group. This can be done by selecting the circuit and entering the grouping mode. Another option: when cloning the circuit, set the `pair` option.
## Inheriting circuits

Once you create a circuit, in the settings, you can make it reusable:

Now it will be accessible in explorer, so you can use it as a prototype for other circuits. Just drag it to the editor pane, and choose whether you need it to be expanded or added as a node.

## Root circuit

Besides having the same set of features as a regular circuit, a root circuit includes additional properties, like global environment variables, that can be used inside its nodes.

## Circuit files

Circuit data is stored in the binary file with .flx extension. Files are well optimized to ensure optimal size and the ability to be transported to other systems. In the case of organ dependencies, they can also be ported as executables with wasm and loaded onto any standalone system.


# IBM-Hackathon-2020 summer jam
Following is an attempt at a built-in package for surface codes to be pushed in the topological codes package of qiskit.

The GraphDecoder class, located in fitters.py, constructs the graph corresponding to the possible syndromes of a quantum error correction surface code, runs minimum weight perfect matching (MWPM) on a subgraph of detected errors to determine the most probable sequence of errors, and then flips a series of qubits to correct them. Our surface code is structured as a square lattice with alternating X and Z syndrome nodes, as depicted below for `d=3` and `d=5`:

![Surface_Code_Diagram](https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs41534-018-0106-y/MediaObjects/41534_2018_106_Fig1_HTML.png?as=webp)

This surface code evolves over a specified number of time steps `T`, giving rise to a 3D syndrome graph of the syndrome node lattice over time. Virtual nodes, which are nonphysical but nevertheless error-inducing, alternate between X and Z nodes across the border of the surface code and are also included in the graph. Each edge weight is 1 to denote that adjacent syndrome nodes have a rotated Manhattan distance of 1. Below is an example of a 3D syndrome graph of X syndromes with `d=3` and `T=3`:

![X_d3_T3_plot](https://user-images.githubusercontent.com/42923017/86188148-b1c91080-bb0b-11ea-9012-abd3ac91210e.jpg)

We construct our 3D syndrome graph by specifying the coordinates of the syndrome and virtual nodes, connecting the lattice of syndrome nodes at a given time step, and connecting the lattices between adjacent time steps and virtual nodes to their adjacent syndrome nodes at all time steps. Then, a subgraph of detected syndrome errors is extracted, where we account for path degeneracy for the edge weights and clone virtual nodes to allow for multiple virtual node to syndrome node matchings.
Below is an example of this error subgraph with `node_set = [(0, 1.5, 0.5), (1, 1.5, 0.5), (1, 0.5, 1.5), (2, 0.5, 1.5)]`:

![X_d3_T3_error](https://user-images.githubusercontent.com/42923017/86188813-cefede80-bb0d-11ea-8355-4e6a2acf1b1f.jpg)

We run a MWPM on the subgraph to determine the most probable set of syndrome errors.
Below is an example of the MWPM for our error subgraph:
![X_d3_T3_match](https://user-images.githubusercontent.com/42923017/86188816-d1613880-bb0d-11ea-8125-a544fdf50f80.jpg)

Finally, we correct the syndrome errors through a series of qubit flips.

# Sources

## Rep Code
- [tutorial](https://qiskit.org/textbook/ch-quantum-hardware/error-correction-repetition-code.html#Lookup-table-decoding)


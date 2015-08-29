# B<sup>+</sup> tree visualization package
Drawing *beautiful* B<sup>+</sup> trees in `TeX` using the standard `TikZ` features is cumbersome.  
This package tackles this problem by providing macros to simplify drawing B<sup>+</sup> trees in `TikZ`.

The `btreevis` package utilizes the standard tree environment of `TikZ` for the placement of the B<sup>+</sup> tree nodes.  
The edges of the tree environment are disabled by the default B<sup>+</sup> tree style (`BTreeDefault`) because it does not suffice to get beautiful connecting edges. This is mostly because of the B<sup>+</sup> tree characteristics: a node has multiple pointers (and keys of course) each of which is connected to a node on the subsequent level of the B<sup>+</sup> tree.  

This package provides macros to generate the B<sup>+</sup> tree nodes in a simple way as well as macros to draw B<sup>+</sup> tree edges in a proper way.  

The explanation of the B<sup>+</sup> tree characteristics are skipped here. One may study B<sup>+</sup> trees e.g. on [Wikipedia](https://en.wikipedia.org/wiki/B%2B_tree).

# Example result
![alt text](https://github.com/danielkocher/btreevis/blob/master/example-beautiful-btree.png "An example of a beautiful B+ tree")

# Full Examples
The `btreevis` package consists of two full example `TeX` files to show how to use the package. These two files contain a full documentation describing each step separately (what is done and why it is necessary).

# Documentation
Some documentation is provided by this README althought internal macros and counters are skipped (by now). But a full and detailed documentation is located inside the `btreevis.sty` file.  
Moreover, there are two full documented example `TeX` files given, a simple and an advanced one, to should the usage of the `btreevis` package.  

## Required `TeX` packages
The following `TeX` packages are used and thus must be installed to use the `btreevis` package:

* `tikz`
* `ifthen`
* `etoolbox`

## Default styles
The defined styles provide all important properties for nodes and trees, respectively, such that the resulting B<sup>+</sup> tree looks beautiful. These styles may be overridden by own styles.

* `BTreeNodeDefault`
* `BTreeDefault`

See `btreevis.sty` for detailed information about the individual style properties and their respective purpose.

## Global macros

### `\SetNoOfPointersPerNode[1]`
Sets the globally defined variable (`\noOfPointersPerNode`) to the value supplied. It specifies to number of pointers per node (usually referred to as *m*) - not the number of keys but the number of keys + 1 per node.

**Arguments:**

1. Value to be assigned to `\noOfPointersPerNode`

### `\CreateBTreeNode[4]`
Generates the content of a complete B<sup>+</sup> tree node (in fact, a matrix of nodes). Values are supplied through a comma-separated list and the generated content is stored in a supplied variable.

For the comma-separated list, there are three cases:

1. If the list consists of less elements than (number of pointers per node - 1), the remaining key cells are left empty.
2. If the lists consists of more elements than (number of pointers per node - 1), the oversupplied elements are skipped/ignored.
3. In all other cases the number of elements is between (the number of pointers per node / 2) (in short: *m*/2) and the number of pointers per node (*m*).

The generated matrix of nodes is not named because this is not needed to connect the nodes. However, the nodes inside this matrix of nodes are named consecutively.

**Arguments:**

1. Number of the tree level of the node (used to name automatically)
2. Number of the node of the supplied tree level (argument #1)
3. Values as comma-separated list
4. Name of the variable the B<sup>+</sup> tree node content is stored to

### `\ConnectBTreeNodes[5]`
To generate beautiful B<sup>+</sup> trees, we need to disable the edges from parent of the tree environment of `TikZ` and use own macros the draw the edges between the nodes.

The `\ConnectBTreeNodes` macro is used to generate the edges between nodes of the B<sup>+</sup> tree. It connects all pointers of a supplied node on a supplied source (tree) level to all its child nodes of a supplied destination (tree) level with an arrow. The incoming arrows on any node of the subsequent (tree) level are located in the north center of the node (dependent on the number of keys a node holds - calculated automatically if `\noOfPointersPerNode` is set correctly).
This macro only works properly if the naming of the nodes is correct!

**Arguments:**

1. Number of source (tree) level
2. Number of source node (parent node)
3. Number of destination (tree) level
4. Number of (child) nodes to be connected with parent
5. Number of the first (child) nodes to be connected with parent

### `\ConnectBTreeLeaves[2]`
This macro is used to generate the edges between the leaf nodes of the B<sup>+</sup> tree. It connects all leaf nodes of a supplied (tree) level with dotted arrows. Because each B<sup>+</sup> tree node is a matrix of nodes, each arrow starts at `.east` of the rightmost node of the leaf (matrix) and ends at `.west` of the leftmost node of the next leaf (matrix) to the right.
This macro only works properly if the naming of the nodes is correct!

**Arguments:**

1. Number of the (tree) level of the leaf nodes
2. Number of leaf nodes to be connected

## Interaction with `TikZ`

### `tikzpicture` options

As described before, default styles are defined within the `btreevis` package to provide basic style information. The `BTreeDefault` style provides basic `tikzpicture` information (for more details see `btreevis.sty`).

If there are no special needs, it suffices to use the `BTreeDefault` style and define the style of the respective tree levels in the tikzpicture options to get a standard but beautiful B<sup>+</sup> tree.

**Example:**

    \begin{tikzpicture}[
      BTreeDefault,
      level 1/.style = {
        sibling distance = 22em,
        level distance = 6em
      },
      level 2/.style = {
        sibling distance = 8em,
        level distance = 6em
      }
    ] 

### Tree generation

The macros provided by the `btreevis` package are just to generate the content of a node (as a matrix of nodes). To keep it as simple as possible, the actual tree layout/drawing is done using the standard tree environment of `TikZ`. Hence, the content of all nodes of the desired B<sup>+</sup> tree must be generated upfront and are used as node contents afterwards.

**Example:**

    % Content level 0
    \SetNoOfPoinersPerNode{4}
    \CreateBTreeNode{0}{1}{{5}}{\levelZeroNodeOne}
    
    % Content level 1
    \CreateBTreeNode{1}{1}{{3}}{\levelOneNodeOne}
    \CreateBTreeNode{1}{2}{{7, 9}}{\levelOneNodeTwo}
    
    % Use tree environment to draw the nodes (without connecting arrows)
    \node[BTreeNodeDefault] {\levelZeroNodeOne}
    child { node[BTreeNodeDefault] {\levelOneNodeOne} }
    child { node[BTreeNodeDefault] {\levelOneNodeTwo} };

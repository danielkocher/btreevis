# btreevis - B+ tree visualization package

Drawing good-looking B+ trees in LaTeX using the standard TikZ/PGF features is cumbersome.  
This package tackles this problem by providing macros to simplify drawing B+ trees in TikZ.  
Moreover, by using this package, the drawings actually look like real B+ trees.  

This package utilizes the standard tree environment of TikZ for the placement of the B+ tree nodes.  
The edges of the tree environment are disabled by the default B+ tree style (BTreeDefault) because it does not suffice to get beautiful connecting edges. This is because of the B+ tree characteristics: a node has multiple pointers (and keys of course) each of which is connected to a node on the subsequent level of the B+ tree.  
There are two macros contained in the btreevis package solving this 'visualization issue' by generating proper connecting edges.

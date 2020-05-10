[TOC]

# Intro





# Statistical

* n~r~: number of tuples in a relation r.
* b~r~: number of blocks containing tuples of r.
* f~r~: blocking factor of r
    * i.e., the number of tuples of r that fit into one block.
* I~f~: tuples of r are stored together physically in a file, then:
* l~r~: number of bytes for a tuple in r
* V(A, r): number of distinct values that appear in r for attribute A; same as the size of A(r).
* SC(A, r): selection cardinality of attribute A of relation r ; average number of records that satisfy equality on A.
    * Sc(A, r) == nr / V(A, r)
# A* with dynamic operator reception

In some applications, some of the operators involved in A\* might not be relevant to reach goal state. If we somehow (machine learning etc.) sort the operators by their relevance, we can apply operators by giving priority to operators of high relevance. Therefore, the objective is to tweak the A\* algorithm such that it can cope with dynamic addition of operators to it’s operator set. This article discusses several approaches which preserve completeness and optimality of the algorithm while achieving our objective.

A\* algorithm
=============

A\* algorithm is a search algorithm which uses *f-cost*, sum of path cost and heuristic estimate, as it’s cost function and tries to achieve goal. If the heuristic estimate being used is admissible, then A\* algorithm finds optimal solution. In A\* algorithm, we use *open list* and *closed list* to keep track of unexplored and explored nodes respectively.

Dynamic A\*
===========

Dynamic A\* (D-A\*) is an algorithm which can cope with increasing number of operators. We will receive several *operator-chunk* which are set of some operators. Each time we consider a new *operator-chunk*, we add it to our set of operators *operator-list*. We’ll also assume that we’ll get indication, *halt-message*, indicating that no more *operator-chunk* will be received. In the following sub-sections, we will discuss several versions of D-A\*

Subspace Exhaustion D-A\*
-------------------------

-   In this version D-A\* algorithm (SED-A\*), we take latest *operator-chunk*, explore the search space until it’s exhausted, then take the next. We loop until we receive *halt-message* or expand the goal state.

-   In SED-A\*, instead of maintaining *open list* and *closed list*, we maintain *fresh list* and *stale list*.

-   In SED-A\*, as soon as we consider a new *operator-chunk*, we add all operators in *operator-list*. Also, we move *operator-chunk* to *fresh-operators* (we don’t add, we substitute).

-   States are operated upon sequentially, first we operate all states in *stale list* with *fresh-operators* and add all the newly generated states to *fresh list*. Once we exhaust *stale list*, we start operating states in *fresh list* with *operator-list*. We add all the newly generated states back to *fresh list* in that order so that states in *stale list* are sorted according to f-cost (for maintaining the optimality of the algorithm). Also we add all the operated states in *fresh list* to *stale list*.

-   Visited states are tracked (all visited states are in *stale list*) and no visited state is added to *fresh list*.

-   All the states in *fresh list* are sorted according to their f-value and operated accordingly.

-   Next *operator-chunk* is requested once the *fresh list* is exhausted.

Time Exhaustion D-A\*
---------------------

-   In this version of D-A\* algorithm (TED-A\*), we take an *operator-chunk*, explore the search space until it’s *operate-time* is exhausted, then consider the next. The *operate-time* represents how long to explore search space using the *operator-chunk* under consideration.

-   TED-A\* maintains following state lists:

    open list
    :   Used as a local open list (with respect to *operator-chunk*) while operating using an *operator-chunk*. The states in this list are sorted according to their f-values.

    stale list
    :   Used as a local (with respect to *operator-chunk*) closed list while using an *operator-chunk*.

    closed list
    :   It’s a permanent closed list.

    **Note:** During simulation (using an *operator-chunk*) all the explored nodes are moved to *stale list* (acting like closed list in A\*) and nodes are nodes are picked from *open list* (acting like open list in A\*).

-   We operate states from *open list* (if not already visited by current *operator-chunk*) using *operator-chunk* under consideration. Now there can be few cases:

    -   If *halt-message* is not received, the expanded node is added to *stale list*.

    -   If *halt-message* is received and the *operator-chunk* is not the last *chunk* (for details read points below), the expanded node is added to *stale list*.

    -   If *halt-message* is received and the *operator-chunk* is the last *chunk*, the expanded node is added to *closed list*.

    Whenever we consider the next *operator-chunk*, all the nodes in the
    *stale list* are filled back into *open list*.

-   In TED-A\*, we record all the *operator-chunk* and store the m<sup>th</sup> *operator-chunk* as m<sup>th</sup>_gen_operators. We’ve knowledge of what all states are operated with i<sup>th</sup>_gen_operators (We can do this by annotating each states with all operator-chunk applied to it).

-   There are two instances under which this algorithm halts:

    -   It receives a *halt-message* and *open list* is empty.

    -   It expands the goal state.

-   The *operate-time* here can be chosen as per choice and purpose of use. One can try to choose *operate-time* that has correlation with the degree of relevance of operators etc.

-   Just for clarification, every i<sup>th</sup>_gen_operators will be run for *operate-time* corresponding to it’s *operate-chunk*.

-   We assume atomicity of expanding a state with respect to *operator-chunk*.

Conclusion
==========

We discussed two A\* based techniques which can handle dynamic operator reception. These techniques can be useful when we learn about our operators dynamically. Also they are helpful when we have some relevance based sorting of our operators, since we’ll reach goal state faster in the new restricted search space.

* * *
Lashit Jain

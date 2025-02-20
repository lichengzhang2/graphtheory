# CIRCLE GRAPHS

## PERMUTATION REPRESENTATION

~~~python
#    a---c       path graph P_3  a---b---c
#  / |   | \     nodes: a, b, c
# b--+---+--b    edges: (a,b), (b,c)
#  \ |   | /   double perm (clockwise): [a,c,b,c,a,b]
#    a---c
#
#     c---a      cycle graph C_4
#   / |   | \    nodes: a, b, c, d
# b---+---+---b  edges: (a,b), (b,c), (c,d), (d,a)
# |   |   |   |  double perm (clockwise): [a,b,d,a,c,d,b,c]
# d---+---+---d
#   \ |   | /
#     c---a
~~~

## RECOGNITION

~~~python
from graphtheory.structures.edges import Edge
from graphtheory.structures.graphs import Graph

# TO DO
~~~

## GENERATORS

~~~python
from graphtheory.permutations.circletools import make_random_circle
from graphtheory.permutations.circletools import make_complete_circle
from graphtheory.permutations.circletools import make_path_circle
from graphtheory.permutations.circletools import make_cycle_circle
from graphtheory.permutations.circletools import make_2tree_circle
from graphtheory.permutations.circletools import make_ktree_circle
from graphtheory.permutations.circletools import make_star_circle

# perm has (double) numbers from 0 to n-1.
n = 10
perm = make_random_circle(n)
perm = make_complete_circle(n)   # make perm for complete graph K_n
perm = make_path_circle(n)   # make perm for path graph P_n
perm = make_cycle_circle(n)   # make perm for cycle graph C_n
perm = make_2tree_circle(n)   # make perm for 2-tree graph
perm = make_ktree_circle(n, n // 2)   # make perm for k-tree graph
perm = make_star_circle(n)   # make perm for star graph K_{1,n-1}

assert len(perm) == 2*n
assert sorted(perm) == sorted(2 * list(range(n)))
~~~

## FUNCTIONS

~~~python
from graphtheory.permutations.circletools import is_perm_graph
from graphtheory.permutations.circletools import circle2perm
from graphtheory.permutations.circletools import circle_has_edge
from graphtheory.permutations.circletools import circle_is_connected
from graphtheory.permutations.circletools import make_abstract_circle_graph

assert is_perm_graph([0, 1, 0, 1])   # P_2 graph
assert not is_perm_graph([4, 1, 0, 2, 1, 3, 2, 4, 3, 0])   # C_5 graph

# figure-8 graph (n=6)
double_perm = list("cdbcabedfeaf")
perm, n2l, l2n = circle2perm(double_perm)
assert perm == [2, 4, 0, 5, 1, 3]   # permutation graph

perm = [3, 1, 0, 2, 1, 3, 2, 0]   # C_4 graph
assert circle_is_connected(perm)
assert circle_has_edge(perm, 0, 1)   # O(n) time, O(n) memory
assert not circle_has_edge(perm, 0, 2)

# Note that if this test failed then the corresponding abstract graph
# still can be a perm graph but harder to detect.
assert is_perm_graph(perm)   # O(n) time, O(n) memory, weak test

G = make_abstract_circle_graph(perm)
assert isinstance(G, Graph)
assert G.v() == 4   # C_4
assert G.e() == 4   # C_4
~~~

## TRAVERSING

~~~python
from graphtheory.permutations.circlebfs import CircleBFS
from graphtheory.permutations.circledfs import CircleDFS

# Using BFS/DFS in O(n^2) time.
perm = [...]   # circle graph as a double perm (list)
source = ...   # starting node
pre_order = []
post_order = []
algorithm = CircleBFS(perm)
#algorithm = CircleDFS(perm)
algorithm.run(source,
    pre_action=lambda node: pre_order.append(node),
    post_action=lambda node: post_order.append(node))
# Results.
print(pre_order)   # node list
print(post_order)   # node list
print(algorithm.parent)   # BFS/DFS tree as a dict
print(algorithm.path(source, target)   # node list
~~~

EOF

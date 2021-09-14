Big O rankings
1. O(log n) - binary search
2. O(n) - simple search
3. O(n*log n) - quicksort
4. O(n^2) - selection sort
5. O(n!) - traveling salesman

                 Arrays          Lists    Hash Tables    Binary Search Tree
Search        O(log n)                                               O(log n)
Reading      O(1)          O(n)       O(1)              
Writing        O(n)          O(1)       O(1)                    O(log n)

6. breadth-first search = O(v+e)
> Check by level: 1st connections, then all second connections, so on
7. Dijkstras algorithm
> get distance of each child of first node
> then, do same for all children of children
> if new distance is lower, update the cost
> do this until you get to the destination
8. Greedy Algorithm - Also useful in life when assigning priorities: big rocks first
> get the best solution in the local space.
> if there is still room, add the next best
> stop before you go over
9. Dynamic programming
How much can you fit in your container of a fixed size?
> break down the container to incremental sizes. the decimal division depends on the items you are checking
> For each size, find the optimal item value that can fit it.
> for getting succeeding optimal item values, first check if the current highest value fits the size. If there is leftover, search previous optimal values for the best value for the remainder
> the item cannot be subdivided. but the size can be. use greedy if items can be subdivided / not integers
10. K-nearest neighbors
> compare the unknown item to the items most similar to it. it is probably closest to the one with the shortest distance, based on dimensions
> use pythagorean formula to calculate distance. can be applied to any number of dimensions
> Can also take average of the nearest to get a value

----

Binary search tree: add item to left node if smaller than parent, right otherwise

Inverted index: a hash table that records the location of specific terms

Fourier transform: given an item, identify and split its component parts

Parallel algorithms: run an algorithm across multiple processors

MapReduce: distributed algorithm. Map a function to each item in the array, then reduce all the results to a single answer (like a sum)

Bloom filters: gives a probably correct result vs hash table. false positives are possible (so fail safe)

HyperLogLog: gives an approximate number of unique elements.

SHA: hash function, useful for comparing two items without directly needing to compare them, like for passwords or large files.

Simhash: like SHA but can tell you if your hash is close to the actual you want.

Diffie-Hellman: uses private and public key to decode a hash. encrypt with public, decrypt with private

Linear programming:  maximize an outcome given a set of constraints. Uses simplex algorithm.





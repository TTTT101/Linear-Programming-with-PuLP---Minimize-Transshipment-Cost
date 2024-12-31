# Minimize-Transshipment-Cost---Linear-Programming-with-PuLP

The transshipment problem is a transportation problem, in which goods are moved from suppliers to customers through intermediate nodes (or transshipment points). These transshipment points allow minimizing the total transportation cost by finding the nodes that create the most cost-effective route. 

Nodes represent suppliers, warehouses, distribution centers, consumers, etc. Constraints include but not limit to supply capacities and demand. The objective function is to minimize the total transportation cost that satified the constraints. 

In this post, I am using Linear Programming to solve the problem.

## Data Description

The data table consists of 5 columns and 21 rows that show car shipping and production cost. 'Start' column represents the origin cities and 'End' column represents the destination cities. 'cost' indicates the shipping cost from start to end cities while 'prod_cost' is the production cost at the origin cities.

<img width="273" alt="image" src="https://github.com/user-attachments/assets/860ccfdb-6cd9-4633-8ed0-56e48f786226" />

City #1 (Detroit) can produce upto 6500 cars and city #2 (Dallas) can produce upto 6000 cars. City #3 (LA) must receive 5000 cars, city #4 (Atlanta) must receive 4000 cars, and city #5 (St. Louis) must receive  3000 cars. Cars may be shipped through city #6 (Denver) and #7 (Memphis). Maximum 2200 cars can be shipped directly from any one to another city.

## Network Design Visualization

<img width="631" alt="Screenshot 2024-12-31 at 12 20 24 AM" src="https://github.com/user-attachments/assets/2dfb510e-efc0-4aa1-aaad-8725b219d425" />

Specifically,
- The nodes are labelled using the cities’ name and S is the starting node
- The lower bound is zero car. The upper bound is 2,200 cars.
- The arcs cost: each arc cost is the sum of the shipping and production costs (Graph 2). We add an additional amount of $2000 into all the arcs that leave Detroit (Det) and $1800 into all arcs that leave Dallas (Da). By adding a dummy node (S), we can present the arcs values as in Graph 1. In which, the first value inside the parenthesis is the cost and the second value is the arc capacity.
- The net supplies of all nodes in cars are: 6500 (Det), and 6000 (Da). The net demands in cars are: 5000 (LA), 4000 (Atl), and 3000 (St. Lu). Memphis (Mem) and Denver (Den) has zero net supply nor demand.
- Since the demand is smaller than the supply, we create a dummy node that connect to the supply nodes Det and Da for exceed supply.

## Algorithm Implementation

The formula that corresponds to this model is:
Denote 𝑥𝑖𝑗 the number of cars to be shipped from node i to node j

<img width="686" alt="Screenshot 2024-12-31 at 12 22 27 AM" src="https://github.com/user-attachments/assets/bb6bf37c-599c-4d27-b4d4-463f53b214a5" />

The above formulation is equivalent to:

min 2800𝑥𝐷𝑒𝑡,𝐿𝐴 + 2600 𝑥𝐷𝑒𝑡,𝐴𝑡𝑙 + 2300𝑥𝐷𝑒𝑡,𝑆𝑡𝐿𝑢 + 2300𝑥𝐷𝑒𝑡,𝐷𝑒𝑛 + 2200𝑥𝐷𝑒𝑡,𝑀𝑒𝑚 + 2300𝑥𝐷𝑎,𝐿𝐴 + 2000𝑥𝐷𝑎,𝐴𝑡𝑙 + 2000𝑥𝐷𝑎,𝑆𝑡𝐿𝑢 + 2200𝑥𝐷𝑎,𝐷𝑒𝑛 + 2000𝑥𝐷𝑎,𝑀𝑒𝑚 + 300𝑥𝐷𝑒𝑛,𝐿𝐴 + 800𝑥𝐷𝑒𝑛,𝐴𝑡𝑙 + 500𝑥𝐷𝑒𝑛,𝑆𝑡𝐿𝑢 + 500𝑥𝐷𝑒𝑛,𝑀𝑒𝑚 + 800𝑥𝑀𝑒𝑚,𝐿𝐴 + 100𝑥𝑀𝑒𝑚,𝐴𝑡𝑙 + 100𝑥𝑀𝑒𝑚,𝑆𝑡𝐿𝑢 + 500𝑥𝑀𝑒𝑚,𝐷𝑒𝑛

𝑠. 𝑡 𝑥𝐷𝑒𝑡,𝐿𝐴 + 𝑥𝐷𝑒𝑡,𝐴𝑡𝑙 + 𝑥𝐷𝑒𝑡,𝑆𝑡𝐿𝑢 + 𝑥𝐷𝑒𝑡,𝐷𝑒𝑛 + 𝑥𝐷𝑒𝑡,𝑀𝑒𝑚 ≤ 6500 (1)

𝑥𝐷𝑎,𝐿𝐴 + 𝑥𝐷𝑎,𝐴𝑡𝑙 + 𝑥𝐷𝑎,𝑆𝑡𝐿𝑢 + 𝑥𝐷𝑎,𝐷𝑒𝑛 + 𝑥𝐷𝑎,𝑀𝑒𝑚 ≤ 6000 (1)
𝑥𝐷𝑒𝑡,𝐷𝑒𝑛 + 𝑥𝐷𝑎,𝐷𝑒𝑛 + 𝑥𝑀𝑒𝑚,𝐷𝑒𝑛 − 𝑥𝐷𝑒𝑛,𝐿𝐴 − 𝑥𝐷𝑒𝑛,𝐴𝑡𝑙 − 𝑥𝐷𝑒𝑛,𝑆𝑡𝐿𝑢 = 0 (2)
𝑥𝐷𝑒𝑡,𝑀𝑒𝑚 + 𝑥𝐷𝑎,𝑀𝑒𝑚 + 𝑥𝐷𝑒𝑛,𝑀𝑒𝑚 − 𝑥𝑀𝑒𝑚,𝐿𝐴 − 𝑥𝑀𝑒𝑚,𝐴𝑡𝑙 − 𝑥𝑀𝑒𝑚,𝑆𝑡𝐿𝑢 = 0 (2)
𝑥𝐷𝑒𝑡,𝐿𝐴 + 𝑥𝐷𝑎,𝐿𝐴 + 𝑥𝐷𝑒𝑛,𝐿𝐴 + 𝑥𝑀𝑒𝑚,𝐿𝐴 = 5000 (3)

𝑥𝐷𝑒𝑡,𝐴𝑡𝑙 + 𝑥𝐷𝑎,𝐴𝑡𝑙 + 𝑥𝐷𝑒𝑛,𝐴𝑡𝑙 + 𝑥𝑀𝑒𝑚,𝐴𝑡𝑙 = 4000 (3)

𝑥𝐷𝑒𝑡,𝑆𝑡𝐿𝑢 + 𝑥𝐷𝑎,𝑆𝑡𝐿𝑢 + 𝑥𝐷𝑒𝑛,𝑆𝑡𝐿𝑢 + 𝑥𝑀𝑒𝑚,𝑆𝑡𝐿𝑢 = 3000 (3)
∑ 𝑗∈{𝐷𝑒𝑡,𝐷𝑎,𝐷𝑒𝑛,𝑀𝑒𝑚},
0≤𝑥𝑖𝑗 ≤2200 ∀𝑖,𝑗

## Result

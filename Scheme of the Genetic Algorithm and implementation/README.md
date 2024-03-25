
### 1. Description of the task

*Zheki and Lule are passionate fishermen. Every time they go fishing, they reload Zheki's Golf because they can't decide what fishing gear is important to them and what isn't. In order to solve this problem, Lule remembered that he could create a web application that implements a genetic algorithm that will find a solution to their combinatorial problem. He found a couple of videos on YouTube about genetic algorithms and started implementing his idea. In the following guidelines, the steps are shown as Lule went about implementing his task:*

1. He made a list of the things he takes fishing and attached to each thing a certain value and a certain number representing the weight of how much space the item would potentially take up inside the Golf's trunk (since it is not very big).

2. He created the HTML interface and programmed the logic for selecting elements from the specified list, as well as sending the same data to other parts of the program code via JSON format.

     When the form with the selected elements is submitted, it passes the following JSON record:
     ```
     [
         {
             "name": "map",
             "weight": 9,
             "value": 150
         },
         {
             "name": "compass",
             "weight": 13,
             "value": 35
         },
     ]
     ```
    
3. Also, he implemented the potential methods ```updateChart''' to display the average goodness function of an individual generation on a graph and the graphical method ``displayBestCombination'' which displays the best combination of things to take fishing and their total value.

Since Lule is a frontend, he started to implement the genetic algorithm in the file ``geneticAlgo.js''. He implemented the ```GA''' class and its constructor representing a genetic algorithm. However, he is stuck and doesn't know how to further implement the genetic algorithm, as well as how to communicate with the rest of the programming code he has made so far. Help Lulet to implement his idea to the end and to solve the problem of overloaded Golf.


### 2. Task

From the given description of the problem, it is possible to define a genetic algorithm with a goodness function described by the following expression:

#### 2.1 Term
*If N subjects (y0,...,yn-1) are given, each of them with weight W(i) and value P(i), i = 0,...n-1, it is necessary to determine which objects can be selected so that the sum of their weights does not exceed the given M, and that their total value P is maximal. Then the solution of the n-tuple X = (x0,..., xn-1) {0,1}n is such that:*

![Image 7](./Images/07.png)


#### 2.2 Scheme of the Genetic Algorithm and implementation

Within the file ``geneticAlgo.js'' and the class ``GA'' it is necessary to implement the genetic algorithm as follows:

1. The incoming JSON object coming from the form needs to be encoded into a binary representation that will faithfully represent the combination of the chosen things with a defined goodness function based on the chosen things
   
2. It is necessary to create a method for creating the initial population, which will be in the following format:
   
```
[ // Example of one individual from the population
     {
         "generation": 0,
         "genes": [0,1,0,0,0,1,1,1,1,0,0,1],
         "fitness": 240
     },
     .....
     }
]
```

3. A method for the selection of potential parents is needed - ***Tournament selection*** (See appendix 2 at the end)
   
4. It is necessary to create a method for the RECOMBINATION operator (crossing) of parents - ***Crossing at one point*** (See appendix 3 at the end) - The probability of the occurrence of crossing is ***75%*** (need to be implemented)
   
5. It is necessary to create a method for the MUTATION operator of the newly created genetic material - ***Bitflip Mutation*** (See Appendix 4 at the end) - The probability of the occurrence of a mutation is ***10%*** (need to be implemented)
   
6. It is necessary to create a method for selecting a new population - ***MI+ALFA*** method (See appendix 5 at the end)

After the implementation of the genetic algorithm, it is necessary to call and pass the parameters within the implemented logic to the methods ``updateChart'' to display the values on the graph and ``displayBestCombination'' to display the best combination given by the genetic algorithm.

**Note**: When creating the initial population and after creating new genetic material that will represent the new population, it is necessary to check the conditions of the density function. If an individual has been created that has a combination that gives a weight sum greater than the allowed one, it is necessary to punish such an individual and set its density function to 0. By means of punishment, the number of individuals that give bad solutions is reduced and thus we reduce the transmission of bad genetic material in subsequent generations.


**Note**: The above solution must be implemented in the file ``geneticAlgo.js'' using ES6 JavaScript syntax with an emphasis on the object-oriented paradigm. It is also necessary to study the rest of the program code, modules, methods and their parameters in order to better understand the given task.


***Appendix number 6 shows how the solution to the task should look if all elements from the defined form are selected.***


### 3. It is necessary to study:

- Literature at the following link (Chapters 1, 2 and 3): [Link](http://www.zemris.fer.hr/~golub/ga/ga_skripta1.pdf)

- Watch: https://www.youtube.com/watch?v=cxweR4i0ejA

- Watch: https://www.youtube.com/watch?v=L--IxUH4fac

- Watch: https://www.youtube.com/watch?v=uQj5UNhCPuo
  
- Preparation for LV2


### Genetic algorithm

#### Coding problems in the search space
    
There is no one way to create a genetic algorithm; it is created by combining different available operators so that it is suitable for solving a certain optimization problem. A classical genetic algorithm has a binary representation, proportional fitness selection, low mutation probability, emphasis on genetically inspired recombination, and generational selection of candidates to be passed on to the next generation. The first step in the construction of any evolutionary algorithm is to determine the candidate representation for the solution. This includes the definition of the genotype and the mapping from the genotype to the phenotype (switching from the problem space to the space where a computer-intelligible search can be performed). The following representation is most often used:

- binary representation: genotype shown as a string of binary digits (Figure 1)

![Image 1](./Images/01.png)

#### Initial population

The process starts with a set of individuals called a population. Each individual is a potential solution to an optimization problem. An individual is characterized by a set of parameters (variables) that we call genes. Genes are strung together to form a chromosome.

![Image 2](./Images/02.png)

#### Goodness function

The goodness function determines how well an individual is able to compete with other participants within the population (in other words, the goodness function represents the quality of a certain solution). The probability that an individual will be selected as a potential parent to create new genetic material is based on the value of the goodness function.

#### Selection

In the selection phase, the fittest individuals (usually two or more) are selected to create new genetic material for the next generation. Two pairs of individuals (or more) are selected based on the results of their goodness function. High fitness individuals have a better chance of being selected for reproduction.

- Selection methods: Random selection, tournament selection, roulette wheel.

#### Crossing

Crossover is the most important stage in the genetic algorithm. For each pair of parents to be mated, a random crossover point (or multiple points) within the gene array is taken. From the reproduction of the parents, two children (or more, but usually two) are created, which will represent new genetic material (new solutions) within the new population. Offspring are created by exchanging the genes of the parents at a specific crossover point.

![Image 3](./Images/03.png)

#### Mutation

In a particular new offspring, some of the genes of the newly created children may be exposed to mutation with a small random probability. This implies that some bits in the bit string can be reversed. Mutation occurs to maintain diversity within a population and prevent premature convergence.

- Example: Bitflip mutation

![Image 3](./Images/04.png)

#### Selection of the best quality individuals

After new individuals are generated, which should represent a new generation of genetic material, it is often the case that the old and the newly created population are combined into one large sequence, from which the N best individuals with the highest value of the goodness function are taken. Those N best individuals (from the old and new population) represent a new generation of the population from which potential parents are again selected who have a high function of goodness for the creation of new and better genetic material.

A population has a fixed size. As new generations are formed, individuals with the least function of goodness are discarded, making room for new offspring. Repeating these steps above encourages the creation of new individuals in a generation that are better and potentially better than the previous generation.

#### End

The algorithm stops working if the population has converged (or the maximum number of iterations has been reached) (it does not produce an offspring that is significantly different from the previous generation). At this point, we can conclude that the genetic algorithm has provided a number of solutions to our optimization problem.


### 4. Appendix:

#### 1. Genetic algorithm pseudocode:

```
INITIALIZE population with random candidates;
CALCULATE the goodness function for each individual candidate:
while not satisfied STOP CONDITION until
     Select parents;
     Perform the CROSSING of the parents and GENERATE the offspring;
     MUTATE offspring;
     EVALUATE new offspring;
     SELECT the individuals that will be in the next generation;
END
```
![Image 6](./Images/06.png)

#### 2. Pseudocode for Tournament selection:

```
SELECT K individuals from population N by random selection;
SELECT individual K1 with the highest P value from K individuals;
SELECT the next individual K2 with the highest P value from K individuals;
IF K1 == K2 repeat the previous step;
RETURN K1, K2;
END
```

#### 3. Crossing at one point:
If the value of the random variable R has a value less than the defined value for performing the operation, the RECOMBINATION operation will be performed, otherwise, the genes of the children will be identical to the genes of the parents
![Image 3](./Images/03.png)

#### 4. Pseudocode for bitflip mutation:
```
for element N in array M:
     generate random value R in the range [0...1];
     if R < DEFINED VALUE:
         IF N is 0:
             set N to 1;
             END;
         IF N is 1;
             set N to 0;
             END;
END;
```
![Image 4](./Images/04.png)

#### 5. Pseudo code for population MI + ALFA:
```
TAKE the old population N and the new population M and concatenate them into an array NM;
SORT array NM by goodness function P;
TAKE the N best from array NM and place them in array K;
RETURN array K as new population;
END;
```

#### 6. Solution of the task

If the genetic algorithm is well implemented, the value of the goodness function can vary between 900-1000 for all selected elements

![Image 8](./Images/08.png)
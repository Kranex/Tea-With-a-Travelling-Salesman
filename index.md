---
abstract: |
    The Travelling Salesman Problem (TSP) is a famous problem in
    combinatorial optimization with a wide range of application in the real
    world. Its importance has been recognized many years ago, as well as its
    complexity, and to this day no effective solution method has been found.
    This article focuses on the question "How do Genetic Algorithms perform
    when applied to The Travelling Salesman Problem?". It addresses the
    topics of constructing and applying a Genetic Algorithm (GA) to the
    Travelling Salesman Problem, evaluating its efficiency in comparison to
    the traditional methods and investigating its performance as the TSP
    expands. The GA's advantages and disadvantages are addressed in order to
    discuss the performance of the GA, when applied to the TSP. Conclusions
    were made on the performance of the GA. When applied to a TSP of a small
    scale the GA tends to converge to the optimal tour in every run. On a
    TSP of a larger scale, the GA is able to find a near optimum solution in
    every run within minutes.
author:
- |
    Karin Dijkstra, Anastasia Dryaeva, Rick Ploeg & Oliver Strik\
    Propaedeutic Project: A11\
    Rijksuniversiteit Groningen
date: June 16 2017
title: |
    Tea with a Travelling Salesman:\
    A Discussion of Genetic Algorithms
---

Introduction
============

The Travelling Salesman Problem (TSP) is a famous problem in
combinatorial optimization, with the goal to minimize the total travel
cost (be that time, distance, or fuel expenses) for a salesman who has
to visit a finite number of cities and return to the starting point,
given the costs associated with travelling from one city to another.

At first the problem may appear uncomplicated and straightforward,
however its simplicity is only an illusion. Although the exact origins
of the TSP are unclear, it can be dated back to at least 1832, yet to
this day, there has been no effective solution method found for a
general case. In fact, a solution of this problem would resolve the
infamous P vs. NP Millennium Prize problem. As a result, the Travelling
Salesman Problem still remains one of the most intensely studied
problems in computational mathematics and in the recent years has
sparked interests in newer fields, such as computer science.

Throughout the years, different methods and algorithms have been
developed for the TSP. Many of the traditional methods involve
heuristics such as Nearest Neighbor or Nearest Insertion, or
relaxations, for example the Hungarian Algorithm. However, these do not
necessarily give the optimal, and for the latter sometimes not even a
feasible solution. Moreover, these methods become insufficient when the
problem takes a larger scale. An example of a more advanced, newer
method would be the application of a Genetic Algorithm. Genetic
Algorithms (GAs) are algorithms designed to simulate natural evolution:
they are based on the concept of 'survival of the fittest'.

This article will address the topic of applying a Genetic Algorithm to
the Travelling Salesman Problem, focusing on the following question:
"How do Genetic Algorithms perform when applied to the Travelling
Salesman Problem?". In order to provide an answer to this question, the
TSP will be discussed in more detail in section
[2](#IntroTSP){reference-type="ref" reference="IntroTSP"}. After that,
the general concept of a GA will be explained in section
[3](#WisGA){reference-type="ref" reference="WisGA"}, including an
insight into the construction of such an algorithm for the TSP. In
section [4](#SimpleTSP){reference-type="ref" reference="SimpleTSP"} the
constructed GA will be applied to a small problem of 6 cities, where the
efficiency of the algorithm will be evaluated in comparison to the
traditional methods. Section [5](#largeTSP){reference-type="ref"
reference="largeTSP"} will focus on investigating the performance of the
GA on an expanded problem of 26 cities, including a discussion of the
issues encountered, followed by a detailed analysis of the parameters
within the constructed GA in section [6](#Analysis){reference-type="ref"
reference="Analysis"}. In the final section conclusions will be made,
addressing the initial research question.

What is the Travelling Salesman Problem? {#IntroTSP}
========================================

In 1832, a German handbook for travelling salesmen was published, titled
*Der Handlungsreisende - wie er sein soll und was er zu tun hat, um
Aufträge zu erhalten und eines glücklichen Erfolgs in seinen Geschäften
gewiß zu sein - Von einem alten Commis-Voyageur* (translated from
German: "The Travelling Salesman - how he should be and what he has to
do, in order to obtain commissions and to be assured of great success in
his business - by an old Commis-Voyageur")[@tspbook]. The book contains
a definition and an elaborate description of the problem, as well as the
first ever recorded reference to it as "the Travelling Salesman
Problem", but does not give a mathematical treatment for it.
Nevertheless, the author clearly recognizes its importance, writing the
following passage:

"Business leads the traveling salesman here and there, and there is not
a good tour for all occurring cases; but through an expedient choice and
division of the tour so much time can be won that we feel compelled to
give guidelines about this. Everyone should use as much of the advice as
he thinks useful for his application. We believe we can ensure as much
that it will not be possible to plan the tours through Germany in
consideration of the distances and the traveling back and forth, which
deserves the traveler's special attention, with more economy. The main
thing to remember is always to visit as many localities as possible
without having to touch them twice." [@tspbook]

Indeed, the main objective of the TSP is to economize, be that
minimizing time, distance, fuel costs or any other expenses a travelling
salesman may encounter. However, nowadays the applications of the TSP
are recognized to reach far beyond travelling salesmen business.
Naturally, the TSP arises in planning, transportation and logistics,
problems like designing delivery routes or bus schedules are classic
examples of that. There are also numerous applications in everyday life,
for example for tourists wanting to visit many historical sites in a
limited time, or a person running errands around town. Some other more
curious and non-intuitive areas where the TSP has found applications are
genetics, manufacturing, telecommunications, and neuroscience [@tspbook]
[@ORlecture]. Seeing as the TSP is so widely applicable in many areas of
both industrial and day-to-day life, an effective method of solution is
highly desired, yet for centuries many mathematicians have struggled to
find one.

A solution to any Traveling Salesman Problem can without a doubt be
obtained by simply listing all the possible tours and selecting the best
one. This would indeed always work, as every scenario with Euclidian
distances will always have one or more minimal cost tours. However, it
must be noted, that the number of possible tours in a TSP is:
$$\frac{(n-1)!}{2}.$$ In this equation $n$ is the number of vertices
(referred to in this work as cities). This comes from the fact that at
every city the travelling salesman visits, he faces a choice of the
remaining cities for his next destination, and the distance between
every pair of cities is the same in each direction, forming what is
called a 'symmetric TSP'. Hence, for 5 cities one would find 12 possible
tours, 181440 tours for 10 cities, 608225502004416000 tours for 20 and
so on. For an asymmetric case, the results are even more dramatic, since
the number of possible tours becomes $(n-1)!$.

Computing all the possible tours on a TSP becomes a huge burden, even
when the problem in question is still relatively small. That is why
mathematicians and computer scientists have been working on finding more
efficient methods to solve the problem. Many methods have been
developed, but not one of them is perfect and without drawbacks. One of
the relatively successful recently developed methods is solving the
problem via Genetic Algorithms.

What is a Genetic Algorithm? {#WisGA}
============================

![The basic structure of a Genetic
Algorithm.[]{label="struct"}](GA_Structure){#struct width="50%"}

A Genetic Algorithm is an algorithm that uses natural selection, or
survival of the fittest, to find a solution to a problem. All Genetic
Algorithms follow a similar, if not identical structure (figure
[1](#struct){reference-type="ref" reference="struct"}). However, the
elements of this structure are highly customised for each specific
application. Before going into detail about these elements, the concept
of chromosomes and fitness must first be introduced.

Chromosomes are one of the most important parts of a GA, their only
purpose is to store the potential solutions of the problem. How
chromosomes store these solutions is up to the constructor of the GA,
commonly however they are a single string that might represent a binary
number, or possibly a list of the problem's variables. The components
that make up the chromosome are referred to as genes, for example in a
chromosome described as a binary number, each 1 or 0 is a gene. In the
research algorithm a chromosome is a tour. Each city in the given
Travelling Salesman Problem is a gene, and the order of the genes in the
chromosome describes the order that the cities should be visited in.
Another thing to note about chromosomes is their fitness. The fitness
function of a GA describes how fit a particular chromosome is as a
solution. How this fitness works and how fitnesses are compared, is
again nearly entirely up to the constructor of the GA.

Initialisation of the Pool
--------------------------

The first step in the execution of a Genetic Algorithm is the generation
of the pool. The pool is a collection of chromosomes that can be
described as the "current generation". This pool is initialised by
randomly generating chromosomes until the pool is filled, in the case of
the research algorithm, this was done by shuffling the list of cities to
construct a tour. Shuffling a list that contains each city only once
eliminates the possibility of missing or having duplicate cities within
the chromosome which is one of the constraints of the Travelling
Salesman Problem. The number of chromosomes in the pool is usually a
fixed number, however adaptive pool sizing does exist [@populationsize].
However only fixed pool size was used in this research.

The Main Loop
-------------

This next section comprises the main body of the GA. Every time this
loop is completed, a generation has passed. Throughout the next
sections, the methods used in the research algorithm will be used as
examples.

### Parent Selection {#parents}

Parent selection is the first step of each generation. The flow of
Figure [1](#struct){reference-type="ref" reference="struct"} suggests
that all parents are selected before Crossover, however in reality it is
easier and potentially more efficient to do both steps concurrently,
thus you select two parents, breed them (Crossover) and repeat until a
new pool is constructed.

Parents can be selected in many ways, however usually the fitness of the
parents is used in some way to select them. For example, in the research
algorithm a method called roulette wheel selection was used [@Slides],
in this method all chromosomes in the pool take up an area on a wheel
proportional to the fitness of the chromosome. This created some issues
within the research algorithm as the fitness function returns the total
distance of the chromosome. Thus, the smaller the distance, the better
the chromosome. In order to get the correct probability of selection,
the fitnesses needed to be 'inverted'. This was done with two equations:
$$inv = minFit + maxFit$$ $$p = (inv - fitness)/totalInvFitness.$$ Where
$inv$ is the 'Inversion constant', $minFit$ is the smallest/best fitness
in the pool, $maxFit$ is the largest/worst fitness in the pool, $p$ is
the probability of selection, $fitness$ is the fitness of a chromosome,
and $totalInvFitness$ is the sum of all inverted fitnesses in the pool.

Using these equations, the probability of selection for each chromosome
in the pool could be calculated, thus allowing for parents to be
selected from the pool using these probabilities.

### Crossover

*Step 1: The children start as duplicates of the parents that have been
selected. Also a range of genes is randomly selected, in this example
genes 3 to 5 and they are shown in green.*
$$Parent\, 1: [0,3,\colorbox{green}{2,5,1},4]$$
$$Parent\, 2: [1,3,\colorbox{green}{5,0,4},2]$$
$$Child\, 1: [0,3,\colorbox{green}{2,5,1},4]$$
$$Child\, 2: [1,3,\colorbox{green}{5,0,4},2]$$ *Step 2: From the second
child, the genes contained in selected range of the first parent are
removed. The same thing is done to the first child using the second
parent.* $$Parent\, 1: [0,3,\colorbox{green}{2,5,1},4]$$
$$Parent\, 2: [1,3,\colorbox{green}{5,0,4},2]$$
$$Child\, 1: [-,3,\colorbox{green}{2,-,1},-]$$
$$Child\, 2: [-,3,\colorbox{green}{-,0,4},-]$$ *Step 3: The remaining
genes in the children must be moved outside the range selected in step
1, without changing the order the are in.*
$$Parent\, 1: [0,3,\colorbox{green}{2,5,1},4]$$
$$Parent\, 2: [1,3,\colorbox{green}{5,0,4},2]$$
$$Child\, 1: [3,2,\colorbox{green}{-,-,-},1]$$
$$Child\, 2: [3,0,\colorbox{green}{-,-,-},4]$$ *Step 4: Finally the
genes selected in the first parent are moved into the now open space in
the second child. The same goes for the second parent and first child.*
$$Parent\, 1: [0,3,\colorbox{green}{2,5,1},4]$$
$$Parent\, 2: [1,3,\colorbox{green}{5,0,4},2]$$
$$Child\, 1: [3,2,\colorbox{green}{5,0,4},1]$$
$$Child\, 2: [3,0,\colorbox{green}{2,5,1},4]$$

Crossover (also known as breeding) is when two chromosomes, called
parents, are 'crossed' together to produce one or more children. There
are many ways of doing this, from slicing the parents in half and
swapping the ends, to more complicated methods like the one used in the
research algorithm shown in Figure
[\[fig:cross\]](#fig:cross){reference-type="ref" reference="fig:cross"}.
These children are then used to replace the current pool, thus forming
the next generation.

A certain percentage of the population in the pool, called elites, is
not replaced during crossover. This fraction of the pool consists of the
fittest chromosomes, thus allowing these chromosomes to survive into
future generations. The only way an chromosome can be removed from the
elites, is if a fitter chromosome is found to replace it.

### Mutation

$$Before: [0,3,\colorbox{green}{2},5,1,\colorbox{red}{4}]$$
$$After: [0,3,\colorbox{red}{4},5,1,\colorbox{green}{2}]$$

Every child produced in Section [3.2.2](#crossover){reference-type="ref"
reference="crossover"} has a chance of mutation, this is when the
chromosome is subjected to a small but random change. In the case of the
research algorithm, a specific form of mutation called swap mutation was
used where two genes within the chromosome are randomly selected and
then swapped, this is shown in Figure
[\[fig:mut\]](#fig:mut){reference-type="ref" reference="fig:mut"}. This
particular method of mutation does not invalidate the tour a chromosome
describes, since it does not cause duplicate cities nor does it remove
cities. Also, in the research algorithm, a chance of mutating multiple
times was used, with each subsequent mutation being less and less
probable.

Termination
-----------

The final step in the Genetic Algorithm is termination. At the end of
every generation, it is questioned whether the GA has met a specific
criterion, if it has then the GA terminates and the best solution in the
gene pool is the solution you finish with, otherwise it continues and
begins a new generation. The criteria for termination can be nearly
anything and are usually rather specific to the problem, however there
are some generic ones, for example having found a solution that is
fitter than a given solution or having completed a given number of
generations.

The Research Framework
----------------------

The Genetic Algorithm that was constructed for this research is divided
into two components, the first being the Genetic Algorithm Framework,
and the second being a script which integrates with the framework
[@Code]. The GAFramework is actually quite a simple program, it was
designed for the research, however even though the research was
specifically on the Traveling Salesman Problem, the GAF is capable of
being used for any problem. It simply passes data to the scripts and
provides the scripts some generic functionality such as a chromosome
object and a loop function. The script is the main bulk of the program
as it is responsible for defining and solving a specific problem. The
script constructed to solve the research problems was designed to be a
generic script for Traveling Salesman Problems, when provided with a raw
text file containing either a distance matrix or the coordinates for
each city, it will proceed to solve the problem regardless of size. The
script can also be given variables such as the size of chromosome pool,
or the percentage of the pool to make elites.

Simple TSP {#SimpleTSP}
==========

In this section, a small symmetric TSP consisting of only 6 cities will
be solved using some of the simpler and more effective of the
traditional methods: first by applying a modified Hungarian Algorithm,
then via a Binary Integer Program (BIP) in Excel. After that, the
problem will be solved via the GA constructed in section
[3](#WisGA){reference-type="ref" reference="WisGA"}. In the last part of
this section the performance of all 3 of the methods will be discussed.

Problem Definition
------------------

As mentioned before, the small problem consists of 6 cities, named A to
F, with the following coordinates:

  ------------ ------------ -------------
  A (3, 0.5)   C (6, 3.5)   E (8.5, 6)
  B (1, 3.5)   D (2.5, 7)   F (10, 1.5)
  ------------ ------------ -------------

![*6 city map*[]{label="6citymap"}](6citymap){#6citymap height="7.5cm"}

These can be represented on a 2-dimensional map as shown in figure
[2](#6citymap){reference-type="ref" reference="6citymap"}. Knowing the
coordinates of the points, the distance between every pair of points can
be calculated, using the formula $XY=\sqrt{(x_1-y_1)^2+(x_2-y_2)^2}$ for
points $X(x_1, x_2)$ and $Y(y_1, y_2)$. The results can be represented
as a matrix, where element in column $X$ and row $Y$ (as well as element
in row $X$ and column $Y$) represents distance $XY$. For the given
points A to F the matrix is shown in figure
[3](#distancematrix){reference-type="ref" reference="distancematrix"}.
In the future, it will be referred to as 'the distance matrix'.

![*The distance
matrix*[]{label="distancematrix"}](distancematrix){#distancematrix
height="6cm"}

Note that the diagonal, which corresponds to the distance between the
city and itself, is not defined, since such a tour is not allowed.

Hungarian Algorithm {#HA}
-------------------

The Hungarian Algorithm is a method designed for the assignment problem,
which is a relaxation of the TSP. A modified version of it, also known
as the Matrix Reduction method, or Little's Algorithm, was introduced by
John D.C. Little in 1963, in his article titled *An Algorithm For The
Traveling Salesman Problem*. We will now proceed to solving our 6-city
problem, while simultaneously explaining the algorithm in use.

[Step 1: Row and column reduction]{.underline}

Begin with the distance matrix (figure
[3](#distancematrix){reference-type="ref" reference="distancematrix"}).
Identify the smallest element in every row, and subtract it from every
element in that row; then do the same for every column. The resulting
matrix will have a zero in each row and each column. As such, the
original distance matrix reduces as shown in figure
[4](#dmreduced){reference-type="ref" reference="dmreduced"}.

![*Reduced distance matrix*[]{label="dmreduced"}](dmreduced){#dmreduced
height="6cm"}

In Little's article, he elaborates on why this works, explaining:

"If a constant is subtracted from each element of the first row (of the
distance matrix), that constant is subtracted from the cost of every
tour. This is because every tour must include one and only one element
from the first row. The relative costs of all tours, however, are
unchanged and so the tour which would be optimal is unchanged. The same
argument can be applied to the columns." [@little]

[Step 2: Penalty calculation]{.underline}

For each zero in the new matrix note the sum between the minimum element
in the row and the minimum element in the column of that zero (omitting
the zero itself). That number is called the penalty associated with the
zero. Therefore, the penalty associated with the zero in position AB is
0.63, since the smallest element in row A is 0.63, and smallest element
in column B is 0. Similarly, the penalty of the zero in position BD is
1.21, and so on. The complete matrix displaying all the penalties is
shown in figure [5](#dmpenalties){reference-type="ref"
reference="dmpenalties"}. It is interesting to note, that although
Little's work describes the same process, it does not refer to those
numbers as 'the penalties'.

![*Reduced distance matrix with
penalties*[]{label="dmpenalties"}](dmpenalties){#dmpenalties
height="5.9cm"}

[Step 3: Row and column elimination]{.underline}

Select the zero with the highest penalty. If there is more than one zero
with the same penalty, select arbitrarily. The row and column of that
zero can be eliminated from the matrix, and its position noted down as
part of the optimum tour. In our case the zero in position BD has the
highest penalty. Therefore, BD is the first definitive road in the final
optimum tour that we are working towards. Hence it can be eliminated
from the matrix, as shown in figure
[6](#dmelimination2){reference-type="ref" reference="dmelimination2"}.
Note that in the new matrix, element DB becomes undefined, since it is
no longer possible for that road to be selected.

![*Elimination of the selected road from the
matrix*[]{label="dmelimination2"}](dmelimination2){#dmelimination2
height="5.8cm"}

[Step 4: Repeat steps 1 to 3]{.underline}

For the new matrix, repeat all the steps from the beginning: first row
and column reduction, then penalty calculation, and row and column
elimination, until reaching a 2x2 matrix. If no mistakes have been made,
3 elements will be zeros with penalty of zero, and the remaining element
undefined. In such a matrix, only two roads are possible, and they will
be the last entries on the list of roads, forming a complete tour. Our
6-city problem eventually reduces to the following 2x2 matrix:

![*Last matrix of the
algorithm*[]{label="2x2matrix"}](2x2matrix){#2x2matrix height="2.5cm"}

With the following assignments already made:

B $\rightarrow$ D,

A $\rightarrow$ B,

C $\rightarrow$ A,

E $\rightarrow$ F.

We complete this list by selecting assignments F $\rightarrow$ C and D
$\rightarrow$ E from figure [7](#2x2matrix){reference-type="ref"
reference="2x2matrix"}, which gives us the following tour:

A $\rightarrow$ B $\rightarrow$ D $\rightarrow$ E $\rightarrow$ F
$\rightarrow$ C $\rightarrow$ A

![*Final tour*[]{label="6citytour"}](6citytour){#6citytour
height="7.5cm"}

Looking back at the distance matrix (figure
[3](#distancematrix){reference-type="ref" reference="distancematrix"}),
the total distance of this tour is 26.95. The full step-by-step process
can be seen in the appendix (appendix
[8.1](#FullHA){reference-type="ref" reference="FullHA"}).

Binary Integer Program {#BIP}
----------------------

The optimum TSP tour can also be determined using a Binary Integer
Program (BIP) formulation of the problem [@ORlecture], as shown below.

Let $x_{ij}$ be the distance between cities $i$ and $j$, given by the
distance matrix in figure [3](#distancematrix){reference-type="ref"
reference="distancematrix"} and let $y_{ij}$ be the binary decision
variable such that:

$$y_{ij}= 
\begin{cases}
1, & \text{if road $ij$ is part of the tour}\\
0 & \text{otherwise}
\end{cases}$$

Then, the BIP formulation of the TSP is:

Minimize:

$z = \sum_{i} \sum_{j} x_{ij}y_{ij}$

such that:

$\sum_{i} y_{ij}=1$ for all $j$,

$\sum_{j} y_{ij}=1$ for all $i$,

And $y_{ij}$ binary for all $i$, $j$.

This can be implemented into Excel and solved using the Excel-solver
function. Since nowhere in the current formulation of the problem there
is a constraint that requires the solution to be one single connected
tour, it is highly likely that the solution given by Excel will contain
subtours. However, the TSP does not allow for subtours, so they need to
be eliminated with additional constraints.

Using the distance matrix from figure
[3](#distancematrix){reference-type="ref" reference="distancematrix"}
for the $x_{ij}$ values, and after manually eliminating all subtours,
the BIP gives the solution $z=26.95$, with tour A $\rightarrow$ B
$\rightarrow$ D $\rightarrow$ E $\rightarrow$ F $\rightarrow$ C
$\rightarrow$ A, which is the same result as the one obtained via the
Hungarian Algorithm in section [4.2](#HA){reference-type="ref"
reference="HA"}.

Genetic Algorithm
-----------------

Given the distance matrix in figure
[3](#distancematrix){reference-type="ref" reference="distancematrix"}, a
reasonable poolsize and generation number, for example 200 and 1000
respectively, the GA is able to find the same solution for the problem
as the BIP and the Hungarian Algorithm every time.

The choice of poolsize is almost arbitrary, constrained only by a rather
vague restriction of being 'reasonable'. So, naturally, one might wonder
what that means. For a small problem like the one in question this
choice is fairly simple. For 6 cities, the GA can produce $6!=720$
possible tours, which includes duplicates that start from different
cities and ones that go in reverse directions. Removing those leaves
$\frac{6!}{2\cdot{6}}=\frac{5!}{2}=60$ possible tours (recall the
$\frac{(n-1)!}{2}$ formula). Hence, even with a poolsize as small as 60,
an optimal solution is almost guaranteed in the initial pool already
(almost, due to the randomness at the core of a GA). Increasing the
poolsize will improve those chances, however if it is set too high there
are some disadvantages. Section [6](#Analysis){reference-type="ref"
reference="Analysis"} of this article will explain in more detail the
benefits of increasing the poolsize, as well as the drawbacks.

Performance Evaluation
----------------------

When looking at the efficiency of methods, the main factor to consider
is the time taken to reach the optimum solution. In the case of the TSP,
an optimum is not always possible, so it is sensible to consider just a
reasonable solution. Luckily, with this problem, all 3 methods in
question give the optimum solution.

Solving a TSP with the Hungarian algorithm takes a substantial amount of
time, especially if it is done by hand. This is due to the fact that
every newly reduced matrix has to be re-written every time to perform
the next step (see appendix [8.1](#FullHA){reference-type="ref"
reference="FullHA"}). Additionally, manual computations are prone to
mistakes and miscalculations. As such, for the simple TSP it took a
total of 2 hours to reach the optimum solution, excluding the 40 minutes
it took to manually construct the distance matrix. This is a substantial
amount of time, considering the problem consists of only 6 cities.
Arguably, it could've taken less time to approach it with brute force:
list all the tours and select the lowest one.

The Binary Integer Program, done in Excel, found that same solution in 2
seconds, but the preparations took approximately 30 minutes. These
include formulating the problem with initial constraints and adding
extra subtour elimination constraints where necessary.

The GA found the solution to this problem in under 2 seconds, which
suggests that it is most efficient. However, the process of writing the
program is by far the longest, spreading over several days, indicating
that the GA is actually the most inefficient out of the 3 methods.

It must be noted that these values are not an objective measure of the
efficiency of the methods. Surely, for each of the methods there are
ways to decrease the time. For example, if the Hungarian Algorithm is
not done manually, but instead programed on a computer, a significant
amount of time can be saved, since the computer does calculations faster
and does not make mistakes, or if the Genetic Algorithm is written by an
expert, it may only require several hours of work, rather than several
days. Therefore, the values give above are superficial and should not be
taken as an accurate measure of efficiency, but rather a mere
representation.

From this, the conclusion can be made, that in the case of a relatively
small problem and easy access to a computer (with Excel-solver), the
most efficient method is the BIP. However, as the problem gets larger,
Excel-solver becomes insufficient, as it accepts only a limited number
of variables. When that threshold is exceeded, the GA becomes most
efficient. Additionally, if there is a large number of problems, the GA
is more efficient than the BIP as well, because the same GA can work on
any TSP, whereas a BIP needs a change of constraints for every
individual one. The Hungarian Algorithm is never efficient, unless no
computer is available for use.

How does the GA perform on a TSP of a larger scale? {#largeTSP}
===================================================

Considering the characteristics of a Genetic Algorithm, one can assume
that they are not the standard method to use on problems of such a small
scale as the problem discussed in the previous section, since manual or
traditional methods can give the solution as well. GAs hold their real
value in problems of a larger scale. Here they are capable of giving a
feasible solution, where most other methods fail to give one or are just
highly inefficient. The constructed GA has proved itself capable of
dealing with a simple TSP containing six cities, but how does it perform
on a TSP of a larger scale? This section will discuss the answer to that
question, explaining the obstacles encountered along the way.

The details of the TSP, such as the number of cities and their
locations, will be addressed in section [5.1](#tsp){reference-type="ref"
reference="tsp"}. Then in section [5.2](#perf){reference-type="ref"
reference="perf"}, the performance of the GA on this TSP will be
discussed. Since the total of possible tours was relatively low for the
simple TSP, discussed in the previous section, the GA was capable of
finding the optimal tour in every run. In this larger TSP however, the
number of tours is significantly larger, which meant that the GA did not
find the optimal tour in every run. This problem is known as premature
convergence and is the topic of the section
[5.3](#premc){reference-type="ref" reference="premc"}. A possible method
to prevent premature convergence is the topic of section
[5.4](#incest){reference-type="ref" reference="incest"} and in the last
section the conclusions are discussed.

A TSP containing 26 cities {#tsp}
--------------------------

For this expansion it was decided to extend the problem to 26 cities. To
make the problem more realistic, these 26 cities were selected to be
located throughout the Netherlands.

  ---------------------
  1 Amersfoort
  2 Amsterdam
  3 Apeldoorn
  4 Arnhem
  5 Assen
  6 Breda
  7 Den Haag
  8 Den Helder
  9 Eindhoven
  10 Emmen
  11 Enschede
  12 Groningen
  13 Haarlem
  14 Heerenveen
  15 Heerlen
  16 's-Hertogenbosch
  17 Leeuwarden
  18 Lelystad
  19 Maastricht
  20 Middelburg
  21 Nijmegen
  22 Rotterdam
  23 Tilburg
  24 Utrecht
  25 Venlo
  26 Zwolle
  ---------------------

[\[26cities\]]{#26cities label="26cities"}

![image](26cities){width="0.8\\linewidth"} [\[map1\]]{#map1
label="map1"}

The 26 cities are listed in alphabetical order together with a map of
the Netherlands (figure [\[map1\]](#map1){reference-type="ref"
reference="map1"}), that marks all of their locations. The objective is
thus to find the shortest route that visits all of these cities and
returns to the starting point afterwards. The total number of possible
tours here is calculated by the equation:
$$n = \frac{(26-1)!}{2} =  7.755605e+24.$$ This number is significantly
larger than the 60 possible tours for the simple TSP, discussed in
section 2. This is then also the reason why most other methods fail to
give a solution or are just inefficient. The manual methods and
traditional methods, discussed in the previous section, are excellent
examples. The Hungarian Algorithm already took 2 hours to perform on a
TSP, containing only six cities and even though it did give the optimal
solution in the end, it is highly inefficient to apply to this TSP
because of the time it would take. In addition, this algorithm is a
manual method, which makes it susceptible for human errors. The BIP also
fails to give a solution, because the number of integer variables
exceeds the limit of the Microsoft Excel linear solver. Even without
this limit of variables, a BIP would take a long time to construct,
considering all the possible subtours that would have to be added to the
program as constraints in order to make sure that the resulting tour
meets the criteria, set by the TSP. All of these subtours have to be
excluded by manually adding these constraints, which makes the BIP
timewise an inefficient method. Therefore it is necessary to turn to
other methods, such as Genetic Algorithms. Even though they are not
bound to give the optimal solution, they are at least capable of giving
a suitable tour that meets the criteria, set by the TSP.

The performance of the GA {#perf}
-------------------------

The first observation, when the GA was applied to the expanded TSP, was
that the time for one run of the program had increased. Depending on the
settings, such as the number of generations and the poolsize, the
program now takes approximately a minute. Since one chromosome now
consists of not 6, but 26 genes, it makes sense that the time for one
run increased. One minute is still a relatively short amount of time to
find a solution. Besides the short time, another benefit is that the GA
did not need reconstructing in order to be applied to this expanded TSP.
Because of the way the basic structure was programmed, this GA can be
applied to any TSP, that sparks your interest given that you provide the
necessary data, which consists of the number of cities and their
distances to each other. For other methods, like the Hungarian Algorithm
or the BIP, this does not hold.

r0.5

![image](1406tour){width="50%"}

[\[1406tour\]]{#1406tour label="1406tour"}

The next observation was that, while the GA still converged to a
solution in every run, these solutions differed from each other. They
did not show the same tour nor did they share the same total distance.
The solutions seemed to span a reach of approximately 100 kilometers,
the lowest solution having a total distance of 1406 kilometer. The tour,
belonging to this solution of 1406 kilometers, is shown in figure
[\[1406tour\]](#1406tour){reference-type="ref" reference="1406tour"}.
During the analysis of the constructed GA, the program never gave a tour
with a lower distance than this 1406 kilometers. We therefore believe
this solution could well be the optimal tour however, due to the large
amount of possible tours, this cannot be said with full certainty.

In order to find out why the GA gave multiple solutions, a new variable
was introduced to the program. This variable, named *leetChanges*,
counts the number of times the best chromosome in the pool, meaning the
chromosome with the lowest total distance, is changed. Additionally the
program also outputs when the last elite change has occurred. The
conclusion, drawn from analyzing this new variable, was that the program
reached convergence too fast. The last change of the best solution in
the pool nearly always occurred in the first quarter of the number of
generations. For example, if the number of generations was set to a 1000
then in generation 200 the best solution was last changed. This means
that in generation 201 up until 1000 the best solution always remained
the same tour. However, this solution was not necessarily the tour with
the lowest total distance, since for one run the solution came out with
a total distance of 1547 kilometers and for another run the total
distance was 1422 kilometers. Therefore the program does not converge to
the optimal solution every time. The program suffered from premature
convergence, which is the topic of the next section.

Premature Convergence {#premc}
---------------------

r0.5 ![image](PrematureConvergence)
[\[PrematureConvergence\]]{#PrematureConvergence
label="PrematureConvergence"}

Premature convergence is when the GA converges too early, to a local
optimum [@Slides]. It can be visualized by figure
[\[PrematureConvergence\]](#PrematureConvergence){reference-type="ref"
reference="PrematureConvergence"}. This graph, representing a
one-dimensional problem, shows multiple peaks, several being local
optima and one being the global optimum. When the program converges to a
local optimum, it is unlikely to get past that local optimum to a better
solution. Due to the concept of "survival of the fittest", the best
solution in the pool has the highest chance of being selected as a
parent. If that best solution is in the neighborhood of a local optimum,
then the majority of its children will also be in the neighborhood of
that local optimum. In the following crossovers, a pool is generated
with more and more chromosomes that are clustered around this local
optimum. In other words, the selected parents cannot create children
that are superior than they are themselves. Therefore the GA will not
get past the local optimum during crossover. Moreover, the population
loses its diversity as the program goes through the set number of
generations, this is believed to be the main reason for the occurrence
of premature convergence [@Premconvergence][@Popdiv][@Congress].

To obtain a chromosome, which is better than the current best solution
in the pool, mutation is the only hope. As already mentioned by
equation, there are 7.755605e+24 possible tours if duplicates and
mirrored solutions are excluded. The GA however, does not necessarily
exclude duplicated and mirrored tours, therefore the number of tours
that the GA accounts for is: 26! = 4.0329146e+26. The size of this
number makes the chance of randomly mutating a tour to a better solution
than a local optimum very small. Even changing the rate of mutation,
thus making the mutation more frequent and aggressive, will not prevent
premature convergence. It is therefore also a frequent problem,
encountered when applying GAs to optimization problems.

Incest Prevention {#incest}
-----------------

There are different methods of dealing with premature convergence. H.
Pandey, A. Chaudhary and D. Mehrotra discuss 24 methods in their article
*A comparative review of approaches to prevent premature convergence in
GA*[@Premconvergence]. In this section one of those methods will be
discussed, namely incest prevention. Incest prevention is preventing two
identical parents from breeding. During a crossover where two identical
solutions are selected as the parents, two children will be created that
are both identical to their parents and to each other. By constantly
breeding with two identical parents, the generated pool quickly loses
its diversity, which is the main cause of premature convergence. Which
part of the parents is selected does not influence the outcome of such a
crossover. Take for example a crossover between two identical parents,
both containig six indices:

$$\textit{Parent 1} =  [0,1,2,3,4,5]$$
$$\textit{Parent 2} =  [0,1,2,3,4,5]$$

In the first crossover a part of the first parent is selected and the
remaining indices are provided in the order they appear in the second
parent. This could happen in the following way:

$$\textit{Parent 1}: [-,1,2,3,-,-]$$ $$\textit{Parent 2}:[0,-,-,-,4,5]$$

This leads to: $$\textit{Child 1}: [0,1,2,3,4,5]$$

From this first child, it can be easily seen that the second child will
also be identical to the parents and its sibling. However, this only
happens, when both of the parents are completely identical, meaning that
both their tour and starting location have to be the same. For example a
crossover between the parents, depicted below, does not necessarily lead
to a child that is identical to the parents, even though they share the
same tour:

$$\textit{Parent 1} =  [0,1,2,3,4,5]$$
$$\textit{Parent 2} =  [2,3,4,5,0,1]$$ In the first crossover, where a
subset from the first parent is selected and the remaining parts of the
chromosome are given by the second parent, this could happen:

$$\textit{Parent 1}: [-,1,2,3,-,-]$$
$$\textit{Parent 2}: [-,-,4,5,0,-]$$ This leads to:
$$\textit{Child 1}: [4,1,2,3,5,0]$$

Even though both parents contained the same tour, their child does not
share it. Incest prevention therefore focuses on only eliminating
crossovers between two identical parents. Incest prevention therefore
focuses on only eliminating crossovers between two identical parents.

In order to implement incest prevention in the GA, an adjusted form of
tournament selection was used. The first parent is still selected by
means of probability based on its fitness. However, the second parent is
now selected by a tournament of a set number of chromosomes. The winner
of the tournament is the chromosome that is most different to the
already selected first parent. If all candidates in the tournament are
identical to the first parent, the program will dismiss the tournament
and start a new one. This process goes on until a different chromosome
is found.

The result of implementing this into the GA was that it increased the
time for one run of the program. With the same settings, the time about
doubled. This large increase can only be explained by the tournaments,
since they were the only addition to the GA. This gives a helpful
insight in the GA, since it apparently has to run a high amount of
tournaments. This means that in most of the tournaments candidates are
selected that are identical to the already determined first parent.
Although the incest prevention itself works, the GA still converges to
local optima. The incest prevention therefore did not prevent premature
convergence and since it causes a large increase in time, it is not
recommended to use within this GA and has not been used in the next
section.

Further expanding the TSP
-------------------------

The GA is more than capable of dealing with a further expansion of the
TSP, since when it contained 26 cities it gave solutions in
approximately a minute. Therefore in this section, the GA will be
applied to a TSP containing 50 cities, still all located throughout the
Netherlands. Here the number of possible tours is the extremely high
value of: $$n = \frac{(50-1)!}{2} = 3.0414093e+62.$$ To put this number
in perspective, there are more possible tours for this problem than
there are atoms in the earth. Moreover, the GA itself accounts for even
more tours, namely $50! = 3.0414093e+64$. Considering that traditional
methods, like the Hungarian Algorithm or the BIP, were concluded to be
unsuitable for the TSP consisting of 26 cities, they can be concluded
even more unsuitable for this TSP, containing 50 cities.

The first observation to note is again an increase in time. Where the GA
took 1 minute to complete the set number of generations for a TSP of 26
cities, the GA now takes 6 minutes with the same settings. Yet 6 minutes
is a very short amount of time if one considers the number of
possibilities, given in the previous paragraph. The GA still suffers
from premature convergence and in order to work around that issue, the
GA was set to run 600 times, saving the final solutions to an external
file. Out of all these runs, the best solution, given by the GA, was a
tour of 1623 kilometers. This tour is shown in figure
[\[1623tour\]](#1623tour){reference-type="ref" reference="1623tour"}.
Because of the large number of possible tours, it cannot be said with
certainty that this tour is in fact the global optimum.

![image](1623tour) [\[1623tour\]]{#1623tour label="1623tour"}

Conclusions
-----------

This section has dealt with the question: How does the GA perform on a
problem of larger scale? This problem contained 26 cities located
throughout the Netherlands. The GA was still capable of converging to a
solution however, these solutions are not necessarily the optimal tour.
In other words, the program suffered from premature convergence. One
method was examined to prevent this from happening, but without success.
It is also outside of the range of this research to prevent the
premature convergence. While it is true that the GA will sometimes give
a tour that is not the global optimum, the chance of selecting a better
tour by random choice is very very low due to the sheer amount of
possible tours.

Analysis {#Analysis}
========

The constructed Genetic Algorithm takes as input amongst other things a
certain amount of parameters for a particular few functions inside the
script. An example of such a parameter would be the number of
generations the algorithm will run for. So far in other parts of this
work, the value of these parameters were chosen arbitrarily, without any
concern or second thoughts. In this section however, the effect of
different values for these parameters will be observed and discussed.
There are multiple parameters at play in the GA of which we will discuss
the four main ones, namely: the poolsize, the amount of generations, the
percentage of elites and finally the rate of mutation. Furthermore, the
effects of the parameters will be judged on a few properties: The time
taken to finish, the actual quality of the objective value and its
consistency.

The means by which the effects of the parameters are observed is in
essence very straight-forward. The GA has been run thousands of times of
which every subsequent trial differs from the previous one only in a
small change in one of the parameters. For example, the poolsize has
been set to 10 and afterwards increases by 10 for every iteration until
a certain criterion is met. The way this has been accomplished is by
running a for-loop on a system that keeps changing a parameter and then
running the GA while the results of the GA are forwarded to an external
file. From the extracted data a multitude of statements can be made
about the properties of the GA. A simple example of such a statement
would be the correlation between poolsize and total time taken. From a
different and more practical perspective, one could wonder if and how to
change the parameters for the optial performance of the GA.

The Poolsize
------------

To analyse the effect of changes in the poolsize on the program, the GA
was executed with a poolsize of 10 to 1000 in steps of 10. However,
since the GA still relies on random events at its core, the GA was not
simply run once for a specific poolsize but 15 times instead. This to
was to significantly reduce the chance of getting irregular results. Of
these 15 trials, the average is taken of their objective value and other
interesting statistics such as the time it took, while also selecting
the most-and least optimal one for later reference.

![*The Objective Value vs. The Poolsize*[]{label="OVP"}](OVP){#OVP
height="6cm"}

The actual graphing of the data generated can be seen in figure
[9](#OVP){reference-type="ref" reference="OVP"} and it's immediately
very clear that the 'objective value' converges towards 1406 which is
the assumed optimal tour in this TSP. The noteworthy thing here is that
an increase in poolsize has minimal effect on the expected objective
value, but more on the spread of the points.

Now let's take a look at the graphing of the poolsize against the time
taken. Figure [10](#CTP){reference-type="ref" reference="CTP"} follows a
surprisingly smooth curve which clearly is not linear, but it's hard to
tell what relation it follows. The current guess is that it is a
quadratic relation, but this will be investigated in a program designed
for curve-fitting.

![*The Computation Time vs. The Poolsize*[]{label="CTP"}](CTP){#CTP
height="6cm"}

While the seemingly quadratic relation has not been proven via a logical
statement using the properties of the code, it can be seen in figure
[11](#LPPC){reference-type="ref" reference="LPPC"} that the data so far
does fit a quadratic relation very nicely. Maybe even more convincing is
what happens in the next figure [12](#LPPP){reference-type="ref"
reference="LPPP"} where the curving-software is asked to predict the
graph when only giving a very small part of it. The accuracy of this
prediction is very convincing for the case of a quadratic relation and
even though it could very well turn out to be a very weak 4th power
relation, it is assumed to be quadratic and treated as such.

![*The quadratic fit*[]{label="LPPC"}](LPPC){#LPPC height="6cm"}

![*The predicted fit*[]{label="LPPP"}](LPPP){#LPPP height="6cm"}

A noteworthy property of these and the upcoming graphs is that they are
very spiky, even after having taken an average. These spikes are in
essence a reminder that the GA is a program that depends on randomness.
Furthermore, the fact that the GA depends on randomness is exactly the
reason why those spikes will never disappear. They might decline in
amplitude and reduce their frequency, but since they are products of
random events, their absence can never be guaranteed. Just like it will
also be possible for any amount of coins to all toss heads, no matter
how many there are.

One way to reduce the spikes would be to repeatedly run the GA and
consider the whole data-set instead of just a singular value. Another,
more efficient way, is to optimize the input parameters. In the case of
poolsize it can already be seen that an increase reduces the spikes in
the objective value.

The amount of generations
-------------------------

The way that the effect of the generations parameter has on the
performances of the GA has been analysed much like that of the poolsize
and the first part is the effects of changes in the amount of
generations versus the the time spent computing of which the graph can
be seen in figure [13](#CTG){reference-type="ref" reference="CTG"}.

![*The Computation Time vs. The amount of
Generations*[]{label="CTG"}](CTG){#CTG height="6cm"}

The relation between the two can clearly be seen to be proportional.
This makes sense intuitively since if the amount of generations would be
doubled and the amount of calculations per generation would remain the
same, then the total amount of calculations and therefore the time
needed to do the calculations would double as well, hence the linear
relation.

Figure [14](#OVG){reference-type="ref" reference="OVG"} yields something
interesting, because in contrary to the same graph of the poolsize, it
does not reduce its spikes when the amount of generations increase.

This can be explained by considering that after a certain point the GA
will have converged to some optimum and further generations will only
have a negligible effect on finding a better optimum. Instead the GA
will stick with the optimum it has found for the rest of the extra
generations.

![*The Objective Value vs. The amount of
Generations*[]{label="OVG"}](OVG){#OVG height="6cm"}

The percentage of elites
------------------------

The effects on the program of different values of the percentage of
elites has been analysed by considering how the computing time and the
objective value behave and more specifically, the quality of the
objective value and its standard deviation.

It can be seen in figure [15](#OVEP){reference-type="ref"
reference="OVEP"} that the relation between the objective value and the
elites percentage is quite stable. That is, if the outer edges are not
considered because both extremes result in very poor convergence.

![*The Objective Value vs. The Percentage of
Elites*[]{label="OVEP"}](OVEP){#OVEP height="6cm"}

The reason that the side with too small a percentage of elites converges
significantly worse than that with other values for this parameter, is
because the program is effectively completely missing out on elites
which are a vital part of the GA. Then the question arises why it does
not work with higher values for the percentage of elites, this is
because the elites are not susceptible to change over the course of the
generations. In other words, the GA barely gets to apply the very reason
why it is even considered: a gradual increase of the quality of the
solution over the course of many generations by combining and mutating
the current generation.

Figure [16](#CTEP){reference-type="ref" reference="CTEP"} shows a linear
but decreasing relation between the percentage of elites and the
computing time. This happens since the elites are not susceptible to
change and therefore the GA will skip more and more of the computations
the larger the percentage of elites becomes. Thus one should be careful
about the illusion that a higher percentage of elites leads to a faster
program. Instead the program simply does less calculations.

![*The Computation Time vs. The Percentage of
Elites*[]{label="CTEP"}](CTEP){#CTEP height="6cm"}

Finally, figure [17](#SDEP){reference-type="ref" reference="SDEP"} shows
the standard deviation of the objective value for different values for
the percentage of elites. It can be seen that the percentage of elites
obtains optimal consistency around 30 percent of the poolsize.

![*The Standard Deviation vs. The Percentage of
Elites*[]{label="SDEP"}](SDEP){#SDEP height="6cm"}

The Rate of Mutation
--------------------

When considering the rate of mutation in the program, it is intuitively
clear that this will have a minimal effect on the time needed to get the
solution. However, it might deliver more interesting effects on the
objective value and its standard deviation. The statement about the time
needed can quickly be verified with figure
[18](#CTRM){reference-type="ref" reference="CTRM"}, and indeed there it
can be seen that there is no notable effect on the computation time.

![*The Computation Time vs. The Rate of
Mutation*[]{label="CTRM"}](CTRM){#CTRM height="6cm"}

Figure [19](#OVRM){reference-type="ref" reference="OVRM"} shows the
plotting of the objective value versus the rate of mutation. It can be
seen that there seems to be a very slight decreasing factor there. But
more notably is the little dip it attains around a rate of mutation of
30 percent. It does not look like much however, it should be kept in
mind that the minimum this graph could ever obtain is 1406 and since
this little dip comes consistently close to that particular value it is
a very significant drop.

![*The Objective Value vs. The Rate of
Mutation*[]{label="OVRM"}](OVRM){#OVRM height="6cm"}

The drop around the 30 percent mark can perhaps better be considered in
Figure [20](#SDRM){reference-type="ref" reference="SDRM"} concerning the
standard deviation versus the rate of mutation, and much like the
previous graph there is a very notable drop around this value for the
rate of mutation. The existence of this dip also immediately explains
the dip in the previous graph since the program tries to get to 1406 all
the time, but at these values it can do so more consistently hence the
lower average of the objective value.

![*The Standard Deviation vs. The Rate of
Mutation*[]{label="SDRM"}](SDRM){#SDRM height="6cm"}

Conclusions of the analysis
---------------------------

At the beginning of the this section, three main properties were
mentioned to be judged: the time taken to finish, the actual quality of
the objective value and its consistency.

After having seen all the graphs and relations so far, some conclusions
can be made with respect to the preferable settings for the optimal
experience while handling the GA. For starters, to gain access to the
better solutions in the solution space it is recommended to invest in
both the poolsize and the amount of generations to overcome the
threshold both of those parameters have at the lower values.
Furthermore, the percentage of elites should not be in either of its
extreme ends for this goal.

If the goal is to increase the potency of the GA while limiting the
extra added time needed to finish, it is recommended to invest in the
poolsize up until the slope of the quadratic relation catches up to that
of the amount of generations and from there invest in the generations.
This will be especially effective in bigger problems since for smaller
problems chances are that there is no need to be careful about the time
needed since it is negligible anyway.

Finally, considering the consistency of the GA, it already has been
mentioned that both the percentage of elites and the rate of mutation
give optimal consistency around a value of 30. Additionally, considering
the poolsize and the amount of generations, the standard deviation of
those parameters are shown in the figures
[21](#SDG){reference-type="ref" reference="SDG"} and
[22](#SDP){reference-type="ref" reference="SDP"}. It should be clear
through the spikiness of the graphs that the standard deviation related
to the amount of generations is generally constant in contrary to that
of the poolsize since that seems to generally be decreasing. This means
that the recommended settings for the goal of increasing the consistency
can be concluded to be a minimal investment in the generations, a value
between 30 and 50 for the elites percentage and a poolsize as big as the
available computational power allows.

![*The Standard Deviation vs. The amount of
Generations*[]{label="SDG"}](SDG){#SDG height="6cm"}

![*The Standard Deviation vs. The Poolsize*[]{label="SDP"}](SDP){#SDP
height="6cm"}

Conclusions
===========

In the Introduction of this article, the following question was asked:
How does a Genetic Algorithm perform when applied to the Travelling
Salesman Problem? This article has provided the general concept of a GA,
together with a detailed explanation of the construction and
characteristics of such an algorithm for the TSP. Furthermore, the
performance of the constructed GA has been discussed on small and larger
TSPs.

For small problems, the GA tends to converge to the global optimum in
every run and therefore it can be said that it performs excellently.
More traditional methods, like the Hungarian Algorithm and the BIP, are
also capable of finding the optimal tour. However, when a problem of a
larger scale is given, these traditional methods become highly
inefficient. This is because of the time they need to find a solution
and in some cases the construction of these methods is infeasible. Even
though the GA also needs more time, when the TSP expands, it is still
capable of finding a solution within minutes, which is a lot faster than
the other mentioned methods. Another benefit of the GA is that it can be
applied to any TSP without a reconstruction when given the neccesary
data, consisting of the number of cities and their distances to each
other.

The GA however, does also have some disadvantages. One example is
premature convergence, which occurs when applying the GA to a very large
TSP. Another disadvantage is that the exact settings for optimal
performance are closely related to the GA itself and take a considerable
amount of time to determine. However, this disadvantage does have a
definite solution, whereas in the case of premature convergence such a
solution is hard to find. It is debatable whether premature convergence
is even solvable or not, some believe that solving premature convergence
would also result in solving the P versus NP problem to which the TSP is
famously related.

12

D.L. Applegate, R.E. Bixby, V. Chvatal, and W.J. Cook. (2006). *The
Traveling Salesman Problem: A Computational Study*. Princeton: Princeton
University Press. Retrieved: June 4, 2017. Available:
http://press.princeton.edu/chapters/s8451.pdf

R.K.B. Bhattacharjya. (2013, November 7). Introduction to Genetic
Algorithms \[Lecture slides from the Indian institute of Technology
Guwahati\]. Guwahati. Retrieved: May 12 2017. Available:
https://www.iitg.ernet.in/rkbc/CE602/CE602/Genetic%20Algorithms.pdf

Google. *Google Maps* \[Google Maps distances\]. Retrieved: May 19,
2017.

B. de Jonge. (2016). \[Lecture slides from the University of
Groningen\]. Operations Research 1, Lecture 7: Network Theory 2.

L. Juan, C. Zixing, and L. Jianqin. (2010). Premature convergence in
genetic algorithm: Analysis and prevention based on chaos operator.
*Proceedings of the 3rd World Congress on Intelligent Control and
Automation*. 495-499.

Y. Leung, Y. Gao, and Z. Xu. (1997). Degree of Population Diversity - a
Perspective on Premature Convergence in Genetic Algorithms and Its
Markov Chain Analysis. *IEEE Transactions on Neural Networks*, *8*(5),
1165-1176.

J.D.C. Little, K.G. Murty, D.W. Sweeney and C. Karel. (1963). An
algorithm for the traveling salesman problem. *Operations Research*,
*11*(6), 972--989.

M. Mitchell. (1998). *An introduction to genetic algorithms.* Cambridge
MA: MIT Press.

H.M. Pandey, A. Chadhary and D. Mehrota. (2014). A Comparative Review of
Approaches to Prevent Premature Convergence in GA. *Applied Soft
Computing*, *24*, 1047-1077.

B.R. Rajakumar and Aloysius George.(2013). APOGA: An Adaptive Population
Pool Size Based Genetic Algorithm. *AASRI Procedia*, *4*, 288-96.

J.P. Ryan. (1992). *An algorithm for the solution of a traveling
salesman problem to minimize the average time to demand satisfaction*
\[Unpublished master's thesis\]. Texas AM University. Retrieved: May 31,
2017.

O.H.P. Strik. (2017) *GAFramework* \[The publication of the code\].
Groningen. Available: https://github.com/Kranex/GAFramework

Appendix
========

Full Hungarian Algorithm {#FullHA}
------------------------

As already mentioned in section [4](#SimpleTSP){reference-type="ref"
reference="SimpleTSP"} of this article, the full Hungarian Algorithm
takes an immense amount of time for any TSP of a reasonable scale.
However, with a small problem like the one described in that section, it
is not too problematic. What follows in this section is a full solution
of the 6-city problem via the modified Hungarian Algorithm.

Begin with the distance matrix:

![image](distancematrix){height="5cm"}

Perform the first step: row and column reduction. For that, subtract the
smallest element of each row from its respective row, then do the same
for columns, like so:

![image](1red0){height="5.2cm"}

This results in the following matrix, called the reduced distance
matrix:

![image](1red){height="6cm"}

The second step of the algorithm is penalty calculation. The penalty of
a zero is the sum between the smallest element in the row and the
smallest element in the column in which that zero stands. The figure
below shows these penalties:

![image](1pen){height="6cm"}

Third step is elimination. The row and the column of the zero with the
highest penalty get eliminated from the matrix, leaving behind a smaller
matrix and a fraction of the final tour. Hence, row B and column D get
eliminated:

![image](1elim2){height="6cm"}

From this step we got a fraction of the final tour, namely road B
$\rightarrow$ D, and a smaller matrix, for which we now repeat steps
1-3. Note that element DB is undefined, since road B $\rightarrow$ D is
already taken.

[Step 1:]{.underline}

![image](2red0){height="5.6cm"}

[Step 2:]{.underline}

![image](2pen){height="5.2cm"}

[Step 3:]{.underline}

![image](2elim3){height="5.2cm"}

Therefore, we add road A $\rightarrow$ B to the list and remove the
value of element BA. In this case, row B has already been eliminated, so
element BA already does not exist in the new matrix. Now repeat again.
Notice that the newest matrix already contains a zero in every row and
every column, so step 1 could be emitted.

[Step 2:]{.underline}

![image](3pen){height="4.8cm"}

[Step 3:]{.underline}

![image](3elim4){height="4.5cm"}

Hence, we note down road C $\rightarrow$ A, and proceed with the 3x3
matrix.

[Step 1:]{.underline}

![image](4red0){height="4.2cm"}

[Step 2:]{.underline}

![image](4pen){height="3.7cm"}

[Step 3:]{.underline}

![image](4elim5){height="3.6cm"}

Road E $\rightarrow$ F is now part of the tour, and element FE becomes
undefined in the new 2x2 matrix. Now, one could proceed with the steps
of the algorithm, but at this stage it is fairly simple to select the
final tours: two are remaining and only two are possible:
F $\rightarrow$ C and D $\rightarrow$ E, which correspond to elements FC
and DE respectively. DC cannot be selected because it would leave FE,
which is not possible, so FC and DE is the correct choice. Looking back,
the following roads have been selected:

B $\rightarrow$ D,

A $\rightarrow$ B,

C $\rightarrow$ A,

E $\rightarrow$ F,

F $\rightarrow$ C,

D $\rightarrow$ E.

These roads can be compiled into a tour as follows:

A $\rightarrow$ B $\rightarrow$ D $\rightarrow$ E $\rightarrow$ F
$\rightarrow$ C $\rightarrow$ A,

with city A chosen arbitrarily as the starting point. This completes the
algorithm.

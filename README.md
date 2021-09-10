# Polyhedral_Feautrier_algo
This is a briefly introduction/summary/review about Feautrier algorithm based on [Some efficient solutions to the affine scheduling problem Part I One-dimensional Time](https://www.researchgate.net/publication/2421859_Some_efficient_solutions_to_the_affine_scheduling_problem_Part_I_One-dimensional_Time) and [Some efficient solutions to the affine scheduling problem Part II Multidimensional time](https://www.researchgate.net/publication/2810999_Some_efficient_solutions_to_the_affine_scheduling_problem_Part_II_Multidimensional_time). Many corrent polehedral models are based on Feautrier's model. Feautrier's orginal paper is very long(80+pages intotal). Next, I will pick the key points so that people can understand faster.

## Generalized Dependence Graph
<!-- <img src="https://render.githubusercontent.com/render/math?math=e^{i \pi} = -1"> -->
A Generalized Dependence Graph (GDG) is a directed multigraph. A vertex represents a set of related statements. Each statement belongs to one (and only one) vertex of the GDG. An edge represents a timing constraint(dependence relation) between two operations which belong respectively to its source and destination. The GDG may have loops (i.e. an edge whose source and destination are the same vertex) and cycles. 
- Each vertex <img src="https://render.githubusercontent.com/render/math?math=S"> is associated with a domain polyhedron <img src="https://render.githubusercontent.com/render/math?math=D_S">. 
- Each edge <img src="https://render.githubusercontent.com/render/math?math=e"> is associated with a dependence relation polyhedron <img src="https://render.githubusercontent.com/render/math?math=R_e">.

<figure>
<img src="https://user-images.githubusercontent.com/22283979/131426147-20dda646-34c4-4db0-9d07-873ae98ae8ef.png" height="150" align = "center">
<figcaption align = "center"><b>Fig.1 - Program 1</b></figcaption>
</figure>

Take above program as an example, there are two statements(vertex). Whose domain are:

<img src="https://user-images.githubusercontent.com/22283979/131427464-ba2fab65-c430-4203-b07e-528ab8a41e03.png" height="75" align = "center">
And two edges, first one has the dependence polyhedron:

<img src="https://user-images.githubusercontent.com/22283979/131427650-64ae28d6-1e4d-4f79-81ce-635681a71b41.png" height="60" align = "center">
Second edge is an uniform loop with dependence polyhedron:

<img src="https://user-images.githubusercontent.com/22283979/131427901-79f84ffc-a5f6-4e75-9143-65ad02169dba.png" height="50" align = "center">
These two dependence can be simplfied to the following(typo: not 1 should be 0, not 2 should be 1):
<img src="https://user-images.githubusercontent.com/22283979/131428051-c459340a-1c20-458a-b963-f5a3da2fe70e.png" height="70" align = "center">

It is clear that each dependence polyhedral <img src="https://render.githubusercontent.com/render/math?math=R"> can be simplified into a pair of polyhedral <img src="https://render.githubusercontent.com/render/math?math=P"> and an affine transformation <img src="https://render.githubusercontent.com/render/math?math=h">:
<img src="https://user-images.githubusercontent.com/22283979/131428301-f9429eb7-8c2b-47fc-a2dc-ef884fd0a625.png" height="40" align = "center">

In this case we can eliminate x. Use this way, dependence polyhedral of edges in above example would be:

<img src="https://user-images.githubusercontent.com/22283979/131435035-2660acef-27e0-499d-95f6-d865c646b7e0.png" height="70" align = "center">
<img src="https://user-images.githubusercontent.com/22283979/131435175-112c8644-c709-4ce6-be81-48a11687beaa.png" height="70" align = "center">

## one-dimensional algorithm
### 1d - Schedule
<img src="https://user-images.githubusercontent.com/22283979/131428805-93c91093-ba43-4a22-ab4e-5d5f4a9eb7a3.png" height="100" align = "center">
Each schedule has the form:
 <img src="https://render.githubusercontent.com/render/math?math=\theta(S,x) = \vec{a}x%2B\vec{b}n%2Bc"> 
where a,b are unknown vectors and c is unknown number. n is the structure parameters(known).
The problem is that given a GDG, find the solutions which satisfy the causality condition above. Will talk about picking an optimal solution later.
To find the solution of schedules, Farkas lemma will be used.

<img src="https://user-images.githubusercontent.com/22283979/131432277-d36fb5ba-2045-400a-b18a-b46585f6cc4c.png" height="200" align = "center">

Now, each domain polyhedral D is represented by a set of inequalities:

<img src="https://user-images.githubusercontent.com/22283979/131432645-9f0eb6f4-1c43-4002-83ff-cda717ef3b40.png" height="60" align = "center">
Each dependence polyhedral R is repensented by a set of inequalities similarly:
<img src="https://user-images.githubusercontent.com/22283979/131432724-192e606c-51a6-42e0-9edb-e05c486b52a3.png" height="80" align = "center">
And dependence polyhedral can be simplified as the description of P:
<img src="https://user-images.githubusercontent.com/22283979/131432868-e827f722-829c-4478-87d2-bce16839a872.png" height="65" align = "center">

Each schedule must be nonnegative, so first use Farkas multiplier to represent schedule in this way:
<img src="https://user-images.githubusercontent.com/22283979/131433110-16481059-5ece-420c-8e4e-069b664632ab.png" height="65" align = "center">

For each edge in GDG, there is delay associate with it where <img src="https://render.githubusercontent.com/render/math?math=\delta"> is the destination of egde and <img src="https://render.githubusercontent.com/render/math?math=\sigma"> is the source of the edge :

<img src="https://user-images.githubusercontent.com/22283979/131433433-6f03329b-7519-4295-a756-c758c66e0e2b.png" height="40" align = "center">

Each edge must have non-negative delay.
So there are Farkas multiplier(typo: should be e not f):
<img src="https://user-images.githubusercontent.com/22283979/131434250-e70346a6-4577-4fa6-a368-8651d21c4d43.png" height="100" align = "center">

As we discussed above, R can be represented by P and h, so above can be rewrite as:

<img src="https://user-images.githubusercontent.com/22283979/131434497-bbf99d8b-c43c-47f0-89ca-e08f5ca25128.png" height="70" align = "center">

then we can use Linear programminng tools to solve Farkas multipliers. Usually choose <img src="https://render.githubusercontent.com/render/math?math=\mu"> with minimum absolute value in lexicographical order.

### 1-d schedule example
Use program 1 above as an example. Here are schedules with Farkas multipliers:
<img src="https://user-images.githubusercontent.com/22283979/131435282-c9ceb6e7-1dc3-4e1b-a8cc-02439f7a42b8.png" height="70" align = "center">

Delay for the first edge:

<img src="https://user-images.githubusercontent.com/22283979/131435606-b2a24722-2d99-4255-b164-7e8ef2685b28.png" height="90" align = "center">

which results:

<img src="https://user-images.githubusercontent.com/22283979/131436049-fa0d1bdf-ff35-4ca4-9310-9fa2c4e5713f.png" height="110" align = "center">

Delay for the second edge do not need to use Farkas Lemma since it is uniform:

<img src="https://user-images.githubusercontent.com/22283979/131436172-a4d3a6a1-6d74-4776-8b40-83275e5c7032.png" height="50" align = "center">

Eliminate unknowns will give:

<img src="https://user-images.githubusercontent.com/22283979/131436557-592a9f54-e9b8-4479-9951-97dc938a1c8f.png" height="150" align = "center">

Finally:

<img src="https://user-images.githubusercontent.com/22283979/131436715-46c1a832-4768-457b-a9ba-802416056a2e.png" height="120" align = "center">

Below is one possible solution:

<img src="https://user-images.githubusercontent.com/22283979/131437093-569217b0-8f21-4839-91c9-55ed3462acc6.png" height="70" align = "center">

with parallel form of the program:

<img src="https://user-images.githubusercontent.com/22283979/131437257-3e43fd73-e366-4e78-891e-f3ee396fb7b8.png" height="150" align = "center">

There might be many other solutions.

### 1-d how to select a good solution

- Minimize Latency

\bf{Lemma}: if domains are bounded, and exists at least one affine schedule, then there exist at least one affine form of structure parameters:

<img src="https://user-images.githubusercontent.com/22283979/131444088-b96fec06-f8c5-43cf-9dc2-a235bd4fdc83.png" height="100" align = "center">

We may apply Farkas Lemma:

<img src="https://user-images.githubusercontent.com/22283979/131445822-49ccde76-153c-4ee8-b5fc-7f422591129e.png" height="50" align = "center">

Make h and k be the leading unknown, so that Linear Programming solver will give the minimal values for h. Applying this to above program gives the min latency L=n+1. The problem of this method is that the order of structure parameters and unknowns matter.

- Bounded delay schedule

Want to find the minimun <img src="https://render.githubusercontent.com/render/math?math=\delta"> such that for each edge:

<img src="https://user-images.githubusercontent.com/22283979/131446864-e558bcc7-8cf5-459d-99c0-a1329486a392.png" height="50" align = "center">

Smaller delay tend to have better schedule(cache?).
method is very obvious, apply Farkas Lemma and make <img src="https://render.githubusercontent.com/render/math?math=\delta"> the leading unknown. Bounded delay is more constrained than minimum latency.

- Dual

<img src="https://user-images.githubusercontent.com/22283979/131447495-3a6cb8ac-6e92-4472-90c4-1b5d04125883.png" height="70" align = "center">

So want to find the minimum schedule:

<img src="https://user-images.githubusercontent.com/22283979/131448231-e7a09cec-9095-4b8a-b307-d3cd557af500.png" height="160" align = "center">

This might return a isolated numerical values of <img src="https://render.githubusercontent.com/render/math?math=\theta">, rather than a closed form solution.
Since parameters(n?) occur in the objective function, not suitable for Linear Programming solver. By the following thm:

<img src="https://user-images.githubusercontent.com/22283979/131450013-830a200c-1743-4ff3-9edf-3c6999348fb9.png" height="260" align = "center">

We can have:

<img src="https://user-images.githubusercontent.com/22283979/131450112-2ab168e6-4c13-4fa4-9b3e-705bf424100b.png" height="230" align = "center">

Dual method and minimum latency method have the same latency. But dual method and bound delay method may have different delay.

- Uniform recurrences

For the uniform dependences no need to use Farkas Lemma. The condition for edge e simply be:

<img src="https://user-images.githubusercontent.com/22283979/131451698-a5edf34b-d751-465b-a6eb-2912ba02453a.png" height="100" align = "center">

The dual form:

<img src="https://user-images.githubusercontent.com/22283979/131453532-c2d6bcd9-3b83-44b3-b0b8-b6ef7c3a6310.png" height="120" align = "center">

Consider following as an uniform example:

<img src="https://user-images.githubusercontent.com/22283979/131453884-d91dedc3-fccc-4c6e-8160-c3619c6351b6.png" height="60" align = "center">

<img src="https://user-images.githubusercontent.com/22283979/131453950-d3475b93-6559-4f19-b727-5d1c99e7c04f.png" height="60" align = "center">

<img src="https://user-images.githubusercontent.com/22283979/131454150-df792063-ed69-4714-b298-e9d7effc0abd.png" height="30" align = "center">

let schedule be:

<img src="https://user-images.githubusercontent.com/22283979/131454358-509188a5-08e1-4f07-ad55-c100a5cbd607.png" height="30" align = "center">

the dual form is：

<img src="https://user-images.githubusercontent.com/22283979/131454471-6f424b2a-1726-45c1-999d-354de6747ae8.png" height="160" align = "center">

Linear Programming solver gives:

<img src="https://user-images.githubusercontent.com/22283979/131454607-55f70fea-e00a-4775-8783-b18b8cd2b3ae.png" height="140" align = "center">

### drawbacks of 1-d algo

- Some programs whose free schedule if not concave, then cannot find piecewise affine schedule.

For this program:

<img src="https://user-images.githubusercontent.com/22283979/131455341-2fd4a921-d845-4674-a86b-7ab13da93431.png" height="70" align = "center">

the best schedule is：
<img src="https://render.githubusercontent.com/render/math?math=\theta(i) = if\ \ i>n\ \ then\ \ 1\ \ else\ \ 0">

The dule Farkas algo will gives: <img src="https://render.githubusercontent.com/render/math?math=\theta'(i) = i">

The best schedule has latency 1 while the algo will give latency O(n).

- Some program do not have affine schedule:

Take the following program as example:

<img src="https://user-images.githubusercontent.com/22283979/131456132-73d765f2-07ba-43fb-8fb5-e6c837e3fa9c.png" height="110" align = "center">

This program has schedule <img src="https://render.githubusercontent.com/render/math?math=\theta'(i) = ni %2Bj"> which is not linear. The term ni is not linear.

## multi-dimension algorithm
### Basic Algorithm
delay with edge <img src="https://render.githubusercontent.com/render/math?math=e">

<img src="https://user-images.githubusercontent.com/22283979/132789168-e53e9680-2f11-44a9-9b97-36699bb5839c.png" height="50" align = "center">

Schedule must satisfy:

<img src="https://user-images.githubusercontent.com/22283979/132789257-b5634db0-c6e5-4d39-8750-b1d08c872f36.png" height="70" align = "center">

So:

<img src="https://user-images.githubusercontent.com/22283979/132789308-892dea55-0647-4eba-adfd-1f93fd555e00.png" height="40" align = "center">

First iteration starts:

<img src="https://user-images.githubusercontent.com/22283979/132789567-f3da2fd7-7daa-4ab7-b0af-15d73a7cb20e.png" height="40" align = "center">

<img src="https://user-images.githubusercontent.com/22283979/132789600-0affec9f-ea3e-455a-8de7-677206b73f62.png" height="40" align = "center">

May not all strongly satisfy, update polyhedral for next iteration:

<img src="https://user-images.githubusercontent.com/22283979/132789713-2b7e4f62-9e52-4c17-a1ff-609b2c350ac0.png" height="40" align = "center">

<img src="https://user-images.githubusercontent.com/22283979/132789718-70aa509b-af2c-4394-a8da-b343a222d647.png" height="30" align = "center">

Way to update:

<img src="https://user-images.githubusercontent.com/22283979/132789963-ceb0d42c-7217-45bf-99f5-f53b005e37b3.png" height="180" align = "center">

If P is not empty, go to next iteration.

### mult-dimension example

<img src="https://user-images.githubusercontent.com/22283979/132790216-36821663-bcf1-4959-a74b-e1b9cdb7f85d.png" height="350" align = "center">

### Greedy multi-dimension

<img src="https://user-images.githubusercontent.com/22283979/132790388-6e4bc442-bca0-4c3c-b7d3-81fefb1ed2e6.png" height="110" align = "center">

Lemma1: The solution is that all e are either 0 ot 1.

Lemma2: The solution is unique.

Theorem: Let U be an arbitrary subset of the edges of a static program. The solving programing for U always satisfies at least one edge of U(Greedy algorithm terminates).




# Polyhedral_Feautrier_algo
This is a briefly introduction/summary/review about Feautrier algorithm based on [Some efficient solutions to the affine scheduling problem Part I One-dimensional Time](https://www.researchgate.net/publication/2421859_Some_efficient_solutions_to_the_affine_scheduling_problem_Part_I_One-dimensional_Time) and [Some efficient solutions to the affine scheduling problem Part II Multidimensional time](https://www.researchgate.net/publication/2810999_Some_efficient_solutions_to_the_affine_scheduling_problem_Part_II_Multidimensional_time). Many corrent polehedral models are based on Feautrier's model. Feautrier's orginal paper is very long. Next, I will pick the key points so that people can understand faster.

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


## 1d - Schedule
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

## 1-d schedule example
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

<img src="https://user-images.githubusercontent.com/22283979/131437257-3e43fd73-e366-4e78-891e-f3ee396fb7b8.png" height="140" align = "center">

There might be many other solutions.

## 1-d how to select a good solution
哎有空再写吧
要不是因为怕忘记
我也懒得写啊

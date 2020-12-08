# Restricted-Boltzmann-Machine---Assignment
Topic Modelling is the art and science of identifying the 'latent topics' in a text. It is an unsupervised problem. You input a set of documents/ corpus into the model and the model finds the topics that describe the corpus. Each topic is a distribution over the words that best describe the topic. 


As you already know, you have to perform Topic Modelling on a set of documents and print the distribution of words in each topic.  

 

Please download the zip file which contains the notebook and the dataset:

RBM_assignmentfile_download	Download
 

Until now, Prof. has explained the structure of the RBM and has given a generic intuition of the training process of the RBM. You have seen that the training process is contrastive divergence. In this assignment, we shall create a function to perform Gibbs sampling k times within contrastive divergence often referred to as CD-k. Let's understand the structure of the notebook and the CD-k training process:

Importing the dataset.
Create a bag of words model.
Note that the shape of a bag of words model will be (data_size, vocablury_size)
Please note that the above shape might vary with the way you perform bag of words model but the index data_size will be the same.
Training using CD-k
This involves performing Gibbs sampling k-times which can be better understood using the following image.

Contrastive Divergence
 You start with the input batch of data, 
v
0
. 
You then calculate 
p
(
h
|
v
0
)
=
σ
(
c
j
+
∑
n
i
=
1
v
i
w
i
j
)
 as seen in the lecture. 
Vectorized implementation: 
p
(
h
|
v
0
)
=
σ
(
C
+
V
.
W
)
Using this 
p
(
h
|
v
0
)
, you sample 
h
0
.
Now that you have got 
h
0
, you calculate 
p
(
v
|
h
0
)
=
σ
(
b
j
+
∑
m
j
=
1
h
j
w
i
j
)
 as seen in the lecture.
Vectorized implementation: 
p
(
v
|
h
0
)
=
σ
(
B
+
H
.
W
T
)
Using this 
p
(
v
|
h
0
)
, you sample 
v
1
.
You repeat this k times till you get 
h
k
 and 
v
k
.

Step 2 and 3 are performed in the function sampleHiddenLayer() while Step 4 and 5 are performed in sampleVisibleLayer(). 

sampleHiddenLayer() and sampleVisibleLayer() are combined to create a function gibbs() which does one iteration of Gibbs sampling.

gibbs is repeated k-times in the function cd_k() to perform Contrastive Divergence k-times.

 

You have already learned that the training process in an RBM involves maximizing the joint probability distribution. Using the energy function as defined in the lectures and the above sampling process, the update matrices and vectors simplify as follows:

Δ
W
=
v
0
⊗
p
(
h
0
|
v
0
)
−
v
k
⊗
p
(
h
k
|
v
k
)
Δ
b
=
a
v
g
_
a
c
r
o
s
s
_
b
a
t
c
h
(
v
o
−
v
k
)
Δ
c
=
a
v
g
_
a
c
r
o
s
s
_
b
a
t
c
h
(
p
(
h
0
|
v
o
)
−
p
(
h
k
|
v
k
)
)
You do average across batch because you need a vector update for the bias vectors and 
v
o
,
v
k
 and 
p
(
h
0
|
v
o
)
,
p
(
h
k
|
v
k
)
 are matrices.

 

Note that the exact derivations are not covered as it is very complex and the Prof. has given the intuition about the training process in the previous segments.

 

Since you have to maximize the joint probability distribution, you use gradient ascent here with momentum. It is recommended that you understand the working of momentum from here.

 

The momentum equations are as follows:

m
W
t
=
γ
 
m
W
t
−
1
−
Δ
W
m
b
t
=
γ
 
m
b
t
−
1
−
Δ
b
m
c
t
=
γ
 
m
c
t
−
1
−
Δ
c
γ
 is the momentum coefficient here.

Using these momentum terms, you update the weights and biases as follows:

W
t
=
W
t
−
1
+
α
 
m
W
t
b
t
=
b
t
−
1
+
α
 
m
b
t
c
t
=
c
t
−
1
+
α
 
m
c
t
α
 is the learning rate.

The above equations are implemented in the function train().

 

These equations should help you in implementing topic modelling using RBMs without any issue.

 

In the end, you'll be able to see the words that constitute the different topics.

 

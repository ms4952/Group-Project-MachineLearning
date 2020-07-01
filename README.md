# Music Recommendation system by  Tiffany Jason and Mitali

Collaborative filtering is a method of building recommendation
systems that use the totality of all user feedback
of all items to give recommendations. This feedback can be
either explicit, such as a 5-star rating, or implicit, such as a
listen count. We compare two collaborative filtering approaches
for recommending music based on a set of implicit feedback:
a neighborhood approach and a latent model approach. We
compare our results from these approaches with a third naive
popularity recommender as a baseline. Our results found that the
latent model approach performs the best while the neighborhood
and popularity approach are roughly equal.

based Approach
We start with a naive popularity based approach which
gives the same generic results to all users. We simply sort
the songs in decreasing order of their total listen count. Our
first recommendation to every user is the most popular song.
Our second recommendation is the second most popular song.
And so on. This is a surprisingly effective approach despite
not being at all tailored to the user’s tastes.
B. User-based Neighborhood Approach
A user-based neighborhood approach depends on a similarity
metric between users. We use the cosine similarity between
each user’s listen count vector. Our goal is to predict the
unobserved rating rui a user u might give an item i. We denote
U as the set of k nearest users to u who rated i. The value
of rui is a weighted average. This is a traditional k-nearest
neighbors approach. See Fig. 2 for the exact calculation.
C. Latent Factor Approach
First, we lay out our m users and n items in a n  m
”interaction” matrix where each value rui denotes the rating
user u has given item i. This rating can be explicit such as a 5-
star rating or implicit such as the number of times the user has
listened to that song. Second, we decompose this interaction
matrix into an mf matrix X and an nf matrix Y where
f is the number of latent factors. Then to predict a user’s
rating for a certain item, we take the inner product XT
u Yi. One
shortcoming with this approach is there is no intuition behind
what these latent features mean. Another larger shortcoming
is that there is no way to introduce a new user or item without
recreating the entire model.
We are particularly interested in approaches that work well
for implicit feedback. We borrowed the Weighted Matrix
Factorization (WMF) method introduced by Hu et el. To work
better with implicit feedback, WMF introduces two new values
derived from rui. The first is preference, pui, which is simply
a binary value denoting if user u has listened to song i at least
once. The second is confidence, cui, which is a scalar-weighted
value proportional to listen count. Confidence denotes how
confident we are they like the song. One example confidence
function is given below.
cui = 1 + rui
We need these new values to work around implicit feedback.
A user’s listen count for a song is not really a rating. If a user
has never listened to a song, that does not mean they do not
like it. They simply may have never heard it. Additionally, with
only listen counts we have no way to tell if a user does not
like a song. We make two assumptions: (1) if a user listened
to a song once, they like it, (2) the more a user listened to
a song, the more they like it. This is the motivation for the
definitions of preference and confidence.
From there we borrow the objective function of Hu et al.
to optimize the latent matrices X and Y .
argmin
X
u;i
cui(pui 􀀀 XT
u Yi)2 + (
X
u
kXuk2 +
X
i
kYik2)
This consists of a confidence-weighted mean squared error
term and a lambda-weighted regularization term to curb overfitting.
However because this function contains over m  n
terms, this will quickly explode on a typical dataset. This
prevents the usual stochastic gradient descent for optimization.
Instead Hu et al. have proposed another method of minimizing
this function called Alternating Least Squares (ALS). If you
hold either the user-factors or item-factors fixed, the function
becomes quadratic. So, this leads to alternating between computing
user-factor and item-factor. By differentiation, we can
find a closed form solution for Xu.
Xu = (Y TCuY + I)􀀀1Y TCup(u)
Here Cu is the square n  n matrix with the confidence
values, cui, across its diagonal for a given user u. Additionally,
p(u) is the vector that contains all preference values, pui, for
user u.

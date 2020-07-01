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

Popularity-based Approach
We start with a naive popularity based approach which
gives the same generic results to all users. We simply sort
the songs in decreasing order of their total listen count. Our
first recommendation to every user is the most popular song.
Our second recommendation is the second most popular song.
And so on. This is a surprisingly effective approach despite
not being at all tailored to the user’s tastes.

User-based Neighborhood Approach
A user-based neighborhood approach depends on a similarity
metric between users. We use the cosine similarity between
each user’s listen count vector. Our goal is to predict the
unobserved rating rui a user u might give an item i. We denote
U as the set of k nearest users to u who rated i. The value
of rui is a weighted average. This is a traditional k-nearest
neighbors approach. 

C. Latent Factor Approach
First, we lay out our m users and n items in a n  m
”interaction” matrix where each value rui denotes the rating
user u has given item i. This rating can be explicit such as a 5-
star rating or implicit such as the number of times the user has
listened to that song. Second, we decompose this interaction
matrix into an mf matrix X and an nf matrix Y where
f is the number of latent factors. Then to predict a user’s
rating for a certain item, we take the inner product XT
u Yi. Oneshortcoming with this approach is there is no intuition behind
what these latent features mean. Another larger shortcoming
is that there is no way to introduce a new user or item without
recreating the entire model.

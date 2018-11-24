# elezioni_onb_2017
Code for analyzing the elections of the Italian "Ordine Nazionale Biologi" of 2017

This code analyzes the data of the 2017 elections for the Italian "Ordine Nazionale dei Biologi" (ONB).
The results are available here http://www.onb.it/wp-content/uploads/2017/11/RISULTATI-ELEZIONI-2017.pdf

The code is release as Jupyter notebook and is written in Python.
It can be run by installing Python and Jupyter (I recommend the Anaconda platform) and can be run from within the Jupyter notebook.

# The observation
When I analyzed the results of the elections for the first time I noticed something anomalous.
The votes of the candidates where distributed in three groups, the ones that got about 1500 votes, the ones that got about 700 and the last group of candidates with less than a few tens of votes.
This was made obvious by just plotting the number of votes for each candidate, ordered.
It was quite obvious by looking at the histogram of the votes, showing 3 bumps of vote frequencies.
This behavior is anomalous in an election since usually there are a bunch of favorite candidates (sometimes even just 2) who obtain a big chunk of the votes while the rest of candidates gets the rest.
This must hold true also for the ONB's election unless the voting system had some rule that I don't know, for instance if candidates and/or voters were divided into groups.
However the fact that the results were presented all in a single list (see the link above) led me to assume that the election was a typical one with all candidates in the same pot and all voters allowed to vote any candidate.
Notice that this assumption may not necessarily hold true but I'll use it for the rest of the exercise here.
Notice also that the total number of voters was about 7x smaller than the total number of votes, which means that likely each voter could express about that number of preferences.
Unfortunately I couldn't find any information in the ONB's website about the election rules.

# The idea
To verify that my intuition was somewhat reasonable, i.e., that the distribution of votes should have a much more "spread" look in case of a fair election and not steps like in the data, I created a simple stochastic process to generate election results.
The main question was to generate a distribution of votes that was somewhat realistic.
Indeed, assigning votes from a uniform distribution would be completely unrealistic, since all the candidates would receive about the same votes (about 700), plus or minus some noise.
I therefore came up with a simple algorithm to allow the system to skew the distribution towards strong candidates.
First, I used an ideal observer approach in which votes are sequential and each voter can see the vote of all the other voters up to that points.
This is useful since it allows the algorithm to manipulate the popularity of a candidate while keeping the stochastic nature of the process.
Then, the algorithm progressively increases the probability to get one vote to the candidates that obtained more votes to that point.
This mechanism is reminiscent of the "preferential attachment" of the Barabàsi network generation algorithm https://en.wikipedia.org/wiki/Barabási–Albert_model.
This type of algorithms generate "hubs", i.e., nodes with a much higher degree of connectivity than the rest of the nodes, like in the Internet and in social networks.
Similarly, in my voting simulation, such a mechanism generates popular candidates, as in normal elections.
I assumed, as I got from the data, a share of 29220 votes with 3589 voters and 43 candidates.

# Results
As I expected from intuition, the simulation generated a much more spread distribution, basically an exponential distribution whereby the first couple of candidates would get a large number of votes whilst a minority of candidates would share a much larger proportion of votes.
I then played a bit with the algorithm to see whether there could be a simple explanation to the steps seen in the data.
I then allowed the first 9 candidates to start with 100 votes each.
The result of this unfair manipulation is that these 9 favorite candidates are the ones that initially take the votes because they have a much higher probability to get an other vote from the beginning, as opposed to the rest of the candidates.
On the other hand if one of the 9s get 1 vote, this only increases its "popularity" by 1/100 with respect to the other 8 in the group.
Indeed, this simulation with an initial gift of 100 votes to 9 candidates generates a step in the distribution of the votes somewhat similar to the real data.
This result encouraged me to generate 2 gifted groups, one for 9 candidates and one for an other set of 18 candidates.
I found that giving about 120 votes to the first group and 60 to the second groups gets qualitatively close to the data.
Further optimization may find a much closer fit to the data, which goes beyond the point of this work.

# Conclusion
The 2017 election of the ONB has many obscure points and certainly a lack of transparency that is not appropriate for such an important organism.
In this work, I tried to use very simple statistics and my programming skills to analyze the results of the elections from a purely numerical standpoint.
The results show that three groups of candidates shared votes, the popular ones with about 1500 votes, the less popular ones with about 700 votes each and the least popular ones with a few hundred of votes.
This behavior is inconsistent with a model in which all voters are allowed to express preferences for all the candidates but in which some candidates are more favorite than others.
In particular in my model I used a sequential vote mechanism in which the candidates with most votes increase their popularity.
The results of the simulations show that this mechanism generates strong winners, as expected from an election with popular candidates. The simulations also show that the steps distribution cannot be obtained unless votes are initially assigned to groups of candidates.
All in all, the results show that it is possible to obtain a step distribution in a voting process in which popular candidates progressively increase their popularity and votes are gifted to groups candidates.

Notice however that the results I described here do **not** demonstrate that the 2017 election of the ONB was unfair nor manipulated.
In particular, these results do **not** demonstrate that votes where gifted with some candidates.
There may be multiple reasons why the results could show a step distribution like the one in the data, including very plausible reasons related to the rules of the elections.
Unfortunately, these rules are not public (shame) and therefore it is not possible to assess with certainty what mechanism could have led to that distribution.
My algorithm is just one of a possibly infinite class of algorithms generating votes stochastically.

# License
The code is released under whatever free license to do whatever you want with it.

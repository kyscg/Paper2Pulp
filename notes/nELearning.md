This week, I picked up the paper titled [Never-Ending Learning](http://www.cs.cmu.edu/~tom/pubs/NELL_aaai15.pdf). 
It is a part of [Read The Web](http://rtw.ml.cmu.edu/rtw/) project at Carnegie Mellon University. It introduces 
a new machine learning paradigm called never-ending learning. In most machine learning applications, a system aims
to learn a single function or data model from a given data set. But in this new paradigm, the system learns the
different type of functions from years of experience in a way that previously learned knowledge makes way for 
accumulating new types of knowledge. Stagnation and performance plateaus are avoided by the ability to formulate
new learning tasks.

#### Summary
A never-ending learning problem has 2 components — a set of learning tasks and a set of coupling constraints. A
learning task in this paradigm is same as the learning task in any other paradigm — improving the system’s performance,
as measured by some metric **P**, over a task **T** given some experience **E**. Each coupling constraint can be thought
of as a function, defined over 2 or more learning tasks, which specifies the degree of satisfaction of the constraint.
Given such a learning problem, a never-ending learning agent **A** produces a sequence of solutions to the individual learning
tasks such that, over time, the quality of the individual learning functions as well as the degree to which each coupling
constraint is satisfied both increases. To take a simplified example, consider the problem of Google classifying emails
in our inbox. Let us say we have 2 learning tasks going on — one which learns whether to put a mail in spam or not and
another whether to mark a mail important or not. An obvious constraint here would be that any mail which is marked as
spam must not be marked important (though not the other way round). Think of them as the set of rules that you do not
want to see violated. What makes coupling constraints important and powerful is that the learning agent can improve its
learning of one function by successfully learning other functions.

The paper also presents the case study of a program called the **Never-Ending Language Learner** (NELL). NELL implements some of the features of this new paradigm and has been in action 24X7 since January 2010. Every day it extracts beliefs from the web and updates its knowledge base by removing incorrect beliefs and adding the ones which, it believes, are correct.

NELL started with an initial ontology of categories and some labelled examples and relations and has got over 80 million interconnected beliefs in its Knowledge Base (KB) so far. It is performing over 2500 learning tasks which include:
- Category classification: eg Apple is a food and a company.
- Relationship classifications: eg (Apple, Company, “IsA”).
- Entity Resolution: e.g “NYC” and “Big Apple” can refer to the same entity.
- Inferring Rules among belief triples.

For all these tasks, it uses a variety of classification methods. The coupling constraints include:
- Multi-view co-training coupling: Different classifiers should give same output on the same input.
- Subset/superset coupling: If a category A is a subset of category B then each phrase belonging to A must belong to B.
- Multi-label mutual exclusion coupling: When 2 categories are declared to be mutually exclusive, none of the noun phrases should lie in both categories.
- Coupling relations to their argument types: In constraints like LivesIn(A, B), A should be a person and B should be a place.
- Horn clause coupling.

Each reading and inference module (based on above classifications and constraints) sends proposed updates in the KB to a Knowledge Integrator (KI) which makes a final decision on all these updates. Once the updates are made, all the modules are re-trained based on the updated KB. Due to sheer size of the KB, it is not possible to consider each and every candidate belief so the KI would consider only high confidence candidate beliefs and would re-assess confidence using a limited subgraph of consistency constraints and beliefs. This means many iterations are required for the effect of constraints to propagate throughout the graph. The paper mentions a more effective algorithm to achieve this propagation.

To add learning tasks and ontology extension, it extracts sentences mentioning different categories, build a context by context co-occurrence matrix and then cluster the related contexts together. Now each cluster corresponds to a candidate relation. These candidates go through a trained classifier and manual filtering before they are added to the ontology.

An empirical analysis of the systems performance shows that:

The system is improving its reading competence over time as was expected and desired.
Its KB is increasing but at a decreasing rate as it gets difficult to extract new knowledge from less frequently mentioned beliefs.
The paper mentions some places for improvement:
-Adding a self-reflection capability to NELL so that it can detect where it is doing well and where it is doing poor and allocate its resources more intelligently. This feature is a part of the paradigm itself.
-Broaden the scope of NELL by using other data sources as well eg Never-Ending Image Learner (NEIL) uses image data.
-Merge other ontologies like DBPedia to boost to its ontology.
-Use ”micro-reading” methods to NELL perform deep semantic analysis

The never-ending learning paradigm raises 2 fundamental questions:

Under what conditions is an increasingly consistent learning agent also an increasingly correct agent? — This is important because an autonomous agent can perceive consistency but not correctness.
Under what conditions is convergence guaranteed in principle and in practice? — The architecture may not have sufficient self-modification operations to ensure never-ending learning or these operations may not be practical due to limits of computation and/or training experience.

#### Thoughts

What makes never-ending learning different, and in some cases more powerful, from conventional paradigms are the concepts of coupling constraints and never ending learning. As a learning model, it seems closer to the human learning model. We try to learn and take actions by relating different scenarios. Our actions and decisions are constrained by both variables and our other actions and decisions. Constraint coupling seems to capture this requirement.

Then there are scenarios where conventional machine learning approaches would fail. Learning is not always about throwing in more data or introducing new variables. If the domain is evolving rapidly, there would always be newer data coming in and newer variables that are not accounted for. These are the kind of scenarios where this paradigm can fill the gap.

Another aspect is that all this work builds on top of existing work. All the algorithms used for the various classifications are existing ones. The paradigm does not suggest a new algorithm for any of the individual learning problems. Instead, it provides a mechanism where success in learning one function helps in learning others.

The paper has strongly put forward the case for this new paradigm. There is a case study to evaluate the model practically, they start out with a small labeled dataset, the reported metrics are behaving as desired and the web is the best choice for applying a never-ending learning algorithm as the web is the never-ending growing domain. One criticism is that the paper does not mention how resource-intensive NELL is — other than mentioning about the dataset. Even time-based metrics are missing. Not that I expect such a system to be frugal, but I would none-the-less be interested in knowing about their computing infrastructure and time-based metrics.

There is still a lot to be explored about the effectiveness of this model. Two prime questions are already listed above. Other than that, the model needs firm mathematical footing. It also needs to be put to test in other domains as well. NEIL is one of the extensions of this. I would be interested to see how does this approach plays out in other domains and what kind of ontologies are obtained especially in case of social networks which are both data-rich and constantly evolving.

# GloVe: Global Vectors for Word Representation

## Introduction

* Introduces a new global log-bilinear regression model which combines the benefits of both global matrix factorization and local context window methods.


## Global Matrix Factorization Methods

* Decompose large matrices into low-rank approximations.
* eg - Latent Semantic Analysis (LSA)

### Limitations

* Poor performance on word analogy task
* Frequent words contribute disproportionately high to the similarity measure.

## Shallow, Local Context-Based Window Methods

* Learn word representations using adjacent words.
* eg - Continous bag-of-words (CBOW) model and skip-gram model.

### Limitations

* Since they do not operate directly on the global co-occurrence counts, they can not utilise the statistics of the corpus effectively.


## GloVe Model

* To capture the relationship between words *i* and *j*, word vector models should use ratios of co-occurene probabilites (with other words *k*) instead of using raw probabilites themselves.
* In most general form:
     * *F(w<sub>i</sub>, w<sub>j</sub>, w<sub>k</sub><sup>~</sup> ) = P<sub>ik</sub>/P<sub>jk</sub>* 
* We want *F* to encode information in the vector space (which have a linear structure), so we can restrict to the difference of *w<sub>i</sub>* and *w<sub>j</sub>*
    * *F(w<sub>i</sub> - w<sub>j</sub>, w<sub>k</sub><sup>~</sup> ) = P<sub>ik</sub>/P<sub>jk</sub>*
* Since right hand side is a scalar and left hand side is a vector, we take dot product of the arguments.
    * *F( (w<sub>i</sub> - w<sub>j</sub>)<sup>T</sup>, w<sub>k</sub><sup>~</sup> ) = P<sub>ik</sub>/P<sub>jk</sub>*
* *F* should be invariant to order of the word pair *i* and *j*.
    * *F(w<sub>i</sub><sup>T</sup>w<sub>k</sub><sup>~</sup>) = P<sub>ik</sub>*
* Doing further simplifications and optimisations (refer paper), we get cost function, 
    * *J = Sum (over all i, j pairs in the vocabulary)[w<sub>i</sub><sup>T</sup>w<sub>k</sub><sup>~</sup> + b<sub>i</sub> + b<sub>k</sub><sup>~</sup> - log(X<sub>ik</sub>)]<sup>2</sup>* 
    * *f* is a weighing function.
    * *f(x) = min((x/x<sub>max</sub>)<sup>&alpha;</sup>, 1)*
    * Typical values, *x<sub>max</sub> = 100* and *&alpha; = 3/4*
    * *b* are the bias terms.

### Complexity

* Depends on a number of non-zero elements in the input matrix.
* Upper bound by the square of vocabulary size
* Since for shallow window-based approaches, complexity depends on *|C|* (size of the corpus), tighter bounds are needed.
* By modelling number of co-occurrences of words as power law function of frequency rank, the complexity can be shown to be proportional to *|C|<sup>0.8</sup>*

## Evaluation

### Tasks

* Word Analogies
    * a is to b as c is to ___?
    * Both semantic and syntactic pairs
    * Find closest d to w<sub>b</sub> - w<sub>c</sub> + w<sub>a</sub> (using cosine similarity)

* Word Similarity
* Named Entity Recognition

### Datasets

* Wikipedia Dumps - 2010 and 2014
* Gigaword5
* Combination of Gigaword5 and Wikipedia2014
* CommonCrawl
* 400,000 most frequent words considered from the corpus.

### Hyperparameters

* Size of context window.
* Whether to distinguish left context from right context.
* *f* - Word pairs that are *d* words apart contribute *1/d* to the total count.
* *xmax = 100*
* *&alpha; = 3/4*
* AdaGrad update

### Models Compared With

* Singular Value Decomposition
* Continous Bag-Of-Words
* Skip-Gram

### Results

* Glove outperforms all other models significantly.
* Diminishing returns for vectors larger than 200 dimensions.
* Small and asymmetric context windows (context window only to the left) works better for syntactic tasks.
* Long and symmetric context windows (context window to both the sides) works better for semantic tasks.
* Syntactic task benefited from larger corpus though semantic task performed better with Wikipedia instead of Gigaword5 probably due to the comprehensiveness of Wikipedia and slightly outdated nature of Gigaword5.
* Word2vec’s performance decreases if the number of negative samples increases beyond about 10.
* For the same corpus, vocabulary, and window size GloVe consistently achieves better results, faster.

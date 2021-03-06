# Amazon_Automotive_Reviews_LDAvis
### *By Bhushan Mehta*
### *Tuesday's batch - Fall 2016 - Business Analytics*
Text reviews analyzed using Topic modelling and visualized using LDAvis in R.

---
*LDAvis visualization*
[Link to visualization](https://rawgit.com/mbhushan909/Amazon_Automotive_Reviews_LDAvis/master/Atomotive_5/index.html#topic=0&lambda=0.5&term=)

___

<p align="center"> <b><i>Case</b></i></p>

___
The data set for this particular analyses has been taken from [Amazon product reviews data repository](http://jmcauley.ucsd.edu/data/amazon/) which is maintained by [Julian McAuley](http://cseweb.ucsd.edu/~jmcauley/).

This data set consisted of 10 columns and 20473 rows. But the data of our interest was mainly the "reviewText" column. Therefore a corpus with mainly the reviewText column was created for our usage. The complete code can be found in the <b><i>Code</b></i> section of this documentation.

This analysis is conducted over automotive parts that are sold on Amazon and the reviews that Amazon customers have posted from May 1996 to July 2014. 

Upon viewing the visualization, the initial page with no topic selected shows the most used or most frequently occuring words in the corpus of this review text. It is evident from the LDAvis visualization that the most frequent word from the top 30 is "car" and one would think that's obvious since the reviews are on automotive parts. The word car has appeared close to 7750 times. When you hover over the word car from the list of the top 30 Salient features, the topics that have the menion of "car" get enlarged. And so it can be seen that topics 4 and topic 13 have no mentions of the word car at all and based on the sizes of the topics, one can comprehend that topic 2 has the mention of car the most. 

To check for its autenticity, we select [Topic 2](https://rawgit.com/mbhushan909/Amazon_Automotive_Reviews_LDAvis/master/Atomotive_5/index.html#topic=2&lambda=0.5&term=) and it becomes clear that car is indeed the most frequent feature of that topic appearing approximately around 2200 times.
Second of all, when you go through the list of features of Topic 2 one can predict that this topic is mostly related to cleaning/washing products related to cars.

Also, there are various topics that co-incide with each other which suggests that they are correlated.
For instance, <b><i>Topic 2 and Topic 10 co-incide</b></i> and therefore when you click on [Topic 10](https://rawgit.com/mbhushan909/Amazon_Automotive_Reviews_LDAvis/master/Atomotive_5/index.html#topic=10&lambda=0.5&term=) to see if it has relavent vocabulary to that of Topic 2, it turns out that the top 30 salient features of Topic 10 also have mentions of words that indicate towards cleansing products for cars or products that enhance the looks of a car, namely: leather, cleaner, smell, conditioner, spray, etc.

Topics 11, 14 and 15 are isolated and do not co-incide with any other topics, thus indicating their independence from other topics. For instance, [Topic 11](https://rawgit.com/mbhushan909/Amazon_Automotive_Reviews_LDAvis/master/Atomotive_5/index.html#topic=11&lambda=0.5&term=) mentions words wiper and blades which on the slider bar has 100% Overall term frequency and Estimated term frequency in this analysis, which proves that these words do ont appear on any other topics in this entire corpus and thus aloofs Topic 11 from the rest.

[Topic 15](https://rawgit.com/mbhushan909/Amazon_Automotive_Reviews_LDAvis/master/Atomotive_5/index.html#topic=15&lambda=0.5&term=) appears to be a topic on tires of automotives and their maintenance. It also has some 100% appearance words like stem and inflate which are unique to this topic alone and perhaps could be reason for its isolation.

Similarly, one may hover over different topics on the left panel of the visualization and look at the top 30 salient features of that topic to understand what that topic indicates about. 

The section below documents the coding that was performed for this analysis.

___
<p align="center"> <b><i>Code</b></i></p>
___
#####Loading the data set
```R
> library(ndjson)
> autocorpus <- ndjson::stream_in("E:/Business Analytics/Homeworks/Homework 8/Automotive_5.json")
> View(autocorpus)
> library(quanteda)
> require(quanteda)
> reviews<- corpus(autocorpus$reviewText, docvars = data.frame(summary=autocorpus$summary))
> reviews<- toLower(reviews, keepAcronyms = FALSE)
```

The following code cleans the data set and creates a DFM
```R
> cleanc <- tokenize (reviews,
+                     removeNumbers = TRUE,
+                     removePunct = TRUE,
+                     removeSeparators = TRUE,
+                     removeTwitter = TRUE,
+                     verbose = TRUE)
Starting tokenization...
  ...tokenizing texts...total elapsed:  1.17000000000007 seconds.
  ...replacing names...total elapsed:  0 seconds.
Finished tokenizing and cleaning 20,473 texts.
> dfm.reviews<- dfm(cleanc,
+                   toLower = TRUE, 
+                   ignoredFeatures =stopwords("english"), 
+                   verbose=TRUE, 
+                   stem=TRUE)
Creating a dfm from a tokenizedTexts object ...
   ... lowercasing
   ... indexing documents: 20,473 documents
   ... indexing features: 40,490 feature types
   ... removed 168 features, from 174 supplied (glob) feature types
   ... stemming features (English), trimmed 9205 feature variants
   ... created a 20473 x 31117 sparse dfm
   ... complete. 
Elapsed time: 8.43 seconds.
> head(dfm.reviews)
Document-feature matrix of: 20,473 documents, 31,117 features.
(showing first 6 documents and first 6 features)
       features
docs    need set jumper cabl new car
  text1    1   1      1    1   1   2
  text2    0   0      1    2   0   0
  text3    4   0      1    4   1   1
  text4    5   1      3   15   0  11
  text5    1   1      0    3   0   0
  text6    0   0      1    5   0   0

```
####Now we take a look at the top 50 features that appear in our data set
```R
> topfeatures<-topfeatures(dfm.reviews, n=50)
> topfeatures
      use       car      work       one      will   product      just       get      like      good 
    16762      9300      8903      7893      6919      6836      6450      6268      6236      5816 
    great       can      well      time      need      look   batteri      make     light      easi 
     5793      5723      5683      4658      4527      4439      3966      3924      3906      3698 
    clean      much    instal       fit    realli      also    better        go     price      keep 
     3690      3411      3332      3302      3065      3058      2839      2838      2834      2799 
    littl      year      nice recommend       oil    filter       wax      tire     water       tri 
     2764      2686      2680      2658      2652      2638      2596      2576      2538      2526 
     even       buy      wash     remov      come    bought       put    replac      last      back 
     2514      2502      2450      2429      2361      2355      2348      2331      2301      2287 
```
And now we iterate our fitted corpus for topic modelling 3000 times that will generate 15 topics in the LDAvis package. K denotes the number of topics and G denotes the number of iterations.
It took closely around 18 minutes to run the entire model.
```R
> K <- 15
> G <- 3000
> alpha <- 0.02
> eta <- 0.02
> library(lda)
> set.seed(357)
> t1 <- Sys.time()
> fit <- lda.collapsed.gibbs.sampler(documents = documents, K = K, vocab = vocab, 
+                                    num.iterations = G, alpha = alpha, 
+                                    eta = eta, initial = NULL, burnin = 0,
+                                    compute.log.likelihood = TRUE)
> t2 <- Sys.time()
> ## display runtime
> t2 - t1
Time difference of 17.69709 mins
> theta <- t(apply(fit$document_sums + alpha, 2, function(x) x/sum(x)))
> phi <- t(apply(t(fit$topics) + eta, 2, function(x) x/sum(x)))
> reviewdata <- list(phi = phi,
+                    theta = theta,
+                    doc.length = doc.length,
+                    vocab = vocab,
+                    term.frequency = term.frequency)
> library(LDAvis)
> library(servr)
> json <- createJSON(phi = reviewdata$phi, 
+                    theta = reviewdata$theta, 
+                    doc.length = reviewdata$doc.length, 
+                    vocab = reviewdata$vocab, 
+                    term.frequency = reviewdata$term.frequency)

> serVis(json, out.dir = 'Atomotive_5', open.browser = TRUE)
```

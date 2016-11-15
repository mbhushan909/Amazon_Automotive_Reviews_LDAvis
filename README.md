# Amazon_Automotive_Reviews_LDAvis
### *By Bhushan Mehta*
### *Tuesday's batch - Fall 2016 - Business Analytics*
Text reviews analyzed using Topic modelling and visualized using LDAvis in R.

---
*LDAvis visualization*
[Link to visualization](https://rawgit.com/mbhushan909/Amazon_Automotive_Reviews_LDAvis/master/Atomotive_5/index.html#topic=0&lambda=0.5&term=)
---

<p align="center"> <b><i>Case</b></i></p>

____

```R
> library(ndjson)
> autocorpus <- ndjson::stream_in("E:/Business Analytics/Homeworks/Homework 8/Automotive_5.json")
> View(vgdata)
> library(quanteda)
> require(quanteda)
> reviews<- corpus(autocorpus$reviewText, docvars = data.frame(summary=autocorpus$summary))
> reviews<- toLower(reviews, keepAcronyms = FALSE)
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
```

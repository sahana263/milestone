Milestone Report -- Data Science Capstone Project
December 25, 2024

Introduction
This milestone report is a part of the data science capstone project of [Coursera](https://www.coursera.org, https://www.coursera.org/learn/data-science-project/home/week/2) and Swiftkey. The main objective of the capstone project is to transform corpora of text into a Next Word Prediction system, based on word frequencies and context, applying data science in the area of natural language processing. This Rmarkdown report describes exploratory analysis of the sample training data set and summarizes plans for creating the prediction model. Text mining R packages tm[1] and quanteda[2] are used for cleaning, preprocessing, managing and analyzing text. This report meets the following requirements:

Downloads, loads the data, creates sample training data and preprocess it.

Generates summary statistics about the data sets and makes basic plots such as histograms to illustrate features of the data.

Describes some interesting findings.

Reports plans for creating a prediction algorithm and Shiny app.

Data Acquisition and Summary Statistics
Data Source
The text data for this project is offered by coursera-Swiftkey, including three types of sources: blogs, news and twitter with four different languages. The English - United States data sets will be used in this report.

Load the libraries
The R packages used here include: quanteda, tm, stringi, downloader, readr, stringr, dtplyr, tibble, ggplot2, rmarkdown, knitr, and ggthemes.

Download and Load the Course Data Sets
Download the data and save to local disk:

[1] "../data_capstone/final/en_US/en_US.blogs.txt"  
[2] "../data_capstone/final/en_US/en_US.news.txt"   
[3] "../data_capstone/final/en_US/en_US.twitter.txt"
Summary Statistics about the Data Sets
file_name file_size (Mb) word_count line_count Max words/line Avg words/line

en_US.blogs.txt 200 37272578 899288 40832 42 en_US.news.txt 196 34309642 1010242 11384 34 en_US.twitter.txt 159 30341028 2360148 140 13 Total 555 101923248 4269678 40832 24

Load the text Data in R and remove non-ASCII characters
Sampling the Data for exploratory analysis
In order to enable faster data processing, a data sample from all three sources was generated, extracting 0.01 of data randomly using rbinoma() function and store them.

# A tibble: 3 × 2
  `sample text` length
          <chr>  <int>
1         sblog   6278
2         snews   8721
3         stwit  22982
Data Cleaning and Preprocessing
Loading bad-word list from here
Create a tm corpus from three kinds of samples
Clean and transform the corpus using stringi() and tm_map()
The cleaning and preprocessing include:

convert to lowercase
remove stopwords: c("will", quanteda::stopwords("english")
remove profanity and other bad words
remove URL: (http, https, atp, www and followings)
remove twitter hash tag and email id
remove Symbols
remove Punctuation including Hyphens using tm::removePunctuation
remove Numbers
Stem words using tm::stemDocument (Porter’s stemming algorithm)
remove white space
Converting tm corpus to quanteda corpus
sample quanteda corpus: 
Corpus consisting of 37981 documents, showing 5 documents.

  Text Types Tokens Sentences author       datetimestamp description heading id language origin
 text1     3      3         1   <NA> 2017-05-17 09:42:41        <NA>    <NA>  1       en   <NA>
 text2    10     10         1   <NA> 2017-05-17 09:42:41        <NA>    <NA>  2       en   <NA>
 text3     9     10         1   <NA> 2017-05-17 09:42:41        <NA>    <NA>  3       en   <NA>
 text4    21     26         1   <NA> 2017-05-17 09:42:41        <NA>    <NA>  4       en   <NA>
 text5    17     27         1   <NA> 2017-05-17 09:42:41        <NA>    <NA>  5       en   <NA>

Source:  Converted from tm VCorpus 'tcorps'
Created: Wed May 17 05:45:55 2017
Notes:   
N-grams and dfm (sparse Document-Feature Matrix)
Creating dfm for n-grams
In statistical Natural Language Processing (NLP), an n-gram is a contiguous sequence of n items from a given sequence of text or speech. Bigram and trigram are combination of two and tree words respectively. We will build and use n-gram model, a type of probabilistic language model, for predicting the next item in such a sequence in the form of a (n − 1)–order Markov model.

Unigram
Document-feature matrix of: 37,981 documents, 34,525 features (100% sparse).
Bigram
Document-feature matrix of: 37,981 documents, 315,159 features (100% sparse).
Trigram
Document-feature matrix of: 37,981 documents, 350,471 features (100% sparse).
Most common ngrams
The most common unigrams
 [1] "get"   "just"  "said"  "like"  "one"   "go"    "time"  "can"   "day"   "love"  "year"  "make" 
[13] "new"   "good"  "thank" "work"  "now"   "know"  "want"  "peopl"

The most common bigrams
 [1] "right_now"      "look_like"      "last_year"      "new_york"       "last_night"    
 [6] "look_forward"   "feel_like"      "high_school"    "year_ago"       "thank_follow"  
[11] "make_sure"      "last_week"      "can_get"        "first_time"     "st_loui"       
[16] "next_week"      "happi_birthday" "good_morn"      "just_got"       "thank_much"    

The most common trigrams
 [1] "happi_mother_day"    "happi_new_year"      "presid_barack_obama" "look_forward_see"   
 [5] "two_year_ago"        "new_york_time"       "new_york_citi"       "let_us_know"        
 [9] "cinco_de_mayo"       "cake_cake_cake"      "st_loui_counti"      "love_love_love"     
[13] "graduat_high_school" "follow_follow_back"  "follow_look_forward" "make_feel_better"   
[17] "thank_follow_us"     "want_make_sure"      "hope_feel_better"    "keep_good_work"     

Some Observations and Issues in the Exploritary Analysis
The three corpora of US english text are around 200, 196, and 159 Megabytes respectively. The twitter corpus has shorter lines, not exceeding 140 "words" per line; while the blogs has the longest line.

Bigrams and trigrams should be formed within a sentence, not crossing the sentences.

Cleaning and other preprocessing may make the sentence boundaries vague or destroyed. We may use special tokens to mark the beginning and ending of each sentence before converting to lower case.

Trigrams such as "follow_follow_back" and "love_love_love" should not happen by the ngrams functions. Need to avoid them Or filter them.

Word stemming is necessary, but it may result in something like "peopl", "citi", "happi", "good_morn", "st_loui_counti", "cinco_de_mayo". Restoring some stemmed words might need a lot of work. Any better ways?

Removing the stopwords is necessary concerning the memory size and speed. But the stopwords might be necessary to get real world phrases in the final next-word prediction.

Data size, memory, speed and accuracy are the challenges, especially for very limited resources (such as x86-64, windows 7 with 8GB RAM).

Plans for creating a prediction algorithm and Shiny app
Split the original data randomly into training, held-out and test data set with 60%, 20% and 20% ratio.

Rewrite the cleaning and preprocessing functions. Tokenize as "sentence" at first before converting to lower case and removing punctuation. Find out better ways to handle "stemming" and "stopwords" issues.

Clean and preprocess the training, held-out and test sets exactly the same way. Test data should not be touched in the model building process, but should have the same feature variables as training data. But in the reality the test data may have words that are not in the training sets. (Please correct me if my understanding is incorrect.)

Create unigrams, bigrams and trigrams from the training data. Remove singletons and sparse terms.

Want to build an interpolated modified Kneser-Ney smoothing next word prediction model. Will try to compile on Windows 7 the KenLM package (in C++), which seems superior in memory demand, performance, accuracy and speed. But KenLM is not found in CRAN. Any suggestion?

Apply the model to the held-out data set to evaluate and tune the model.

Apply the word prediction model to the test data sets to predict the next word.

Create a shiny App and publish it at "shinyapps.io" server.

Any corrections and suggestions would be deeply appreciated.

References:
[1] Ingo Feinerer and Kurt Hornik (2017). tm: Text Mining Package. R package version 0.7-1. https://CRAN.R-project.org/package=tm; Ingo Feinerer, Kurt Hornik, and David Meyer (2008). Text Mining Infrastructure in R. Journal of Statistical Software 25(5): 1-54. URL: http://www.jstatsoft.org/v25/i05/.

[2] Benoit, Kenneth and Paul Nulty. (2017). "quanteda: Quantitative Analysis of Textual Data". R package version: 0.9.9-24. URL https://github.com/kbenoit/quanteda; https://cran.r-project.org/web/packages/quanteda/index.html; http://quanteda.io/articles/development-plans.html

Appendix
The Rmarkdown code index.Rmd can be found in my github repository

Session Info

R version 3.3.2 (2016-10-31)
Platform: x86_64-w64-mingw32/x64 (64-bit)
Running under: Windows 7 x64 (build 7601) Service Pack 1

locale:
[1] LC_COLLATE=English_United States.1252  LC_CTYPE=English_United States.1252   
[3] LC_MONETARY=English_United States.1252 LC_NUMERIC=C                          
[5] LC_TIME=English_United States.1252    

attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
 [1] quanteda_0.9.9-24 tm_0.7-1          NLP_0.1-10        ggthemes_3.4.0    ggplot2_2.2.1    
 [6] tibble_1.2        dplyr_0.5.0       stringr_1.2.0     readr_1.0.0.9000  stringi_1.1.2    
[11] downloader_0.4    knitr_1.15.1     

loaded via a namespace (and not attached):
 [1] Rcpp_0.12.9         ca_0.70             magrittr_1.5        munsell_0.4.3      
 [5] lattice_0.20-34     colorspace_1.3-2    R6_2.2.0            fastmatch_1.1-0    
 [9] plyr_1.8.4          tools_3.3.2         parallel_3.3.2      grid_3.3.2         
[13] data.table_1.10.4   gtable_0.2.0        DBI_0.6             htmltools_0.3.5    
[17] RcppParallel_4.3.20 yaml_2.1.14         lazyeval_0.2.0      assertthat_0.1     
[21] rprojroot_1.2       digest_0.6.12       Matrix_1.2-8        SnowballC_0.5.1    
[25] slam_0.1-40         evaluate_0.10       rmarkdown_1.3       labeling_0.3       
[29] scales_0.4.1        backports_1.0.5    

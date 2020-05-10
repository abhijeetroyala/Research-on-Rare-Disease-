# Research-on-Rare-Disease-

List of Contents:

1.	Data Scrapping and Processing
a)	Reddit Data Scrapping
b)	PubMed Data Scrapping
2.	Word to Vector Model for PubMed and Reddit Data
3.	Labeling Data
4.	Long-Term Short-Term Memory and Bi-Directional Long-Term Short-Term Memory Models
a)	LSTM
b)	Bi-LSTM
5.	Sjogren Dataset

## 1)	Data Scrapping and Processing:
Initially scrapped the data for Sjogren disease from two different sources namely Reddit and PubMed. Reddit is a user forum and PubMed is one of the prominent search engines for biomedical journals.
### a)	Reddit Data Scrapping:
The data from Reddit contains is split into to file one includes the information related to the posts like link, timestamp etc.(posts.csv) and other has processed text data of post’s body and comments. The processing here refers to tokenizing the text data into sentences (your.csv).

Code File: Reddit_Data_Scraping.ipynb

### b)	PubMed Data Scrapping:
#### Part 1:
The data scraping was done using PubMed’s API. The data from PubMed contain only the titles of the papers (sjogren_pubmed.txt)

Code File: Pubmed_Data_Scraping.ipynb
#### Part 2:
Later I scrapped the data for five different diseases namely Sjogren, Asthma, Yellow Fever, Diabetes and Alzheimer’s from PubMed. This data contains the tile along with the abstract texts of the papers for a respective disease. I tokenized this data into sentences and formed datasets for each disease individually. For each disease there are two dataset one with only titles and other with abstract texts. Example for Sjogren the title file is named sjogren_pubmed.txt and for abstract the file is named sjogren_pubmed_abs.txt

Code File: Pubmed_Data_Scraping.ipynb
Data File: 5 diseases

## 2)	Word to Vector Model for PubMed and Reddit Data:
Two word to vector models, one each using PubMed and Reddit data were built without using any pretrained model. Then following things were performed for using each model:
•	t-SNE plot was made to visualize the data.
•	Distance function was defined which filters the words based on distance (cosine similarity score) from a given word.
•	Visual network graph representing the distance of a word from word Sjogren was developed.
•	Search function was defined, which basically is a two layered breath first search function. This takes the input word and gives topn nearest word and their cosine similarity, which forms first layer and again gives topn word and their cosine similarity for each word in first layer which forms second layer.

Word2vec Model for Reddit Data:
Code File: W2V_reddit_data.ipynb
Input Data File: your.csv
Word2vec Model for PubMed Data:
Code File: W2V_pubmed_data.ipynb
Input Data File: sjogren_pubmed.txt

List of words whose similarity score was greater than 0.9 was extracted from each w2v model (PubMed and Reddit) and compared against each other. Extracted the words which were there in Reddit but not in PubMed and the sentences containing these words and vice versa (i.e. in PubMed but not in Reddit).

Reddit list pf words: reddit_list.txt
PubMed list of words: pubmed_list.txt
Code File: reddit_pubmed_list_difference.ipynb

## 3)	Labeling data:
Selected 200 sentences randomly from three different diseases and manually labeled the sentences based on the presence of any causal effect relationship in sentence. I crosschecked this labeling by comparing the same sentences labeled by one other person. Then we collected all the sentences where both of us agreed that the sentences have a causal effect relationship and stored them in matched_sentences.txt file, there were 100 sentences in this. Then I manually extracted the relationship word from each sentence and made a list of them relationship_words_list.txt. 

I combined all the sentences five diseases which had around 300,000 sentences and then using the above list of relationship words, I labeled all the sentences in following manner: If the sentence has any of the word form the list then it is labeled “1” or else the sentence is labeled “0”. The data file was 300k_labeled_sentences.zip. Then performed some data cleaning and removed some sentences to obtain final data set which was 227k_labeled_sentences.zip, this had around 227,000 sentences.

Code File: 600_manually_labeled_sents.ipynb and relationship_labeling.ipynb

## 4)	Long-Term Short-Term Memory and Bi-Directional Long-Term Short-Term Memory Models:
In order to automate the process of causal-effect relationship extraction from sentences, I built LSTM and BI-LSTM using 227k_labeled_sentences.zip dataset which I scraped previously.
### a)	Long-Term Short-Term Memory Model (LSTM):
Long-Term Short-Term Memory neural network architecture allows for the sequential information to flow only in forward direction which means the architecture preserves information only from the past because the only inputs it has seen are from the past.

I developed a LSTM model using 70:30 train-test split, whose class wise (class 0 and class 1) performance was as follows:
 
Code File: LSTM_pubmed.ipynb

### b)	Bi-Directional Long-Term Short-Term Memory Mode (Bi-LSTM):
Bi-Directional Long-Term Short-Term Memory is an extension of LSTM architecture. Unlike LSTM, this architecture allows for the sequential information to flow in both forward and backward direction which helps in dealing with loss of information when the length of sequence is large. That means the architecture preserves information from the past and future.

I developed a LSTM model using 70:30 train-test split, whose class wise (class 0 and class 1) performance was as follows:
 
Code File: Bi-LSTM_pubmed.ipynb

## 5)	Sjogren Dataset:
We wanted to create a dataset for Sjogren, for that I used abstract text data for Sjogren which I collected previously sjogren_pubmed_abs.txt had around 50,000 sentences. From these 50k sentences I extracted all the sentences which have the word Sjogren in it. Then made some data cleaning steps like changing the different spellings of Sjogren to one spelling, removing some sentences which had irrelevant characters and formed a final dataset of length 3595 sentences named sjogren_dataset_final.txt.

Code File: sjogren_dataset.ipynb

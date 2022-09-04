### Text Processing
1.  Remove Html characters, punctuation
2. Stop word removal
3. lower case conversion
4. Stemming : [ Porter Stemmer](https://www.nltk.org/_modules/nltk/stem/porter.html), [Snowball Stemmer](https://www.nltk.org/api/nltk.stem.snowball.html?highlight=snowball%20stemmer)
5. Lemmitizatiom : break a sentence into word  [Ref1](https://stackoverflow.com/questions/1787110/what-is-the-true-difference-between-lemmatization-vs-stemming), [Ref2](https://blog.bitext.com/what-is-the-difference-between-stemming-and-lemmatization/) 

### Convert text to Vectors Embedding 
#### Bag of Words (BOW) 
1. Semantic meaning of word is lost
2. Types: Binary/Boolean BOW,   Count BOW,   Uni-gram/ Bi-gram/N-gram
#### TF-IDF Term Frequency Inverse Documents Frequency  
1. Semantic meaning of word is lost [Ref1](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfTransformer.html#sklearn.feature_extraction.text.TfidfTransformer) [Ref2](https://towardsdatascience.com/tf-idf-for-document-ranking-from-scratch-in-python-on-real-world-dataset-796d339a4089)
#### Word2Vec
1. Semantic information learned by the vector [Tensorflow Reference](https://www.tensorflow.org/tutorials/text/word2vec) 
2. [Word Embedding](https://www.tensorflow.org/text/guide/word_embeddings) 
3. Avg Word2Vec
4. [TFIDF Weighted Word2Vec](https://medium.com/analytics-vidhya/featurization-of-text-data-bow-tf-idf-avgw2v-tfidf-weighted-w2v-7a6c62e8b097)
#### Positional Embedding
1.  [Semantic and  information about the Position in a sentence is captured](https://kazemnejad.com/blog/transformer_architecture_positional_encoding/)
2. [Keras Position Embedding](https://keras.io/api/keras_nlp/layers/position_embedding/)
3.  


- RNNs (LSTMs / GRUs)  :  Word Embedding with [TF-IDF Weighted Word2Vec](NLP#word2vec)
- Transformers  : Absolute Or Relative [Positional Embedding](https://theaisummer.com/positional-embeddings/#positional-encodings-vs-positional-embeddings)
## Quick Notes
1. Transformers are more efficient in parallel processing than LSTMs [Ref 1](https://voidful.medium.com/why-transformer-faster-then-lstm-on-generation-c3f30977d747#:~:text=That's%20all%20for%20transformer%20model,neural%20network%20such%20as%20LSTM.), [Ref 2](https://ai.stackexchange.com/questions/20075/why-does-the-transformer-do-better-than-rnn-and-lstm-in-long-range-context-depen) 


## References
1. [Regular Expression Blog](https://pymotw.com/2/re/) 
2. [Gensim](https://radimrehurek.com/gensim/auto_examples/index.html#documentation)


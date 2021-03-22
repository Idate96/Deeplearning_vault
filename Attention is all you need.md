Type: #paper

## General Thoughts 
Authors would like to get away from [[RNNs]]. 

If you attempting translation of: "the cat eats the mouse". With an RNNs you would feed it the word vector and using the previous hidden state and the network input the hidden state is updated. 
![[rnn_example.png]]

You then feed the last hidden state to a decoder to predict the first translated word. 
Ideally we want the decoder to able to pay attention (give more weight) to certain hidden state of the encoder. 
![[rnn_decoder.png]] 

## Summary


## Analysis
The source sentence goes to the first stack while the sentence we have translated so far ("die katz ....") goes to the second stack. 
There are no multistep backprop in transformers. 

The input and output embeddings are word vectors. 
The [[positional encodings]] use sinosoidal function (continous way) to binarily encode their position. 
![[positional_encodings.png]]

There are three kind of attentions:

1. Attention over the input sentece, which has to be encoded (all at once) into one. Discovers interesting things about the input and builid key value pairs
2. Over the output sentence 
3. It unites the source and the target sentence that you have produced so far

To the third attention there are three connection:
![[multihead.png]]
1. Values (V): bunch of things that are interesting
2. Keys (K): way to index the values 
3. Queries (Q): I would like to know certain things


The attention dot products the keys and the queries: keys are vectors and each key has an associated value, when we introce a query by taking the dot product with *each* of the key. 
Then we compute the [[Softmax]]: this selects the dot product resulting from the "most" aligned key with the query. 
The softmax is multiplied with the values: this basically weights the values. 



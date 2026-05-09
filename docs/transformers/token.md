# Token

A token is the smallest possible unit an LLM uses to process and understand text. Since LLMs can only understand numbers (to perform math/computation), each unique token is assigned a specific ID (number). This allows it to start building context on each individual token and assign value to it.

## Token Limits

Token limits occur because every token requires a bit of memory and compute power and models have a maximum "context window".

## Out-Of-Vocabulary

OOV occurs when the LLM finds a token it doesn't have in its token vocab. In general, LLMs have a base set of tokens that it can use to understand text.

## Tokenization

Tokenization is the process of splitting up a sentence into multiple tokens.

**Common Types:**

`Word-Level` - Self-explanatory, split up a sentence by its words. Problem is you run into OOV often.
`Subword-Level` - Most ideal one. You split sentences based on prefix, suffix, subwords, etc. Good tradeoff for speed and OOV
`Character-Level` - Split up sentences into each individual letter. Best for avoiding OOV, but computation gets incredibly slow and expensive.

## Embedding

Since LLMs only understand numbers, tokens are typically represented as a continuous vector of numbers. This vector gives the token a specific meaning. Similar words tend to have very similar embeddings. We can then map this on a n-dimensional plane to plot similar tokens to each other. The main thing to understand is these embeddings are learned through model training rather than being inherently assigned. We kind of just trust that through repetitive training, the model will generate these embeddings for us.

> This was a successor to `one-hot encoding` where every word was a simple index in a giant list. This proved to be flawed since every word was equally different, so you couldn't tell the meaning of each word. Embeddings solve this byu assigning each token a fixed-length list of numbers (vector)

### word2vec

word2vec is a popular framework that helps create high quality embeddings using a shallow, two-layer neural network. Instead of looking at a dictionary to generate meanings, it looks at millions of sentences and trains on it to understand which words have relationships to each other.

Typically to train this neural network, `Continuous Bag of Words (CBOW)` and `Skip-gram` are used. In CBOW, the model looks at the surrounding "context" words and tries to predict the missing word. In skip-gram the model takes one word and tries to predict its surrounding words. Through this long process, the model is able to take in a token and spit out a high-quality embedding. We don't actually care about the neural network itself, but rather just its output (which becomes the embedding layer)

> Note: This only generates static embeddings. A "river bank" and a "savings bank" has the exact same embedding for bank. Contextualization is needed to change the embeddings.

### Text -> Embedding

1. `Tokenization` - The sentence is broken up into tokens
2. `Lookup Table` - Each unique token has a corresponding (embedding) row in a massive matrix (`Embedding Layer`). This layer acts as a sort of token alphabet for the LLM. This is generated after learning has completed
3. `Contextualization` - This is where these embeddings are refined based on the `attention` mechanism to change the meaning of the embeddings to better match the context of the sentence

## Positional Embedding

While embeddings help give words meaning to the LLM, the position of a word in a sentence will drastically change the context.

### Hardcoded Position Embeddings

The original way positional encodings were decided were through hardcoded values that used sin/cos functions. This worked because sin/cos have values between [0, 1]. Due to their unique natures, the LLM is easily able to determine the position of each word. This generated embedding is then added to the input embedding to give a further nudge.

### Attention with Linear Biases (ALiBi)

This is relative positioning. It's a way to inject positional information through the use. Introduced to solve the "length extrapolation" problem where models in the past for hardcoded embeddings, the model could only handle fixed lengths of inputs. It achieves relative positioning by increasing a constant bias that increases linearly as the distance between two tokens increases.

### Rotary Position Embeddings (RoPE)

This is the most popular strategy nowadays. While hardcoded positional embeddings add values to the input embedding, RoPE rotates them instead. By rotating the embedding vector, it lets the LLM know the position of the token. When calculating the attention between two words, the result depends on the relative angle between them. This allows a model understand how far words are from each other.

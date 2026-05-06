# Attention

Attention is the concept that gives embeddings contextual meaning in sentences. At a high level overview, a token will use the surrounding tokens in the sentence to help nudge it towards a certain meaning.

## Query, Key, Value

This is how attention works mathematically. Scaled Dot-Product Attention is the most common type to ensure the scores don't get too large. We use a softmax function to make sure all weights are between 0-1

`Query (Q)` - The embedding of the current token in the sentence
`Key (K)` - The embedding of another token in the sentence
`Value (V)` - The actual weight of the key word

What happens is we multiple `Q * K^T` to find how similar the word is to each other. This helps us determine how much the key will influence the query. From there, the actual value of the key is multiplied with the weight to help create the attention which will nudge the embeddings a bit.

## Types

`Self-Attention` - A word in a sentence looks at other words in the same sentence to understand its own context
`Cross-Attention` - Typically used in translation: the model looks at the original sentence while generating the output sentence
`Multi-Head Attention` - Model uses several "heads" to look at different aspects of the text at once (ie. one head for grammar, one for meaning, etc)

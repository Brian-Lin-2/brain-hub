# Model

## Target Type

In a model, a target type is actually what a model is trying to predict.

**Common Types:**
`Unstructured` - Most commonly used. Used when model doesn't fit a standard schema. Allows you to pass arbitrary data (blobs, JSON, etc) as both input and output (ie. a generative model that takes in a text prompt and spits out an image)
`Binary Classification` - Model has only two possible outcomes (ie. cat or dog)
`Multi-Class Classification` - Model has three or more distinct categories. However, only one category is picked at the end. This typically requires class labels to be classified
`Multi-Label Classification` - Similar to multi-class classification, but the model allows a single row to be associated with multiple categories
`Regression` - Used for predicting a continuous numerical value (ie. predicting the temperature tomorrow)
`Forecasting` - Model predicts a series of future values. (ie. predicting the temperature for the next week)
`Anomaly Detection` - Model looks at "normal" patterns in the data and flags rows that are statistically different (ie. model to predict fraud)
`Clustering` - Model groups similar data points together based on shared characteristics

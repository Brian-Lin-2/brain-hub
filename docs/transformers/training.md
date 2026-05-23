# Training

## Training Steps

`Data Preparation` - This is where you prepare the dataset needed for the model to train on
`Initialization` - This is where you define the model's structural blueprint (ie. parameters) and how it will process information
`Training` - Consists of three phases:

1. `Forward Pass` - This is when we feed input into the model to compute its loss
2. `Backwards Pass` - This is where back-propagation helps us compute the gradients (needed for weight update)
3. `Weight Update` - This is where the optimizer takes those computed gradients and helps the model updates its weights

## Training Types

### Pretraining

This is where a model is trained on large amounts of text to get a generic high-level NLP. This allows an LLM to understand language.

### Finetuning

Instead of training a model a model from scratch, we can take a pretrained model and finetune it for our use-case.

#### LoRA

A major problem of finetuning is that it's resource intensive and requires a lot of GPUs. Low-Rank Adaptation (LoRA) solves this by freezing the weights of the pretrained model and then adding a smaller layer ontop of it that it uses to finetune the model. This will help nudge the model towards the designed outcome.

**Drawbacks:**
LoRA is only as effective as the base model is. Since the added matrix layer is so small, you can't really do many complex tasks. It is really used to teach a model how to act (ie. instruction training, formatting). If you need to teach models new facts, finetuning is necessary.

It also struggles when the batch size increases. Since the matrix is so small, huge amounts of data can muddle the weights

> Since you're only training a slim additional matrix, you can easily swap it out for different tasks.

### QLoRA

A technique that combines model quantization and LoRA to massively save on memory. Regular LoRA freezes base model weights at FP16 (16 bytes), but QLoRA compresses these frozen weights down to NF4 (4 bytes).

### Preference Tuning

Helps teach a model what humans actually prefer. It's also referred to as alignment which ensures that the model is safe and doesn't give dangerous advice.

## Chinchilla Law

A law that focuses on data efficiency of model. It states that you need roughly 20 tokens of data for every 1 parameter in your model for optimal efficiency.

## Knowledge Cutoff

AI only has as much knowledge as it was last trained on. This makes it hard to learn new data after a while.

## Parallelism

When training LLMs, a single GPU often runs out of memory or takes too long to process the data. To solve this, we distribute the workload across multiple GPUs.

### Data Parallelism

Idea is to divide batches of data across devices. Model is also replicated on each device. This allows for parallelized training. The only problem is wasted space as the model's parameters, gradients, and optimizer must be cloned.

#### ZeRO

Stands for Zero Redundancy Optimization. Idea is to optimize redundant information across devices by sharding them.

`ZeRO-1` - Every cloned model will share an optimizer
`ZeRO-2` - Every cloned model will share optimizer + gradients
`ZeRO-3` - Every cloned model will share optimizer + gradients + parameters

### Model Parallelism

We do this when the model is too big to fit into the memory of a single GPU. Therefore, we split the model up and a single piece of data needs to travel across multiple GPUs to finish a single pass.

## Mixed Precision Training

Speeds up model training and slashes VRAM usage by using FP16 and FP32 bit floating points at the same time. In the past, models were trained entirely on FP32. Mixed precision abuses the fact that models can still do calculations with lower precision formats without hurting the accuracy of the final model.

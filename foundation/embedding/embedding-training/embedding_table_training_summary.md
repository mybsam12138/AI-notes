# How the Embedding Table Is Trained Even Though It Starts Random

## Core idea

At the beginning of training, the embedding table does **not** contain meaningful word representations.  
Each token is assigned a vector of small random numbers.

That is normal.

The embedding table becomes meaningful **during training**, because it is a set of **trainable weights** updated by gradient descent, just like other neural network parameters.

---

## What the embedding table is

If the vocabulary size is `V` and the embedding dimension is `d`, then the embedding table is:

```text
E ∈ R^(V × d)
```

- `V` = number of tokens in the vocabulary
- `d` = size of each embedding vector

Each row of `E` is the embedding vector for one token.

For example:

```text
E["cat"]  = [0.12, -0.08, 0.33, ...]
E["dog"]  = [-0.05, 0.21, 0.10, ...]
```

At initialization, these values are random and meaningless.

---

## Important correction

The model is **not** usually trained like this:

```text
input:  cat
output: sentence around cat
```

In modern language models, training is usually based on a prediction task such as:

```text
input:  "I like"
target: "cat"
```

or

```text
input:  "The cat sat on the"
target: "mat"
```

So the model learns:

- given a context
- predict the correct next token

The embedding table is trained **as part of that larger task**

---

## Why random embeddings are not a problem

A common confusion is:

> If the embedding vectors are random at first, and the output is also random at first, how can learning even begin?

The answer is:

- the model prediction is random at first
- but the **correct label is not random**
- the difference between prediction and truth creates a **loss**
- that loss creates a **gradient**
- the gradient tells the model how to update its parameters

So the model does not need meaningful embeddings at the beginning.  
It only needs:

1. an input
2. a prediction
3. a correct answer
4. a loss function
5. backpropagation

---

## Step-by-step training flow

### 1. Tokenize text

Example:

```text
"I like cat"
```

may become token IDs like:

```text
[15, 204, 891]
```

---

### 2. Look up embeddings

The model uses token IDs to select rows from the embedding table:

```text
E[15], E[204], E[891]
```

These vectors are random at first.

---

### 3. Build internal representation

The embedding vectors are passed through the model layers  
(for example, transformer layers).

This produces a hidden representation of the context.

---

### 4. Predict the next token

The model produces a score for each possible token in the vocabulary.

After softmax, this becomes a probability distribution such as:

```text
P(cat)  = 0.04
P(dog)  = 0.10
P(car)  = 0.20
...
```

At first, these probabilities are poor because the model is still random.

---

### 5. Compare with the true answer

If the correct next token is `"cat"`, but the model gives `"cat"` a low probability, the loss is high.

For example:

```text
target = "cat"
prediction = low probability for "cat"
loss = high
```

---

### 6. Backpropagation updates the parameters

The loss is used to compute gradients.

Those gradients update:

- the transformer weights
- the output projection weights
- the embedding table itself

This is the key point:

> The embedding table is trainable, so its rows are updated during backpropagation.

---

## Why the input embedding also gets updated

Another common confusion is:

> I thought training only updates `W` and `b`. Why does the input embedding get updated?

Because the embedding table **is also a weight matrix**.

The embedding lookup can be viewed as:

```text
embedding = one_hot(token_id) × E
```

where:

- `one_hot(token_id)` is the token input
- `E` is the embedding matrix

So `E` is just another learned weight matrix.

That means when the loss is backpropagated, the corresponding rows of `E` are updated too.

---

## Simple intuition

Suppose the training example is:

```text
input:  "I like"
target: "cat"
```

If the model predicts `"car"` instead of `"cat"`, then gradient descent will try to:

- change the representation of `"I like"` so it is more useful
- change the embedding of `"cat"` so it aligns better with the right contexts
- push wrong tokens away when appropriate

Over many examples, the embedding for `"cat"` is gradually shaped by the contexts in which `"cat"` appears.

That is why meaning emerges.

---

## Why similar words become close in embedding space

Words like `"cat"` and `"dog"` often appear in similar contexts.

Because they receive similar update signals across many training examples, their embeddings gradually move into similar regions of vector space.

This is not manually programmed.

It emerges from the training objective.

---

## What is actually learned

The model is not directly learning dictionary definitions.

It is learning something more like:

- which words fit which contexts
- which words behave similarly in language
- which patterns help reduce prediction error

The embedding table becomes meaningful because it supports those goals.

---

## Very important conclusion

The embedding table does **not** need to start meaningful.

It starts as random numbers.

It becomes meaningful because:

1. the model is trained on a prediction task
2. prediction errors produce a loss
3. the loss produces gradients
4. gradients update the embedding matrix
5. repeated updates gradually organize tokens into a useful semantic space

---

## One-sentence summary

The embedding table starts as random noise, but through repeated prediction errors and gradient updates, it becomes a learned representation where tokens that behave similarly in language end up with similar vectors.

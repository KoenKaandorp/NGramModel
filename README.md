# Methods

This project implements a statistical language model based on N-grams, trained on text from *The Lord of the Rings* (LOTR) corpus. The approach relies on estimating word sequence probabilities from frequency counts and applying backoff and pruning strategies to improve robustness.

## N-gram Language Model

An N-gram model estimates the probability of a word given a fixed-length context of the previous $n-1$ words. Formally:

$$
P(w_n \mid w_1, w_2, ..., w_{n-1}) = \frac{C(w_1, w_2, ..., w_n)}{C(w_1, w_2, ..., w_{n-1})}
$$

where $C(\cdot)$ denotes the frequency count observed in the training corpus.

By varying $n$, different models are obtained:

* Bigram model: $n = 2$
* Trigram model: $n = 3$
* General N-gram model: $n \geq 2$

Higher-order models capture more context but suffer more from data sparsity.

## Backoff Strategy

To address sparsity, a backoff mechanism is used. When a higher-order N-gram is not observed in the training data, the model falls back to a lower-order model.

For example, in a trigram model:

$$
P(w_n \mid w_{n-2}, w_{n-1}) =
\begin{cases}
\text{trigram probability} & \text{if } C(w_{n-2}, w_{n-1}, w_n) > 0 \
P(w_n \mid w_{n-1}) & \text{otherwise}
\end{cases}
$$

This process can continue recursively down to unigrams if necessary. Backoff ensures that the model can always produce a probability estimate, even for unseen sequences.

## Pruning

To reduce noise and improve efficiency, low-frequency N-grams are removed from the model. This is done by applying a threshold:

$$
\text{keep N-gram if } C(w_1, ..., w_n) \geq \tau
$$

where $\tau$ is a predefined minimum count.

Pruning reduces memory usage and removes unreliable transitions, but increases reliance on backoff.

## Training Data

The model is trained on text from *The Lord of the Rings*. This corpus provides a consistent narrative style and vocabulary, allowing the model to learn domain-specific word patterns and structures.

## Summary

The model combines:

* Maximum likelihood estimation for N-gram probabilities
* Backoff to handle unseen contexts
* Pruning to reduce sparsity and noise

This results in a probabilistic language model that captures local word dependencies within the LOTR corpus.

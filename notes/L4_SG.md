**Study Guide: Lecture 4 - Language Models**

**1. Core Concepts of Language Modeling**

- **Definition:** A language model computes the probability distribution of the next word given a sequence of previous words (the prefix).
- **Goal:** By modeling $P(w_{next} | w_1, w_2, ..., w_n)$, the model can generate text and rank the likelihood of possible sentences (e.g., recognizing that "Today is Tuesday" has a higher probability than "Tuesday Today is").
- **The Chain Rule:** The probability of an entire sentence is the product of all its conditional word probabilities: $P(w_1, ..., w_T) = \prod_{t=1}^{T} P(w_t | w_1, ..., w_{t-1})$.

**2. N-Gram Language Models & MLE (Highly Testable)**

- **Markov Assumption:** Because calculating the probability based on an entire long sentence is combinatorially massive, N-gram models make a simplifying assumption: the next word only depends on the preceding $n-1$ words.
    - _Bigram:_ Depends on 1 previous word.
    - _Trigram:_ Depends on 2 previous words.
- **Maximum Likelihood Estimation (MLE):** To calculate the probabilities, you simply count frequencies in the training corpus.
    - **Formula:** $P(w_i | w_{i-1}) = \frac{Count(w_{i-1}, w_i)}{Count(w_{i-1})}$.
    - _Midterm Tip:_ Be ready to compute this by hand. If asked for $P(\text{"am"} | \text{"I"})$ and the phrase "I am" appears 3 times, while the word "I" appears 3 times total, the probability is $3/3 = 1$.

**3. The Sparsity Problem & Zipf's Law**

- **Zipf's Law:** The $r$-th most common word has a frequency proportional to $1/r$. This explains why a few words are extremely frequent, but the vast majority of words are rare (the "long tail").
- **Sparsity Issues:** Because of Zipf's law, many valid word combinations will simply never appear in the training corpus.
    - _Problem 1:_ If the exact n-gram never occurred, the numerator is 0, giving the entire sentence a probability of 0.
    - _Problem 2:_ If the context (prefix) never occurred, the denominator is 0, making it impossible to calculate.

**4. Smoothing Techniques (Testable Math)** _Midterm Tip: You must know how to adjust the MLE denominator when smoothing is applied!_

- **Add-One (Laplace) Smoothing:** Pretend we saw every possible word combination one more time than we actually did to avoid multiplying by zero.
    - **Formula:** $P_{Add-1}(w_n | w_{n-1}) = \frac{Count(w_{n-1}, w_n) + 1}{Count(w_{n-1}) + V}$.
    - _Crucial detail:_ You must add the entire vocabulary size ($V$) to the denominator to ensure the probabilities still sum to 1.
- **Add-Lambda Smoothing:** Adding a full "1" steals too much probability mass from frequent words, especially when the dictionary is large ($V \approx 20,000$).
    - Instead, we add a fractional hyperparameter $\lambda$ (e.g., 0.01).
    - Adding $\lambda = 1000$ smooths too much (high bias), while $\lambda = 0.001$ smooths too little (high variance). You select the best $\lambda$ using a separate development dataset.

**5. Neural Language Models (Directly tied to Quiz 1)**

- **Fixed-Window Neural LMs (Feedforward):** Concatenates the word embeddings of a fixed number of previous words and passes them through hidden layers to predict the next word's output distribution.
    - _Pros over N-grams:_ No sparsity issues (assigns non-zero probability to unseen combinations) and natively captures similarity between words.
    - _Cons (Tested in Quiz 1):_ The context window is still strictly fixed, so it cannot handle variable input lengths, nor can it capture dependencies outside that window.

**6. Recurrent Neural Networks (RNN LMs)**

- **Core Principle:** Applies the exactly same parameter matrices ($W_h, W_e$) repeatedly at every time step. This allows the network to process sequences of any length naturally.
    - $h^{(t)} = \sigma(W_h h^{(t-1)} + W_e e^{(t)} + b_1)$.
- **Symmetry:** Unlike feedforward LMs, RNNs process data symmetrically by sharing weights across all positions.
- **The Vanishing Gradient Problem:** When performing backpropagation through time over long sentences, gradients are multiplied repeatedly by values often between 1 and -1. The gradient shrinks exponentially close to zero.
    - _Consequence:_ The RNN struggles to update its weights based on information from many steps back, meaning it practically fails to learn long-term dependencies.
- **LSTMs (Long Short-Term Memory):** Solves the vanishing gradient problem by using distinct "gates" (forget, input, output) to explicitly control what information to keep or forget in a memory cell.

---

**Midterm Prep Strategy for Lecture 4:**

1. Re-read **Quiz 1, Q1** to confirm you clearly understand why Feedforward Neural LMs are limited by their fixed-size context window.
2. Be ready to compute an **Add-One smoothed bigram probability**. Remember to add $+1$ to your target cell's count, but add $+V$ (the total number of unique words in the vocabulary) to the row's total count.

Let me know when you are ready to tackle the final piece: **Lecture 5 (Seq2Seq & Transformers)**!

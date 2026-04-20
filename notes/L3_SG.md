**Study Guide: Lecture 3 - Word-Level Representation**

**1. Discrete Word Representations & Taxonomy**

- **One-Hot Vectors:** The most basic way to represent words, where a word's vector has a "1" at its index and "0" everywhere else.
    - _Midterm Tip - Limitations of One-Hot:_ The dimensionality is massive (equal to vocabulary size, e.g., 13M for Google 1T data) resulting in highly sparse vectors. Crucially, **they cannot capture similarity** because the dot product of any two different one-hot vectors is exactly 0 ($v_{happy} \cdot v_{sad} = 0$). They also cannot represent new out-of-vocabulary words.
- **Taxonomy (WordNet):** Grouping words manually by categories like synonyms, hypernyms (Is-A), and hyponyms.
    - _Limitations:_ It requires intense human labor and is highly subjective. It misses subtle nuances (e.g., listing "proficient" as a strict synonym for "good") and fails to capture new cyber lingo or slang (e.g., "badass", "nifty").

**2. Distributional Representation & Co-occurrence**

- **Core Principle:** "A word is characterized by the company it keeps" (John Firth, 1957). Linguistic items occurring in similar contexts have similar semantic meanings.
- **Pointwise Mutual Information (PMI):** Measures whether events $x$ and $y$ co-occur more than if they were strictly independent.
    - **Formula:** $PMI(word1, word2) = \log_2 \frac{P(word1,word2)}{P(word1)P(word2)}$.
    - **Range:** PMI ranges from $-\infty$ to $+\infty$.
- **Positive Pointwise Mutual Information (PPMI) (Highly Testable):** Negative PMI values mean two words "avoid" co-occurring, but this signal is often noisy and unreliable. To fix this, we replace all negative PMI values with 0, creating PPMI.
    - **Formula:** $PPMI(word1, word2) = \max(\log_2 \frac{P(word1,word2)}{P(word1)P(word2)}, 0)$.
- **Low-Rank Approximation:** You can store co-occurrence or PPMI data in a massive matrix and use Singular Value Decomposition (SVD) to compress it into a smaller number of dimensions ($d \times k$), capturing the most important semantic information.

**3. Word2Vec: Learning Embeddings via Shallow Neural Networks**

- **Skip-gram Model:** Predicts the surrounding _outside_ words (context) given the _center_ word.
- **Two Vectors Per Word (Testable Parameter Structure):** Every word has two distinct vectors that act as parameters learned via gradient descent:
    - $v_w$: Used when the word is the **center word**.
    - $u_w$: Used when the word is the **outside/context word**.
    - The inner product $v_{center} \cdot u_{outside}$ will be high if the outside word frequently appears in the context of the center word.
- **Softmax Objective:** The model converts these inner products into a probability distribution using the softmax function over the entire vocabulary.
- **Gradient / Update Rule Intuition:** When deriving the gradient of the log-likelihood with respect to the center vector $v_c$, the update rule is essentially the correct outside vector ($u_o$) minus the expected value of all word vectors weighted by the model's current predicted probability ($E_{w \sim P(w|c)}[u_w]$).
- **Negative Sampling (Midterm Optimization Trick):** Calculating the softmax denominator over the entire vocabulary is computationally too expensive for millions of words. **Negative Sampling** approximates this by only sampling a few random "negative" words that _do not_ appear in the context, dramatically speeding up training.

**4. Measuring Similarity & Word Analogies**

- **Cosine Similarity:** The standard metric for semantic similarity in vector space: $\cos(u, v) = \frac{u \cdot v}{||u|| ||v||}$. Note that the basic inner product ($u \cdot v$) is exactly the same as cosine similarity if the vectors are strictly normalized to a length of 1.
- **Word Analogies (Linear Algebraic Structure):** Well-trained embeddings capture geometric relationships.
    - _Example:_ $v_{king} - v_{man} + v_{woman} \approx v_{queen}$.
- **Bias in Embeddings:** Because embeddings are trained on real-world human data (like news articles), they often encode dreadfully sexist and stereotypical societal biases.
    - _Example:_ The model might output $v_{surgeon} - v_{man} + v_{woman} \approx v_{nurse}$.

**5. Dealing with Ambiguity (Polysemy)**

- Many common words have multiple distinct meanings (e.g., "bank" as a financial institution vs. a river bank).
- Instead of creating separate vectors for every single sense of a word beforehand, research shows that different word senses naturally reside in a **linear superposition** (a weighted sum) within standard Word2Vec embeddings.
- Using sparse coding ideas, these distinct senses can actually be separated out from the single vector after training.

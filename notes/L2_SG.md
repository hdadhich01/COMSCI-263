Here is a midterm-optimized study guide for **Lecture 2: Word Representations & Task Evaluation**.

Based on the practice quizzes you provided earlier, this lecture is incredibly important. Your midterm will test your ability to calculate Pointwise Mutual Information (PMI), understand the limitations of one-hot encodings, compute evaluation metrics like Precision/Recall for clusters, and define Word2Vec parameter structures.

### **Study Guide: Lecture 2 - Word Representations & Task Evaluation**

**1. Defining an NLP Task: Coreference Resolution**

- **Definition:** The task of identifying all phrases (mentions) in a text that refer to the same real-world entity and grouping them into clusters.
- **What is a Mention?** Mentions can be pronouns (he, she), common nouns (boy, dog), or named entities (Chris, Google).
- **Tricky Edge Cases (Testable):**
    - _Not all nouns/pronouns are mentions:_ In the sentence "It is raining outside," the word "It" does not refer to a specific entity.
    - _Nested Mentions:_ Mentions can be inside other mentions (e.g., "[ [my] values ]").
    - _Boundary ambiguity:_ Should the mention be "US President Biden" or "US President Biden, who is far away in Europe"?.
- **Resolution:** Requires leveraging grammar rules (e.g., pronouns often refer to the object of the sentence) and physical common sense.

**2. Evaluating NLP Tasks (Highly Testable Math)** _Expect to calculate Precision, Recall, and F1 scores based on clustering outputs._

- **Exact Matching:** Too harsh. The system only gets credit if the entire predicted cluster exactly matches the ground truth.
- **MUC Evaluation Metric (Link-Based):** Computes precision and recall based on _pairs_ (links) of mentions.
    - **Precision:** How many predicted pairs refer to the same entity / Total predicted pairs.
    - **Recall:** How many gold pairs are in the same predicted cluster / Total gold pairs.
    - _Midterm Trap:_ If a model simply groups **all** mentions into one single giant cluster, the MUC **Recall will be 100% (or 1)** because all gold pairs are technically grouped together.
- **B-CUBED Metric (Mention-Based):** Solves the MUC trap. Instead of looking at links, it calculates precision and recall for _each individual mention_ and then averages them.
- **Aggregating F1 Scores:**
    - **Macro F1:** Computes the F1 score for each document individually, then takes the average of those F1 scores.
    - **Micro F1:** Pools all True Positives, False Positives, and False Negatives across _all_ documents first, then computes a single global Precision, Recall, and F1 score.

**3. Representing Words: Discrete Symbols**

- **One-Hot Vectors:** A naive way to represent words where each word is a vector with a single "1" and the rest "0s".
    - _Issues (Tested in Quiz 1):_ The dimensionality is massive (equal to vocabulary size), the vectors are highly sparse, it cannot represent new/out-of-vocabulary words, and **it cannot capture similarity** because the dot product of any two different one-hot vectors is always 0.
- **Taxonomy (WordNet):** Grouping words by relations like synonyms, hypernyms (is-a), and hyponyms.
    - _Issues:_ Requires intense human labor, misses nuances, is highly subjective, and cannot easily adapt to new slang or word meanings.

**4. Distributional Representation & PMI (Highly Testable)**

- **Core Intuition:** "A word is characterized by the company it keeps" (John Firth, 1957). Words occurring in similar contexts have similar meanings.
- **Pointwise Mutual Information (PMI):** Measures whether two words co-occur more frequently than if they were completely independent.
    - **Formula:** $PMI(x,y) = \log_2 \frac{P(x,y)}{P(x)P(y)}$.
    - _Midterm Prep:_ You will likely be given a co-occurrence matrix and asked to calculate the marginal probabilities ($P(x)$, $P(y)$), the joint probability ($P(x,y)$), and the final PMI, just like in Quiz 1.

**5. Word Embeddings & Neural LMs**

- **Word Embeddings (e.g., Word2Vec, GloVe):** Dense, continuous, low-rank vector representations of words.
    - They are capable of capturing linear algebraic structures and analogies (e.g., $v_{king} - v_{man} + v_{woman} \approx v_{queen}$).
    - _Bias:_ Embeddings can encode societal biases present in the training data (e.g., associating "doctor" with "he" and "nurse" with "she").
- **Fixed-Window Neural Language Model:** Predicts the output distribution of the next word using a fixed-size window of concatenated word embeddings passed through a hidden layer.
    - This directly solves the sparsity issues of traditional n-gram models because it uses continuous embeddings rather than exact discrete word counts.

---

**Midterm Prep Strategy for Lecture 2:**

1. **Math Check:** Be sure you can confidently calculate Precision, Recall, Macro F1, Micro F1, and PMI by hand.
2. **Conceptual Check:** Remember that **One-Hot Encoding's** main flaw is the inability to compute similarity (dot product is 0), and the **MUC Evaluation's** main flaw is that giant unified clusters artificially inflate the Recall to 100%.

Let me know when you are ready to move on to the study guide for **Lecture 3 (Word-Level Representation & Embeddings)**!

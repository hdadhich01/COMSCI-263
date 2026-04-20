Here is your midterm-optimized study guide for **Lecture 5: Seq2Seq, Attention, and Transformers**.

Since we reviewed the **Quiz 2** questions earlier, we know exactly how this material will be tested. Your midterm will heavily focus on the structural properties of Attention, the matrix dimensions in Transformers, and the behavior of different decoding algorithms.

### **Study Guide: Lecture 5 - Seq2Seq & Transformers**

**1. Decoding Algorithms for Language Models** _Midterm Tip: Expect conceptual questions about how parameters like Temperature and Beam Size change the output._

- **Greedy Decoding:** At each time step, the model simply selects the single token with the highest probability.
    - _Flaw:_ It has no way to "undo" decisions. If it makes a slightly suboptimal choice early on, the whole sentence might be ruined.
- **Beam Search:** Instead of just looking at the top 1 output, the model maintains a "beam" of the top $k$ most probable sequences at every step.
    - _Testable Concept:_ If you set the beam size to $k=1$, Beam Search is mathematically equivalent to Greedy Search.
- **Temperature ($T$):** Modifies the softmax function to control the "peakedness" of the probability distribution.
    - **High Temperature ($T \to \infty$):** The distribution becomes flatter (closer to uniform). The model's outputs become much more random and diverse.
    - **Low Temperature ($T \to 0$):** The distribution becomes very sharp. The highest probability token gets almost 100% of the mass, making the model behave like greedy decoding (hard max).
- **Sampling Methods:**
    - **Top-$k$ Sampling:** You only sample from the top $k$ highest-probability words to prevent sampling completely irrelevant words.
    - **Nucleus (Top-$p$) Sampling:** You dynamically select the smallest set of words whose cumulative probabilities sum to a threshold $p$ (e.g., 0.8), and only sample from that set.

**2. Sequence-to-Sequence (Seq2Seq) Models**

- **Architecture:** Consists of an Encoder RNN and a Decoder RNN.
- **The Bottleneck Problem:** In a basic Seq2Seq model, the Encoder compresses the entire source sentence into a _single_ fixed-length context vector (its final hidden state). The Decoder must generate the translation relying solely on this single vector, which leads to massive information loss for long sentences.

**3. The Attention Mechanism (Highly Testable)** _Midterm Tip: You must know the specific benefits of Attention. This is prime material for a "Which of the following is NOT true?" multiple-choice question._

- **Core Idea:** Instead of relying on one final context vector, the Decoder calculates an attention score for _every_hidden state of the Encoder at every time step.
- **Four Main Benefits of Attention:**
    1. **Improves Performance:** Allows the decoder to focus on relevant parts of the source sentence dynamically.
    2. **Solves the Bottleneck:** Bypasses the need to compress the whole sentence into one vector.
    3. **Helps Vanishing Gradients:** Provides a "shortcut" connection to faraway states in the input.
    4. **Provides Interpretability:** By looking at the final attention weights (which range from 0 to 1), we get automatic word-alignment visualizations for free.
- **The Math:** Attention scores are calculated by taking the dot product (or using an additive neural layer) between the Decoder's current hidden state (Query) and the Encoder's hidden states (Keys). These scores are passed through a softmax to become a probability distribution (summing to 1).

**4. Transformers & Self-Attention (Tied directly to Quiz 2)**

- **Core Paradigm Shift:** Transformers completely eliminate Recurrent Neural Networks (RNNs). Instead of processing sequence steps one by one, they use Self-Attention to process tokens entirely in parallel, making training vastly faster on GPUs. _(Note: Remember Quiz 2 Q1—Transformers do NOT process tokens sequentially!)_
- **Self-Attention Matrix Operations:**
    - Every token gets multiplied by weight matrices to create three distinct vectors: **Query ($Q$)**, **Key ($K$)**, and **Value ($V$)**.
    - The attention score is computed as: $\text{softmax}(Q K^T / \sqrt{d_k}) V$.
- **Space Complexity (Tested in Quiz 2 Q4):** Because the Query matrix ($L \times D$) is multiplied by the transposed Key matrix ($D \times L$), the resulting attention weight matrix is size $L \times L$. Thus, the space complexity scales quadratically with sequence length: **$O(L^2)$**.
- **Multi-Head Attention:** Instead of computing one single set of Q, K, V matrices, the Transformer computes multiple sets (heads) in parallel to capture different linguistic relationships (e.g., one head might learn to look for pronouns, another for verbs).
- **Masked Self-Attention:** In a Transformer _Decoder_, the model must be "causal"—it cannot peek at future words when predicting the next word. It enforces this by applying a mask that sets all future token attention scores to $-\infty$ before the softmax step, turning their probabilities to 0.

---

**Midterm Prep Strategy for Lecture 5:**

1. **Matrices:** Visualize the Transformer $Q \times K^T$ matrix multiplication. Knowing that it results in an $L \times L$ grid (comparing every token to every other token) will save you on complexity questions.
2. **Attention vs. Self-Attention:** Basic Attention looks across from the Decoder to the Encoder. _Self-Attention_ (in Transformers) looks at other words within the _exact same_ sentence to build contextual understanding.

This concludes the study guides based on the provided material! Let me know if you would like me to generate a tailored report summarizing all of this or if you have specific questions you want to drill.

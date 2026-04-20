We begin by understanding the ultimate goals of NLP and the fundamental difficulties computers face when interpreting human language.

- **NLP Basics & Challenges** (Lec 1)
    - Types of language ambiguity (Lec 1)
    - LLM hallucination issues (Lec 1)

To overcome these broad language challenges, we must first learn how to formally define specific NLP tasks, evaluate model performance using concrete metrics, and represent basic text using discrete symbols.

- **Task Evaluation & PMI** (Lec 2, Quiz 1)
    - Coreference resolution metrics (Lec 2)
    - Calculating PMI matrices (Lec 2, Quiz 1)
    - One-hot encoding limitations (Lec 2, Quiz 1)

Because discrete symbols like one-hot vectors fail to capture semantic similarity and suffer from sparsity, we transition to learning continuous, distributed word embeddings that map meaning into a geometric space.

- **Word Embeddings** (Lec 3, Quiz 1)
    - Word analogies and similarity (Lec 3)
    - Word2Vec skip-gram parameters (Lec 3, Quiz 1)

Once we can represent individual words effectively, we scale up to modeling entire sequences to predict the next word, evolving from count-based n-grams to neural networks to handle variable contexts.

- **Language Models** (Lec 4, Quiz 1)
    - N-grams and Laplace smoothing (Lec 4)
    - Feedforward fixed-window LMs (Lec 4, Quiz 1)
    - RNNs and vanishing gradients (Lec 4)

Since standard recurrent neural networks struggle with long-term dependencies and sequence bottlenecks, we introduce Attention mechanisms and Transformers to process complex sequences in parallel.

- **Transformers & Decoding** (Lec 5, Quiz 2)
    - Beam search vs greedy (Lec 5, Quiz 2)
    - Seq2Seq step predictions (Lec 5, Quiz 2)
    - Attention mechanism weights (Lec 5, Quiz 2)
    - Transformer self-attention space (Lec 5, Quiz 2)

Finally, armed with the theoretical knowledge of Transformer architectures, we move to practical implementation, learning how to deploy, tokenize, and train these modern models using code APIs and cloud GPU infrastructure.

- **Implementation Practice** (Lec 6)
    - HuggingFace API tokenizers (Lec 6)
    - Cloud GPU terminal commands (Lec 6)
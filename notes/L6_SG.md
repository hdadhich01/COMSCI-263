Here is your midterm-optimized study guide for **Lecture 6: Implementation Practice (Running and Training LLMs)**.

Since this lecture was heavily focused on practical implementation, your midterm will likely test your knowledge of essential Linux/GPU commands and the specific behaviors of HuggingFace API functions.

_Note: The TA announced that your midterm is closed-book, but you are allowed **one double-sided, letter-size cheat sheet**. Be sure to add these commands and API functions to it!_

### **Study Guide: Lecture 6 - Implementation Practice & HuggingFace**

**1. Cloud Computing & GCP Setup (Practical Knowledge)**

- **GPU Quota:** By default, Google Cloud Platform (GCP) provides **0 GPUs**. You must submit a request to increase the quota for a specific region and GPU type (e.g., K80 in us-west1-b), which can take 24-48 hours to approve.
- **Billing Trick:** To maximize free credits, team members can sequentially link their billing accounts to the same project. When Person A's credits drain, switch the billing account to Person B. **There is no downtime** for machines when switching billing accounts.
- **VM Configuration:** When creating an instance, selecting the **"Deep Learning on Linux"** boot disk image (e.g., Debian 10 based Deep Learning VM) is highly recommended because it comes with the CUDA driver pre-installed.
- **Cost Saving:** Always **stop/turn off** your machine when it is not in use to save your credits.

**2. Remote Server Management & Linux Commands (Highly Testable)** _Expect multiple-choice questions asking you to identify the correct command to check GPU status or run a background job._

- `nvidia-smi`: The command used to verify that the NVIDIA GPU driver is installed, check GPU memory usage, and see which jobs are currently running on the GPU.
- `export CUDA_VISIBLE_DEVICES="0"`: Specifies exactly which GPU(s) your script is allowed to use. Setting it to `""`forces the script to use the CPU.
- **tmux (Terminal Multiplexer):** Used to run jobs in the background so that your code continues to run even if your SSH connection drops.
    - `tmux new -s exp1`: Creates a new session named "exp1".
    - `control + b`, then `d`: Detaches from the current session (leaves it running in the background).
    - `tmux a -t exp1`: Attaches back to the "exp1" session.
    - `tmux ls`: Lists all active sessions.

**3. HuggingFace Transformers: `AutoModel`**

- **Concept:** HuggingFace provides APIs to easily download, use, and fine-tune pretrained models (like BERT, RoBERTa, GPT-2).
- **AutoClasses:** The `AutoModel` class automatically guesses and retrieves the correct model architecture based on the name of the pretrained weights you supply (e.g., `"bert-base-cased"`).
- **Task-Specific Variants:** You must instantiate the correct variant for your specific NLP task. Examples include:
    - `AutoModelForSequenceClassification` (for tasks like sentiment analysis)
    - `AutoModelForQuestionAnswering`
    - `AutoModelForMaskedLM`

**4. HuggingFace Transformers: `AutoTokenizer` (Crucial Exam Material)**

- **Concept:** A tokenizer prepares raw string sentences for the model by converting them into numerical "IDs". **Different models use different tokenizers** (e.g., BERT and RoBERTa tokenize differently), which can lead to different outputs.
- **Key API Differences (Testable!):** You must know the difference between simply tokenizing and encoding.
    - `tokenizer.tokenize(s)`: Breaks the string into a list of string tokens. (e.g., `['i', 'like', 'nl', '##p']`).
    - `tokenizer.convert_tokens_to_ids(tokens)`: Converts that list of string tokens into their corresponding numerical vocabulary IDs.
    - `tokenizer.encode(s)`: **Does both steps AND adds special tokens.** For example, feeding a sentence into `encode()`for BERT automatically adds the `[CLS]` token (ID 101) at the beginning and the `[SEP]` token (ID 102) at the end.
    - `tokenizer.decode(ids)`: Converts numerical IDs back into a human-readable string, keeping the special tokens intact.
- **Batch Encoding:** The tokenizer can handle multiple sentences at once and automatically manage **padding**(adding 0s so matrices match in size) and **truncation** (cutting off sentences that are too long). It also outputs the `attention_mask` (which tells the model which tokens are real words and which are just padding).

---

**Midterm Prep Strategy for Lecture 6:** Focus heavily on the **HuggingFace Tokenizer API**. If a question asks what `tokenizer.encode("I like NLP")` outputs for a BERT model, remember that it doesn't just output the IDs for those three words—it **must** include the starting `[CLS]` token ID and the ending `[SEP]` token ID.

This concludes the available lecture materials! Let me know if you would like me to review any specific concepts from the previous lectures or if you need help planning for your Final Project based on the provided project guidelines.

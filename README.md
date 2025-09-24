# SQuAD-it-2

A dataset that extends [SQuAD-IT](https://huggingface.co/datasets/crux82/squad_it) by adding **wrong answers** for each example, enabling evaluation and training of models on tasks like answer plausibility detection, multiple-choice QA, or robustness to misleading options. The dataset includes a total of 61.768 samples (54.159 for training and 7.609 for test). Each sample consists of a question, a supporting context passage, and a correct answer (all taken from the original SQuAD-IT), plus an additional wrong answer which we generated using Gemini-2.

## 🧩 Dataset Versions

We release four augmented versions of the original SQuAD-it dataset:

### 1. `squad_it_plausible`

This version contains **plausible but incorrect answers** for each `(context, question)` pair. The wrong answers are designed to appear contextually relevant but are factually incorrect, increasing the challenge for models to distinguish between correct and misleading information.

**Prompt used:**

```text
Contesto: {context}
Domanda: {question}
Risposta corretta: {correct_answer}

Fornisci una risposta plausibile ma sbagliata alla domanda sopra, basandoti sul contesto. Restituisci solo la risposta, senza spiegazioni o altro.
````

### 2. `squad_it_ooc`

This version includes **irrelevant or out-of-context answers**, designed to test a model's ability to reject clearly incorrect information that is unrelated to the context.

**Prompt used:**

```text
Contesto: {context}
Domanda: {question}
Risposta corretta: {correct_answer}

Fornisci una risposta completamente fuori contesto e sbagliata alla domanda sopra. Assicurati che non sia basata sul contesto fornito. Restituisci solo la risposta, senza spiegazioni o altro.
```

### 3. `squad_it_all-wrong_plausible-ooc`

This version contains only wrong answers: for each `(context, question)` pair, we include both a **plausible wrong answer** and an **out-of-context wrong answer**, removing the correct one. This setup allows evaluating whether models are biased toward one type of incorrect response over another.

The plausible and OOC wrong answers are taken from the datasets described above.

### 4. `squad_it_all-wrong_ooc-ooc`

This version also includes only wrong answers, but both options are **out-of-context**. One is the original irrelevant answer, while the second is generated using the following prompt to ensure it is different from the first and also fully out-of-context:

**Additional prompt used:**

```text
Contesto: {context}
Domanda: {question}
Risposta corretta: {correct_answer}
Risposta errata: {wrong_answer}

Fornisci una risposta completamente fuori contesto e sbagliata alla domanda sopra. Assicurati che non sia basata sul contesto fornito e che sia diversa dalla risposta errata già presente. Restituisci solo la risposta, senza spiegazioni o altro.
```

This dataset enables controlled experiments with **no valid answer**, useful for studying positional preferences and model uncertainty.

## 📂 Structure

Each version is split into:

* `train`
* `test`

Each example contains the following fields:

* `id`: original SQuAD-it identifier
* `context`: passage containing the answer
* `question`: question based on the context
* `correct_answer`: correct answer span from the context (only in versions 1 and 2)
* `wrong_answer`: generated incorrect answer (either plausible or irrelevant)
* `second_wrong_answer`: only present in the `all-wrong` versions (used as the alternative wrong option)

All files are in `.jsonl` format (one JSON object per line).

## 💡 Use Cases

This dataset can be used for:

* Training or evaluating **multiple-choice QA** models
* Studying **positional bias** and answer order effects
* Exploring **model robustness** to misleading or adversarial answers
* Simulating **uncertainty scenarios** with no correct options
* Creating **binary classifiers** for correct vs. incorrect answer detection
* Augmenting datasets for **contrastive learning** in QA

## 📜 License

This dataset is released under the **Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)** license. See the [LICENSE](LICENSE) file for details.

## 📬 Citation

@article{labruna2025positional,
  title={Positional Bias in Binary Question Answering: How Uncertainty Shapes Model Preferences},
  author={Labruna, Tiziano and Gallo, Simone and Da San Martino, Giovanni},
  journal={arXiv e-prints},
  pages={arXiv--2506},
  year={2025}
}

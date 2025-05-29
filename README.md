# SQuAD-it-2

A dataset that extends [SQuAD-IT](https://huggingface.co/datasets/crux82/squad_it) by adding a **wrong answer** for each example, enabling evaluation and training of models on tasks like answer plausibility detection, multiple choice QA, or robustness to misleading options.

## 🧩 Dataset Versions

We release two augmented versions of the original SQuAD-it dataset:

### 1. `squad_it_plausible_wrong`

This version contains **plausible but incorrect answers** for each `(context, question)` pair. The wrong answers are designed to appear contextually relevant but are factually incorrect, increasing the challenge for models to distinguish between correct and misleading information.

**Prompt used:**

```text
Contesto: {context}
Domanda: {question}
Risposta corretta: {correct_answer}

Fornisci una risposta plausibile ma sbagliata alla domanda sopra, basandoti sul contesto. Restituisci solo la risposta, senza spiegazioni o altro.
```

### 2. `squad_it_irrelevant_wrong`

This version includes **irrelevant or out-of-context answers**, designed to test a model's ability to reject clearly incorrect information that is unrelated to the context.

**Prompt used (for generating irrelevant answers):**

```text
Contesto: {context}
Domanda: {question}
Risposta corretta: {correct_answer}

Fornisci una risposta completamente fuori contesto e sbagliata alla domanda sopra. Assicurati che non sia basata sul contesto fornito. Restituisci solo la risposta, senza spiegazioni o altro.
```

## 📂 Structure

Each version is split into:

* `train`
* `test`

Each example contains the following fields:

* `id`: original SQuAD-it identifier
* `context`: passage containing the answer
* `question`: question based on the context
* `correct_answer`: correct answer span from the context
* `wrong_answer`: generated incorrect answer (either plausible or irrelevant)

All files are in `.jsonl` format (one JSON object per line).

## 💡 Use Cases

This dataset can be used for:

* Training or evaluating **multiple-choice QA** models
* Studying **model robustness** to misleading or adversarial answers
* Creating **binary classifiers** for correct vs. incorrect answer detection
* Augmenting datasets for **contrastive learning** in QA

## 📜 License

This dataset is released under the **Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)** license. See the [LICENSE](LICENSE) file for details.

## 📬 Citation

Ongoing..

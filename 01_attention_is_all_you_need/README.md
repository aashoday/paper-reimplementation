---
# Abstract

Before **"Attention Is All You Need"** models were based on complex **Recurrent Neural Networks** or **Convolutional Neural Networks** that included an **encoder** and a **decoder**. Some best performing models connected the encoder and decoder through an **attention** mechanism.

This paper proposes a different architecture, the **Transformer** with just **attention mechanism** dropping recurrence and convolutions.

The experiments show these models to be superior in quality, more parallelizable and requiring significantly less training time.
The models achieve **28.4 BLEU** on the **WMT 2014 English-to-German** translation task, beating the previous best results including **ensembles** by **2 BLEU**.
On the **WMT 2014 English-to-French** translation task, the models established a single-model **BLEU** score of **41.8** after training for **3.5 days** on **8 GPUs**.

The paper shows that Transformer generalizes well to other tasks by applying it to English constituency parsing both with large and limited training data.

---

# Prerequisite

## 1. [BLEU](https://en.wikipedia.org/wiki/BLEU)

**Bilingual Evaluation Understudy** is an automatic evaluation metric. It measures the quality of machine-generated translations by comparing with one or more human-written references.

BLEU measures overlap using n-gram precision and a Brevity Penalty.
It usually evaluates n-grams of order 1 to 4.

### Modified n-gram precision

Ordinary precision measures the fraction of predicted items that are correct. BLEU applies a modified precision to n-grams.

For an n-gram order \(n\), modified precision is:

$$
p_n =
\frac{
\sum_{g \in G_n} \min\big(c_{\text{gen}}(g),c_{\text{ref}}(g)\big)
}{
\sum_{g \in G_n} c_{\text{gen}}(g)
}
$$

Where:

- \(p_n\): modified precision for n-grams of order \(n\).
- \(G_n\): the set of distinct n-grams in the generated translation.
- \(c_{\text{gen}}(g)\): number of times n-gram \(g\) occurs in the generated translation.
- \(c_{\text{ref}}(g)\): maximum permitted count of \(g\) across the reference translations, using the maximum count from any single reference.
- \(\min(\cdot)\): clips the generated count so that repeated words cannot artificially inflate the score.

### Brevity Penalty (BP)

Precision alone is insufficient for translation evaluation. A system can generate short sentence containing only words that appear in the reference and still achieve high precision.

e.g. The following pair achieves precision of 1.0, even though the candidate omits most of reference's meaning.

- Reference: "The man is sitting on the chair."
- Candidate: "The man."

BLEU addresses this using the **brevity penalty (BP)**.

$$
BP =
\begin{cases}
1, & c > r \\[6pt]
e^{(1-r/c)}, & 0 < c \leq r \\[6pt]
0, & c = 0
\end{cases}
$$

Where:

- \(c\) is the total number of words in the generated translation.
- \(r\) is the effective reference length, selected from the reference translations using the standard BLEU reference-length rule.
- \(e\) is the base of the natural logarithm.

### The BLEU score formula

The standard BLEU-4 score combines modified n-gram precision for orders 1 through 4 with the brevity penalty.

$$
BLEU = BP \cdot
\exp\left(\sum_{n=1}^{N} w_n\log p_n\right)
$$

For standard BLEU-4, \(N=4\) and \(w_n=\frac{1}{4}\) for each n-gram order.

Where:

- \(w_n\) are weights assigned to each n-gram order.
- \(BP\) is the Brevity Penalty.

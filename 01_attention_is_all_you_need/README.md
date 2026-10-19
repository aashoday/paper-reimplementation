# Abstract 

Before **"Attention Is All You Need"** models were based on complex **Recurrent Neural Networks** or **Convolutional Neural Networks** that included an **encoder** and a **decoder**. Some best performing models connected the encoder and decoder through an **attention** mechanism. 

The paper proposes a different architecture, the **Transformer** with just **attention mechanisms** dropping recurrence and convolutions.

The experiments show these models to be superior in quality, more parallelizable and requiring significantly less training time.
The models achive **28.4 BLEU** on the **WMT 2014 English-to-German** translation task. Which beats the previous best results including **ensembles** by **2 BLEU**. 
On the **WMT 2014 English-to-French** translation task, the models established a single-model **BLEU** score of **41.8** after training for **3.5 days** on **8 GPUs**.

The paper shows that Transformer generalizes well to other tasks by applying it to English constituency parsing both with large and limited training data.
---
title: W2V-BERT
tags:
  - paper-review
enableToc: false
---
## W2V-BERT [Contrastive Learning + MLM]

[This paper](https://arxiv.org/pdf/2108.06209.pdf) introduces a W2V-BERT framework which combines Contrastive Learning and Masked Language Modelling for *Self-Supervised Speech Pre-Training*.

- Contrastive Learning Technique
	- used to discretise input continuous speech signal into finite set of discriminative speech tokens by learning contextual representations and latent representation using quantisation method.
- Masked Language Modelling
	- trains the model to learn contextualised speech representation via solving masked prediction task consuming the discretised tokens.
- End-to-End optimisable, ie, Loss = Contrastive Loss + MLM Loss
	- Best compared to alternatives like HuBERT and vq-wav2vec, where former relies on iterative re-clustering and re-training, and later concatenates two models
- Techniques:
	- [[Grouped Convolution.md|Grouped Convolution]]
	- [[Gumbel Softmax.md|Gumbel Softmax]] 
	- [[Conformer.md|Conformer]]
	- [[Depth-wise separable convolution| Depth-wise separable convolution]]
	- [[Quantisaton.md|Quantisation]]
## Insights:

1. Relative positional encoding using grouped convolutional layers, since absolute positional embeddings won't be appropriate since speech is sampled from waveform. And grouped convolutions allow balance between channels and dimensions.

2. Instead of modelling using convolutional alone as Wav2Vec 1.0 or transformer alone Wav2Vec 2.0, Wav2Vec-BERT uses combination of both, ie,  depth-wise separable convolution and transformer to learn local and global interactions for ASR task as mentioned in conformer paper.

3. MLM is superior to Contrastive Technique for learning contextual representations as seen in w2v-conformer vs w2v-BERT, ie, using MLM to predict token-ids created using quantisation of unmasked feature encodings increases performance. In real-life dataset where there will be more non-speech, ie, silences and less context. The conformer alone w2v-conformer won't be able to capture long-term dependencies which are meaningful than, silences which are short-term.

4. "With great power comes greater responsibility", since MLM don't want to take responsibility, it tends to cheat along with quantiser by assigning same codeword or same id for all the masked features, the contrastive module, ie, contrastive loss is necessary to make entries in the codebook to be discriminative. Again, ie, using entropy to prevent codebook collapse.

5. In quantisation using Gumbel Max Trick, since samples from the discrete probability distribution can't be differentiable to allow back-propagation through the stochastic model, to automatically learn discrete units of speech made of codeword sampled from each of the code-groups.

6. Since Masked prediction and Contrastive modules can be trained end-to-end, the issues with token-id assignment in vq-Wav2Vec or DiscreteBERT is solved, along with issues with iterative approach for token-id assignment where you need to choose hyper-parameters again and again depending on task.

## Generalisability:

* Due to the use of codewords and codebooks, the above approach can be used in limited areas where we have unlabelled sequential data (continuous or discrete), where each segment of the data can take discrete values.

* I can see the main relevance of learning representation from data from smart wearable watches or DNA sequences related to Healthcare, Climate reading like temperature related to Weather, and Notes related to Music. 

## Limitations:

* Novel or Out-of-distribution: The codewords and code-groups are being created manually the self-supervised technique won't be able to learn novel or out-of-distribution speech related representations.

* Long sequence lengths: The self-attention has quadratic computational complexity, and can be trained for limited context length only.

* Transformers -> Inherently Discrete : The speech input signal is continuous, but transformers consider speech is discrete. Using Neural ODE's to interpolate and extrapolate, handle irregularity better, but difficult to train many times.

## Extended Research Direction:

* Stochastic Depth: During training dropout of layer randomly to avoid getting stuck at local-minima, ie, finding global minima.

* Preserving Speaker Identity: We can use techniques like LoRA, hyper-networks (different from above), and other approaches used in recent SoTA Diffusion and LLMs to enable the model to learn speaker identity representation weights in those additional weights.

* LLM Techniques: Usage of latest LLM techniques related to Quantisation, Latency, and Context Length can be utilized. Eg: GPTq, Medusa heads, RoPE etc.

* Fairness Constraint: There is a scope to decrease bias/ increase fairness in learned representations, ie, by decreasing joint probability of codewords by using decoder along with a critic which will give reward to use techniques like PPO similar to RLHF since gradients can be back-propagated using Gumbel max trick.

* Hyper-Network: We can use hyper-network to generate weights of some layers, ie, based on the layer-number of conformer or the input data-signal or both, allowing us to create much smaller model with better adaptability**
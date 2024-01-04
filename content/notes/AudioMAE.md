---
title: AudioMAE
tags:
  - sapling
enableToc: false
---
## AudioMAE

[This paper](https://arxiv.org/abs/2207.06405) introduces a AudioMAE, ie, simple extension of image-based Masked Autoencoders  (MAE) to self-supervised representation learning from audio spectrograms. It sets new state-of-the-art among models with In-domain self-supervised pre-training, ie, not including out-of-domain supervised pre-training.

### Model Architecture and Implementation
- **Preprocessing**
	- audio recording -> Mel spectrograms -> non-overlapped regular grid patches.
- **Embedding**
	- Linear projection: patches -> flattened and embedded
	- add sinusoidal positional embeddings
	- convolutional kernels with (16, 16) size and stride in time and frequency.
- **Masking**
	- unstructured high-ration (80%) masked during self-supervised training
	- structured (time+frequency) low ratio during supervised fine-tuning
- **Encoder**
	- 12-layer ViT-Base, standard transformer, with 20% non-masked patches to reduce computation overhead n^2.
- **Decoder**
	- encoded patches padded with trainable masked tokens 
	- restore the order manually
	- add decoder's (fixed sinusoidal) positional embeddings
	- standard transformer blocks again, 16-layer with shifted local attention, not global nor hybrid.
	- linear head to predict and reconstruct input spectrogram
- **Local attention mechanism**
	- In decoder along with global self-attention, local attention mechanism which groups and separates the spectrogram patches in to local windows in self-attention for decoding.
	- Two types are studied
		- **Shifted window location** inspired by Swin Transformers
			- Shift window attention by 50% between consecutive Transformer decoder layers
			- To pad the margin when shifting, cyclically shift the spectrogram to top-left direction
		- Hybrid window attention (global + local attention)
			- computes local attention within a window in all but the last few top layers
- **Loss**
	- MSE between the prediction and the input spectrogram, averaged over unknown patches.
- **Finetuning**
	- AudioMAE encoder alone is used
	- Optional Masking: can remove portion of patches to further regularise learning from a limited view of spectrogram inputs, as a side effect , also reduces computation during fine-tuning.
	- Average pooling layer followed by linear layer on top for fine-tuning in classification tasks.
- **Techniques**:
	- [[Audio Spectrogram Transformer.md|Audio Spectrogram Transformer (AST)]]
	- [[Swin Transformer.md|Swin Transformer]]
	- [[Hybrid Attention.md|Hybrid Attention]]
- **Additional Details**
	- we transform raw waveform (pre-processed as mono channel under 16,000 sampling rate) into 128 Kaldi compatible Mel-frequency bands with a 25ms Hanning window that shifts every 10 ms. For a 10-second recording in AudioSet, the resulting spectrogram is of 1×1024×128 dimension.
	- During fine-tuning, we employ a lower masking ratio (0.3 in time and 0.3 in frequency)
### AudioMAE vs AST
1. Positional Embeddings: AST uses learned whereas AudioMAE uses sinusoidal.
2. Encoder Architecture: AST uses standard transformer encoder, whereas AudioMAE paper mentions using ViT-B which is same as standard transformer encoder.
3. AST is encoder only architecture for supervised learning, whereas AudioMAE is encoder-decoder for SSL.
## Generalisability:

* TBA
## Limitations:

* TBA

## Extended Research Direction:

* TBA
* In paper `multimodal self-supervised learning with a joint audio-visual MAE approach as these domains share natural correspondences in video data.`
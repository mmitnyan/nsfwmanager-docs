# ONNX Models
## The AI Technology That Powers Visual Content Detection

NSFW Manager uses ONNX models to analyze images and video frames. This page explains what ONNX is, where it came from, how neural networks work, how detection models are trained, and why this qualifies as artificial intelligence — not just image processing.

---

## What Is ONNX?

**ONNX** stands for **Open Neural Network Exchange**. It is an open standard file format for representing machine learning models, allowing a model trained in one framework (such as PyTorch or TensorFlow) to be executed in a completely different runtime environment. [[1]](#ref-1)

Think of ONNX the way you think of PDF: a PDF document can be created in Word, LibreOffice, or InDesign, and opened in any PDF reader regardless of which tool created it. ONNX does the same for AI models — it defines a neutral, portable container that any compatible runtime can load and execute.

An `.onnx` file contains:
- The **graph structure** of the neural network (which layers exist and how they connect)
- The **trained weights** — billions of numerical values learned during training
- The **input and output specifications** (what shape of data goes in, what comes out)

---

## A Brief History

### 2017 — The Problem of Framework Fragmentation

By 2017, machine learning research had fragmented across incompatible frameworks. A model trained in PyTorch could not run in TensorFlow, and vice versa. Deploying a research model to a mobile device or embedded system often required rewriting it from scratch.

**September 2017:** Microsoft and Facebook (Meta) jointly announced ONNX at the Neural Information Processing Systems (NeurIPS) conference. [[2]](#ref-2) The goal was a common intermediate representation that could bridge frameworks.

The founding partners made the format open source immediately, with the explicit goal of industry-wide adoption.

### 2018–2019 — Industry Adoption

Within a year of the announcement, major technology companies joined the ONNX ecosystem: [[3]](#ref-3)

| Company | Contribution |
|---------|-------------|
| Microsoft | ONNX Runtime (high-performance inference engine) |
| Facebook/Meta | PyTorch ONNX export |
| Google | TensorFlow ONNX converter |
| NVIDIA | TensorRT ONNX support |
| Intel | OpenVINO ONNX support |
| Qualcomm | Mobile ONNX acceleration |
| ARM | Edge device ONNX runtime |

In 2019, Microsoft open-sourced **ONNX Runtime** — the execution engine that actually runs `.onnx` models. [[4]](#ref-4) It is written in C++ and designed to be fast on CPUs, GPUs, and specialized AI hardware.

### 2020 — ONNX Goes into Windows

Microsoft integrated ONNX Runtime directly into Windows 10. [[5]](#ref-5) The Windows Machine Learning (Windows ML) API allows any Windows application to run ONNX models using **DirectML** [[6]](#ref-6) — Microsoft's GPU-accelerated machine learning backend that works across AMD, NVIDIA, and Intel graphics cards without requiring vendor-specific drivers or SDKs.

This is why NSFW Manager can use GPU acceleration on any modern Windows PC, regardless of GPU manufacturer: it runs through DirectML, which is part of the operating system.

### Today

ONNX is now the de facto standard for deploying machine learning models in production. The ONNX Runtime is used inside Microsoft 365, Xbox, Windows Search, Bing, and thousands of third-party applications. [[7]](#ref-7) The `.onnx` format is supported by every major ML framework as a first-class export target.

---

## What Is a Neural Network?

A neural network is a computational structure loosely inspired by the brain. It consists of layers of mathematical functions called **neurons**, connected by **weighted edges**. Data flows through these layers, being progressively transformed at each step.

### Layers

A typical image classification network contains:

1. **Input layer** — receives raw pixel data (for example, a 224×224 RGB image = 150,528 numbers)
2. **Hidden layers** — perform learned transformations; most of the "intelligence" lives here
3. **Output layer** — produces the final result (a score or a set of scores)

The "deep" in **deep learning** refers to networks with many hidden layers — modern networks used for image recognition typically have dozens or hundreds.

### Convolutional Neural Networks (CNNs)

For images specifically, the dominant architecture is the **Convolutional Neural Network (CNN)**. [[8]](#ref-8) Instead of treating an image as a flat list of pixels, a CNN applies small mathematical filters that slide across the image, detecting local patterns:

- **Early layers** detect low-level features: edges, corners, color gradients
- **Middle layers** detect mid-level features: textures, shapes, body parts
- **Deep layers** detect high-level concepts: faces, objects, scene categories

This hierarchical approach is why CNNs work so well for visual content — they build complex understanding from simple components, the same way the human visual cortex does.

### Weights

Every connection between neurons has a **weight** — a floating-point number. A network with hundreds of layers can have hundreds of millions or billions of weights. These weights encode everything the model has learned. When you see a model described as "150 million parameters," those parameters are the weights.

---

## How Models Are Trained

Training a model is the process of finding the weight values that make the network perform the desired task well.

### Step 1 — Dataset

Training requires a **labeled dataset**: a large collection of examples where the correct answer is already known.

For NSFW detection, a training dataset might contain millions of images, each labeled by human annotators as "safe," "suggestive," or "explicit." The quality of the labels directly determines the quality of the model — garbage labels produce garbage predictions.

Curating a reliable dataset for NSFW detection is one of the hardest parts of building these models. Human annotators must review enormous quantities of explicit material under consistent guidelines, and edge cases (artistic nudity, medical imagery, different cultural standards) require careful policy decisions.

### Step 2 — Forward Pass

During training, a batch of images is fed through the network in its current state. Each image produces a score. These scores are compared against the correct labels using a **loss function** — a mathematical measure of how wrong the predictions are.

If the model predicts "safe" for an explicit image, the loss is high. If it predicts "explicit" correctly with high confidence, the loss is near zero.

### Step 3 — Backpropagation

**Backpropagation** is the algorithm that adjusts the weights to reduce the loss. [[9]](#ref-9) It computes the gradient of the loss with respect to every weight in the network — essentially measuring which direction to move each weight to make the predictions slightly more accurate.

This is an application of calculus (specifically the chain rule for derivatives) applied across millions of parameters simultaneously. Modern training uses optimizers like Adam or SGD to apply these gradients efficiently.

### Step 4 — Iteration

This forward-backward cycle repeats billions of times across the full dataset. With each pass, the weights shift slightly toward values that produce better predictions. After enough training, the network generalizes — it can correctly classify images it has never seen before, because it has learned the underlying visual patterns, not just memorized specific examples.

Training a large image classification model from scratch on a high-quality dataset can take days or weeks on hundreds of GPUs. The resulting weights are then frozen and exported to ONNX format for deployment.

---

## Why Is This Considered AI?

The term "AI" is broad, but ONNX models used for image classification meet any reasonable definition:

**They learn from data.** The behavior of the model is not programmed by a human writing rules. No engineer sat down and wrote "if there are pink pixels in the center of the image, flag it." The model derives its own internal representations of what constitutes explicit content from examples.

**They generalize.** A trained model correctly classifies images it has never seen — including images taken after training ended, in contexts that did not exist in the training data. This generalization is the core capability that distinguishes machine learning from lookup tables or rule engines.

**They handle ambiguity.** They output a probability (0.0–1.0) rather than a binary yes/no, because the underlying reality is probabilistic. A photograph that one human annotator labels "suggestive" and another labels "explicit" will typically receive a middle score — the model reflects genuine uncertainty in the same way the annotators did.

**They operate on raw sensory data.** They take pixels as input, with no hand-crafted feature extraction in between. The model discovers what features matter — they are not specified by a programmer.

The ONNX Runtime executing the model is deterministic software. But the model it executes is an artifact of a learning process, not a program someone wrote. That distinction is what makes it AI.

---

## The Three Precision Variants in NSFW Manager

NSFW Manager ships three variants of its detection model:

### Full Precision (fp32 — "The Nit Picker")

The `.onnx` model runs with 32-bit floating-point weights. This is the format produced directly after training — every weight is stored at full precision. It is the most accurate variant and the slowest, because each weight occupies 4 bytes and the arithmetic is heavier.

### Half Precision (fp16 — "The Laid-Back One")

Weights are stored and computed as 16-bit floats. Memory usage is halved, and on modern GPUs that natively support fp16 arithmetic (which includes most GPUs released since 2017), inference is roughly twice as fast with minimal accuracy loss.

fp16 inference requires a GPU that supports DirectML or CUDA. It cannot run on CPU because most CPUs perform fp16 arithmetic by converting to fp32 internally, which erases the speed advantage.

### Integer Quantization (int8 — "The Just Perfect")

Weights are quantized to 8-bit integers. [[10]](#ref-10) Memory footprint is reduced to one quarter of fp32. Inference is fast on both CPU and GPU because integer arithmetic is cheaper than floating-point. The slight precision loss from quantization is nearly imperceptible for binary classification tasks — the model still correctly identifies clearly explicit content.

This is the default engine because it runs well on any hardware, including older CPUs without GPU acceleration.

| Variant | Weight precision | Memory | GPU required | Accuracy |
|---------|-----------------|--------|--------------|----------|
| onnx (fp32) | 32-bit float | High | No | Highest |
| fp16 | 16-bit float | Medium | Yes | High |
| int8 | 8-bit integer | Low | No | Good |

---

## Inference: Running a Model

When NSFW Manager scans an image, the following happens:

1. **Pre-processing** — the image is resized to the model's expected input dimensions (typically 224×224 or 320×320 pixels) and normalized (pixel values scaled from 0–255 to 0.0–1.0)
2. **Inference** — the pre-processed tensor is passed through the ONNX Runtime, which executes the network layer by layer
3. **Post-processing** — the output tensor (a vector of raw scores called logits) is converted to a probability via a softmax or sigmoid function
4. **Thresholding** — the probability is compared against your configured threshold to determine whether the file is flagged

This entire process typically takes 5–50 milliseconds per image depending on hardware and the model variant selected.

---

## Local Inference: Why No Cloud Is Involved

ONNX Runtime runs entirely on your machine. The model weights are stored locally in your user profile after download. When a scan runs:

- No image data leaves your computer
- No API call is made to an external server
- No account or internet connection is required for scanning

This is a deliberate architectural decision. The alternative — sending images to a cloud API — would require transmitting potentially sensitive content to a third-party server. Local ONNX inference eliminates that risk entirely.

---

## Model Accuracy and Its Limits

No classification model is 100% accurate. Understanding why helps set realistic expectations:

**The training data is imperfect.** Human annotators disagree on borderline content. A model trained on those labels inherits that ambiguity.

**The model has never seen everything.** Unusual lighting conditions, heavy compression, artistic styles, or content categories that were underrepresented in training data will produce less reliable scores.

**Probability is not certainty.** A score of 0.92 means the model is confident — not certain. Occasionally, a clearly safe image will receive a high score (false positive) or a clearly explicit image will receive a low score (false negative).

This is why NSFW Manager exposes the threshold as a user-configurable setting and shows the exact score for every result. You are not forced to act on the model's output — you can review borderline cases manually and adjust the threshold to match your specific use case.

---

## Related Pages

- [Detection Engines](./engine.md) — choosing between int8, fp16, and onnx in NSFW Manager
- [Detection Threshold](../features/detection-threshold.md) — configuring sensitivity
- [False Positives](../troubleshooting/false-positives.md) — handling incorrect detections
- [Performance](../troubleshooting/performance.md) — GPU acceleration and scan speed

---

## References

<a id="ref-1"></a>**[1]** ONNX — Open Neural Network Exchange. Official specification and format documentation.  
https://onnx.ai/onnx/intro/concepts.html

<a id="ref-2"></a>**[2]** Microsoft AI Blog — "Facebook and Microsoft introduce new open ecosystem for interchangeable AI frameworks" (September 2017).  
https://blogs.microsoft.com/ai/facebook-and-microsoft-introduce-new-open-ecosystem-for-interchangeable-ai-frameworks/

<a id="ref-3"></a>**[3]** ONNX Community — Partners and supported frameworks.  
https://onnx.ai/supported-tools.html

<a id="ref-4"></a>**[4]** Microsoft — ONNX Runtime open source repository (MIT License).  
https://github.com/microsoft/onnxruntime

<a id="ref-5"></a>**[5]** Microsoft Docs — Windows Machine Learning overview.  
https://learn.microsoft.com/en-us/windows/ai/windows-ml/

<a id="ref-6"></a>**[6]** Microsoft Docs — DirectML overview.  
https://learn.microsoft.com/en-us/windows/ai/directml/dml

<a id="ref-7"></a>**[7]** ONNX Runtime — Production usage and performance benchmarks.  
https://onnxruntime.ai/

<a id="ref-8"></a>**[8]** LeCun, Y., Bottou, L., Bengio, Y., & Haffner, P. (1998). "Gradient-based learning applied to document recognition." *Proceedings of the IEEE*, 86(11), 2278–2324.  
https://ieeexplore.ieee.org/document/726791

<a id="ref-9"></a>**[9]** Rumelhart, D. E., Hinton, G. E., & Williams, R. J. (1986). "Learning representations by back-propagating errors." *Nature*, 323, 533–536.  
https://www.nature.com/articles/323533a0

<a id="ref-10"></a>**[10]** Gholami, A., Kim, S., Dong, Z., et al. (2021). "A Survey of Quantization Methods for Efficient Neural Network Inference." *arXiv:2103.13630*.  
https://arxiv.org/abs/2103.13630

# PMLDL. X-Net

Anna Boronina

---

# What is a brain stroke (ru: инсульт)?

**Brain stroke** occurs when the blood supply to part(s) of one's brain is interrupted or reduced.

<div>
    <img src="/drawings/brain_stroke.png" />
</div>

<style>
    div {
        display: flex;
        justify-content: center;
        align-items: center;
    }
    img {
        width: 80%;
    }
    h1 + p {
        opacity: 1;
    }
</style>

---

# Problems

- sometimes even doctors cannot spot a brain stroke
- sometimes there is a sequence of MRI scans which should be processed together
- **healthy** VS **stroke**:


<div class="column">
    <div class="row">
        <img src="/drawings/normal/49 (13).jpg"/>
        <img src="/drawings/normal/49 (14).jpg"/>
        <img src="/drawings/normal/49 (15).jpg"/>
        <img src="/drawings/normal/49 (16).jpg"/>
        <img src="/drawings/normal/49 (17).jpg"/>
        <img src="/drawings/normal/49 (18).jpg"/>
        <img src="/drawings/normal/49 (19).jpg"/>
    </div>
    <div class="row">
        <img src="/drawings/stroke/58 (1).jpg"/>
        <img src="/drawings/stroke/58 (2).jpg"/>
        <img src="/drawings/stroke/58 (9).jpg"/>
        <img src="/drawings/stroke/58 (11).jpg"/>
        <img src="/drawings/stroke/58 (12).jpg"/>
        <img src="/drawings/stroke/58 (13).jpg"/>
        <img src="/drawings/stroke/58 (15).jpg"/>
    </div>
</div>

<style>
    .column {
        margin-top: 2em;
        display: flex;
        justify-content: center;
        flex-flow: column;
        align-items: center;
    }
    .row {
        margin-bottom: 3.5em;
        margin-left: 0.5em;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    img {
        width: 13%;
        margin: -0.5em;
        margin-right: 1em;
        border-radius: 5px;
    }
</style>

---

# DL applications

- use Deep Learning model as a complementary tool
- use methods that explain model's decisions
    - GradCam
    - GradCam++
    - LIME
    - ...

<img src="/gradcam_example.png">

---

# Benchmark - ATLAS dataset

**Recall**: How many relevant items are retrieved?

<div class="row">
    <img src="/benchmark/chart_recall.png" class="paper" />
    <img src="/benchmark/qr.png" class="paper_qr" />
</div>

<style>
    .row {
        margin-top: 2em;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .paper {
        width: 75%;
        margin-right: 1em;
    }
    .paper_qr {
        width: 15%;
        border-radius: 5px;
        margin-top: -3em;
    }
</style>

---

# Benchmark - ATLAS dataset

**Precision**: How many retrieved items are relevant?

<div class="row">
    <img src="/benchmark/chart_precision.png" class="paper" />
    <img src="/benchmark/qr.png" class="paper_qr" />
</div>

<style>
    .row {
        margin-top: 2em;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .paper {
        width: 75%;
        margin-right: 1em;
    }

    .paper_qr {
        width: 15%;
        border-radius: 5px;
        margin-top: -3em;
    }
</style>

---

# Benchmark - ATLAS dataset

**IoU**: What is the % of overlap between ground-truth segment and predicted segment?

<div class="row">
    <img src="/benchmark/chart_iou.png" class="paper" />
    <img src="/benchmark/qr.png" class="paper_qr" />
</div>

<style>
    .row {
        margin-top: 2em;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .paper {
        width: 75%;
        margin-right: 1em;
    }
    .paper_qr {
        width: 15%;
        border-radius: 5px;
        margin-top: -3em;
    }
</style>

---

# X-Net Paper: Qi *et. al*

<div class="row">
    <img src="/xnet_paper/title.png" class="paper" />
    <img src="/xnet_paper/qr.png" class="paper_qr" />
</div>

<style>
    .row {
        margin-top: 3em;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .paper {
        width: 80%;
        border-radius: 5px;
        margin-right: 1em;
    }
    .paper_qr {
        width: 15%;
        border-radius: 5px;
        margin-top: 1em;
    }
    h1 + p {
        opacity: 1;
    }
</style>

---

# Previous attempts

CHECK THIS INFO
- SegNet - symmetrical convolutional autoencoder
- UNet - residual convolutional network
- 2D Dense Unet - residual dense network

## Their problems according to Qi *et. al*

- heavy network parameters
<!-- - they do not consider contextual information
    - solution couldn be: LSTM -->
- cannot capture long-range dependencies

---

# Solution proposed by Qi *et. al*

- reduce number of parameters
    - separable convolution
        - it is efficient
        - gives better segmentation results 
    - basic upsampling (NN or bilinear) instead of unpooling
- Feature Similarity Module (FSM) to capture long-range dependencies
- skip-and-concat to solve vanishing gradient problem

---

# Architecture. Encoder

<img src="/arch/encoder.png"/>

<style>
    img {
        width: 90%;
        margin-left: 2.5em;
    }
</style>

---

# Architecture. Encoder. FSM

<img src="/arch/fsm.png"/>

<style>
    img {
        margin-top: 2.5em;
        margin-left: 0.5em;
    }
</style>

---


# Architecture. Encoder. FSM

1. FSM is a non-local operation for capturing long-range dependencies by learning a relationship between any two positions of a feature map
2. FSM has a reduced number of training parameters (thanks to separable convolution)
3. Can be plugged into any model

<img src="/arch/fsm.png"/>

<style>
    img {
        margin-top: 1.5em;
        margin-left: 0.5em;
        width: 70%
    }
    h1 + p {
        opacity: 1;
    }
</style>

---

# Architecture. Encoder. FSM. Separable Convolution

Separating a 3x3 kernel spatially

<img src="/arch/separable_convolution.png"/>

<style>
    img {
        margin-top: 5em;
        margin-left: 4em;
        width: 80%
    }
    h1 + p {
        opacity: 1;
    }
</style>

---

# Architecture. Encoder. FSM. Separable Convolution

Separable Convolution can enhance efficiency without significantly reducing effectiveness

<img src="/arch/apply_sconv.png">

<style>
    img {
        margin-top: 2em;
        margin-left: 5.5em;
        width: 75%;
    }
    h1 + p {
        opacity: 1;
    }
</style>

---

# Architecture. Decoder

<img src="/arch/decoder.png"/>

<style>
    img {
        width: 90%;
        margin-left: 2.5em;
    }
</style>

---

# Architecture. Decoder. XBlock

<img src="/arch/xblock.png"/>

<style>
    img {
        margin-top: 2.5em;

    }
</style>

---

# Architecture. Skip-and-concate

<img src="/arch/architecture.png"/>

<style>
    img {
        width: 90%;
        margin-left: 2.5em;
    }
</style>

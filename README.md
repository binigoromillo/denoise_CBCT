# denoise_CBCT

### SUMMARY

Breast CT (bCT) systems, based on cone-beam CT (CBCT) geometries with high-resolution flat-panel detectors, have been developed over the last ~15 years. Clinical bCT studies showed that the true volumetric information in bCT provided excellent visualization of soft tissue features and improved capability for the detection and characterization of cancerous lesions, compared to 2D mammography or digital breast tomosynthesis, at comparable radiation doses. However, at equidose conditions, bCT results in increased image noise from lower exposure in individual projection views and from amplification of high frequencies caused by the filtering stage in filtered backprojection reconstruction algorithms, commonly used for bCT image reconstruction. Recent developments in deep convolutional neural networks (CNNs) for data-driven image restoration, have shown great potential for denoising in photographic and x-ray CT and CBCT imaging.

Deep learning (DL) denoising networks have been conventionally trained using supervised approaches that minimize the mean squared error (MSE) loss, computed between the CNN inference, with a normal (or low) dose (ND) image as input, and a matched high-dose (HD) target, within the Noise2Clean paradigm. However, such matched datasets are rarely available. The dual noise level requirement was relaxed with the introduction of the Noise2Noise strategy, which proved that minimization of MSE across pairs of noisy
images with zero-mean independent and identically distributed (i.i.d.) noise realizations, and matched image content, converged to an equivalent solution. While more attainable, the acquisition of two matched scans of the same patient is seldom feasible in clinical bCT. Self-supervised training approaches aim at alleviating the need for paired noise realizations by building loss functions employing targets derived from the noisy input that act as surrogates of the clean signal. Self-supervised strategies demonstrated denoising performance comparable to Noise2Clean in photographic imaging, but their performance is impacted by the correlated nature of noise in CT (and bCT), arising from the correlation between the noise in neighboring detector pixels and from correlations introduced by the backprojection operator. 

The previous work of this project has focused on characterizing the impact of bCT noise correlation on self-supervised DL denoising leveraging high-fidelity models of the bCT imaging chain. This work was published in SPIE (Society of Photographic Instrumentation Engineers). The paper can be found here: https://drive.google.com/file/d/1P02st7aWHB-bjU8yjKq541RabqAiJVHu/view?usp=sharing

As an alternative self-supervised denoising algorithm, we implement here some approaches that have been proven effective in denoising **projection data** for low-dose cone beam CT of the head and neck. (https://drive.google.com/file/d/1QdhSBntcSWq73WjSTUG_4njJU7wnKmYL/view?usp=sharing)

In this case, we try to avoid the noise introduced during the backprojection by utilizing the projection data instead of the image data. Our methodology adopts the following strategies: 
  - A projection-wise noise-to-noise training strategy, named 2Proj2Proj. This method aims to learn the true projection from adjacent noisy projections. 

<img width="761" alt="image" src="https://github.com/binigoromillo/denoise_CBCT/assets/123977045/ae0b3c7f-2cdf-4bff-ab46-71751cc8a031">

  - A blind network, termed 3Proj2Self, which is designed to learn correlated information across detector bins within the same projection as well as adjacent projections. 
To rectify model outputs and augment prediction precision, we integrate the outputs of both 2Proj2Proj and 3Proj2Self models during the testing phase. By harmonizing these strategies, we aim to effectively reduce noise and improve image quality in bCT imaging.

<img width="872" alt="image" src="https://github.com/binigoromillo/denoise_CBCT/assets/123977045/32e57aa4-8d09-4d19-a8d9-0197568f129e">

Regarding the model's architecture, we evaluated a **dense residual network** and a **UNet**.

<img width="352" alt="image" src="https://github.com/binigoromillo/denoise_CBCT/assets/123977045/4a34cce8-a17a-413e-949a-ed7a7146a4a5">
<img width="403" alt="image" src="https://github.com/binigoromillo/denoise_CBCT/assets/123977045/df41ee06-62a0-4d2e-a5c4-1e110ac9e542">

You can find all the details of the implementation here: https://docs.google.com/document/d/1r0DfqJ7RGVjGmNTAETixCSb8kjVE5ahLcsLHb0B5-qY/edit?usp=sharing




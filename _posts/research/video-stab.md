---
title: Digital Video Stabilization
layout: post
date: 2020-06-04
permalink: /research/videoStab
comments: true
mathjax: true
---

Mechanical stabilization is well known. There external equipment such as camera gimblas and other apparatus are used to filter out vibrations. However, the capability of such equipment is limited to prventing high frequency vibrations from entering camera footage.
To tackle low frequency vibrations arising from actions such as walking and or running while recording video footage.

Stages of digital Video stabilization-
1. Motions estimation: Transformation parameters between 2 consecutive frames are derived
2. Motion smoothing: Filtering out unwanted motion - Part where we generate the filter
3. Image composition: Reconstruction of a stabilized video

### Video Stabilisation using feature points
- Track a few feature points between consecutive frames
- Tracked features allow us to estimate the motion between subsequrnt frames and subsequently correct for it

#### Paramteric Linear Motion Models
What should be the transform that facillitates the application of a motion model
Heirarchy of transforms from computer vision
- Homography
- Affine
- Euclidean (length preserving)
- Translation
For the purpose of video stabilization, a 

#### L1 optimal camera paths paper implementation details
- Paper focusses exclusively on 2D motion models (which is fair I guess)
- Can compare sparse optical flow (good when sharp features are avaiable) and dense optical flow (good when smooth features are present), both are implemented in openCV [here](https://docs.opencv.org/3.3.1/d7/d8b/tutorial_py_lucas_kanade.html).
- In the paper they have gone for the sparse optical flow method 


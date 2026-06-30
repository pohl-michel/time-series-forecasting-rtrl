## Forecasting 3D positions of internal points in the chest using an RNN trained with RTRL

**Note**: This repository contains the original code supporting my paper on RTRL published in 2021 (see ["References"](#references) section). A newer consolidated implementation is available in the [`Time_series_forecasting folder`](https://github.com/pohl-michel/2D-MR-image-prediction/blob/main/Time_series_forecasting/README.md) of the `2D-MR-image-prediction` repository, which includes code for a simpler version of RTRL, additional online learning algorithms for RNNs, and transformer models, as well as extended capabilities such as hyperparameter tuning and more general evaluation settings.

## Overview

This repository contains MATLAB code for forecasting the 3D positions of internal points in chest CT/CBCT image sequences using a vanilla RNN trained using real-time recurrent learning (RTRL). Gradient clipping is applied to ensure numerical stability.

The code in this repository predicts multidimensional time-series data using a recurrent neural network (RNN) trained by real-time recurrent learning (RTRL) with gradient clipping.

The figure below gives an example of prediction 400ms in advance with RTRL (the sampling rate is 2.5Hz):

![alt text](prediction_RTRL.png "prediction with RTRL a horizon of 400ms")

Our implementation is based on the chapter 15 ("Dynamically Driven Recurrent Networks") of the following book :
Haykin, Simon S. "Neural networks and learning machines/Simon Haykin." (2009).

Note : prediction is made without any specific assumption on the nature of the temporal data,
but the evaluation of the RNN uses the assumption that the data represents the 3D position of several objects.

## How to run

The main script to execute is "prediction_main.m".
The data to be predicted is in the directory named "1. Input time series sequences", 
and represents the 3D motion of 3 implanted metal markers in the chest during lung cancer radiotherapy.
The markers move because of the respiratory motion and their position is sampled at approximately 2.5Hz.

The parameters concerning prediction and display should be set manually in the files "pred_par.xlsx" and "disp_par.xlsx", and the "load_pred_par.m" function.
For instance, one can choose to use training with gradient clipping or not by setting manually the field `update_meth` of the `pred_par` structure (line 28). 
The behavior of the program is controlled by the structure `beh_par` defined in "load_behavior_parameters.m".
In particular, one can choose to perform computations on the GPU by setting `beh_par.GPU_COMPUTING = true`.
The fields of that structure can be set manually.

## Expected output

The figures showing prediction on the test data and the loss function are saved in the following folders: 
"1. Prediction results (figures)" and "2. Prediction results (images)". 
The RNN results are saved in the folder "3. RNN variables (temp)".
The parameters used and the numerical RNN evaluation results are also recorded in a txt file located in the folder "4. Log txt files".

## References

This repository supports the findings in the following article:

Michel Pohl, Mitsuru Uesaka, Kazuyuki Demachi, Ritu Bhusal Chhatkuli, "Prediction of the motion of chest internal points using a recurrent neural network trained with real-time recurrent learning for latency compensation in lung cancer radiotherapy", Computerized Medical Imaging and Graphics, Volume 91, 2021, 101941, ISSN 0895-6111 [[Published version](https://doi.org/10.1016/j.compmedimag.2021.101941)] [[arXiv](https://doi.org/10.48550/arXiv.2207.05951)]

Two other repositories contain code components supporting the article above:
 - Volumetric image registration using the pyramidal, iterative Lucas–Kanade optical-flow algorithm: https://github.com/pohl-michel/3D-Lucas-Kanade-optical-flow
 - 3D image warping with Nadaraya–Watson kernel regression: https://github.com/pohl-michel/Nadaraya-Watson-3D-image-warping

Please kindly consider citing our article if you use this code in your research.

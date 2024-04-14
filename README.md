# Eye State Classifiction. 

Dataset description: 
- All data is from one continuous EEG measurement with the Emotiv EEG Neuroheadset.
- The eye state was detected via a camera during the EEG measurement and added later manually to the file after analysing the video frames.

Link to dataset: https://www.kaggle.com/datasets/robikscube/eye-state-classification-eeg-dataset

## Results 
After training an RNN model on the 8,5k examples sample, the final loss on the test set (2k examples sample) is 0.0419 (MSE), accuracy 0.9772, and R^2 is 0.8294. 

The data preparation, preprocessing, and model details are provided below.

### Data Preprocessing Part.
To maintain simplicity and brevity, this text will focus on transformations based on the F3 electrode. This electrode is commonly used in the study of blinking and other artifacts in electroencephalography signals.

The dataset contained the aforementioned initial data. Outliers are immediately noticeable, and there is a significant range of data amplitude. 

![initial_data](https://github.com/diko0071/eeg_eye_state_classification/assets/82669452/98088e2e-27be-431d-8339-db3eb59bac8d)

Data distribution.

![distribution](https://github.com/diko0071/eeg_eye_state_classification/assets/82669452/60ea58fa-8075-4a8b-9fea-bddad4c942c5)

#### Appying Z-transform & IQR. 
Z-transform and IQR were applied to eliminate scaling problems of signals from different electrodes, reduce data range, and remove outliers to make the data distribution more «normal»

Data distribution after applying Z-transform and IQR. 

![removed_outliers_distr](https://github.com/diko0071/eeg_eye_state_classification/assets/82669452/0d3f0ff1-0c95-498e-a05e-8f32916ace59)

Time-domain distribution. 

![z-transform](https://github.com/diko0071/eeg_eye_state_classification/assets/82669452/bd9640c6-9c92-4676-bd9c-2e0308c3fbe8)

Time-domain distribution with lables.

![labeled_data](https://github.com/diko0071/eeg_eye_state_classification/assets/82669452/4e5a05fa-29e5-43d1-b19f-11994a6b8118)

The data is still very noisy, so we apply a scaling average to reduce it and smooth out the short line fluctuations. 

![moving_average](https://github.com/diko0071/eeg_eye_state_classification/assets/82669452/4f9b4258-b73d-4402-87a6-65607b7e579d)

Also, a low-pass filter was applied to remove all frequencies above 30 Hz at a sampling rate of 120 Hz. 

![low-pass-filter](https://github.com/diko0071/eeg_eye_state_classification/assets/82669452/2fb52758-70a3-4680-95ed-7727d940b9ea)

The last transformation before training is wavelet transformation. Apply a morlet wavelet to add key time-frequency characteristics to the signal. 

Time-frequency domain.

![wavelet-trasform](https://github.com/diko0071/eeg_eye_state_classification/assets/82669452/9e743f86-bd21-4edc-bfaa-0c58d59563d1)

Time-domain after wavelet transformation applying. 

![wavelet_coeff_applying](https://github.com/diko0071/eeg_eye_state_classification/assets/82669452/25b0269e-b8cc-4dc4-8788-e56b7dbe78c7)


### Train part. 
The RNN architecture was chosen because of its ability to efficiently process temporal sequences of data, which is very important for processing time series data.

**Dataset:**
- 10729 data samples for 14 electrodes.
- Train set: 8583 data samples.
- Test set: 2146 data samples.

**Model parameters:**
- Number of layers: 3 hidden layers.
- Hidden layer size: 512.
- Dropout: Standard, at level 0.5.
  
**Training:**
- Number of epochs: 200 epochs
- Loss function: MSE, standard practice for regression problems. 
- Optimiser: Adam, lr = 0.001.

**Results:**
At the end of training, the model showed a loss of 0.0588 on the training dataset. 

![model_loss](https://github.com/diko0071/eeg_eye_state_classification/assets/82669452/eff36710-199a-4a44-9f2d-366e87e97f59)

On the test dataset, the loss was 0.0419. 

The coefficient of determination (R^2) is 0.8294. 

The accuracy of the model on the test sample was 97.72%.











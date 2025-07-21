# Automated-Detection-and-Removal-of-EEG-Artifacts-Using-Spectral-Features-and-Machine-Learning

Project Overview:

This project presents a machine learning-based approach to automatically detect and remove artifacts from EEG recordings using spectral and time-domain features. EEG artifacts—such as eye movements, muscle contractions, and electrical noise—often contaminate neural signals and lead to misinterpretation in clinical and research settings. The system enhances EEG signal quality by identifying, classifying, and cleaning contaminated segments, thereby improving the accuracy of downstream analysis like seizure detection or cognitive state monitoring.

Technologies Used:

    Programming Language: Python
    
    Libraries: MNE, NumPy, Pandas, Matplotlib, PyWavelets, scikit-learn, XGBoost
    
    Machine Learning Models: Random Forest, Gradient Boosting, XGBoost
  
    Dataset: Temple University Hospital (TUH) EEG Artifact Corpus v3.0.1
    
    Data Format: EDF (for EEG signals), CSV (for artifact annotations)


Objectives: 

    Develop a fully automated EEG artifact detection pipeline
    
    Extract meaningful spectral and statistical features from labeled EEG segments
    
    Train and evaluate supervised ML models to classify EEG segments as clean or artifact
    
    Apply wavelet-based denoising to remove noise while preserving signal integrity
    
    Validate results through spectral analysis across frequency bands


Methodology:

       1. Dataset Overview
          TUH EEG Artifact Corpus v3.0.1
          310 EEG recordings (EDF format) and corresponding annotations (CSV)
          100 hours of data with 160,073 artifact-labeled events
          15 artifact classes including muscle, eye movement, electrical noise, chewing, shivering, and their combinations

      2. Preprocessing Pipeline
         Recursive directory scanning to load all EDF files
         Dynamic CSV parser to handle inconsistent delimiters and header formats
         Channel name normalization using regex mapping
         Extraction of artifact-labeled EEG segments based on start/stop times
         Invalid or out-of-bounds segments skipped automatically
          Computation of spectral features (delta, theta, alpha, beta, gamma power) using Welch’s method
          Time-domain features: mean amplitude, total power, zero crossings, variance, etc.

      3. Feature Engineering
         Each EEG segment is represented by 19 handcrafted features, including:
         Band powers and peak frequencies (delta to gamma)
         Ratios such as beta/alpha and theta/delta
         Statistical descriptors like total power, duration, signal variance

      4. Machine Learning Models
         Model	Accuracy	ROC AUC	Key Parameters
         Random Forest	88.93%	0.9534	n_estimators=200
         Gradient Boosting	83.57%	0.9140	n_estimators=200, LR=0.1
         XGBoost	84.53%	0.9222	n_estimators=200, LR=0.1
         Random Forest outperformed others in accuracy and robustness
         Key features contributing to classification: duration, total power, gamma power, delta power

Artifact Removal:

     Wavelet-Based Denoising
     Used Discrete Wavelet Transform (DWT) with sym8 wavelet and 5-level decomposition
     Applied Median Absolute Deviation (MAD) based soft thresholding
     Per-channel denoising preserves the shape and duration of the EEG signals
     Effective in suppressing high-frequency noise (muscle and electrical artifacts)
     Spectral Effects After Cleaning
     Band	Expected Change
     Delta	Often decreases
     Theta	Mild changes
    Alpha	May increase
    Beta	Likely decreases
    Gamma	Significant decrease
    Post-cleaning spectral analysis showed expected reduction in beta and gamma bands, confirming successful artifact removal.

Results:

    Final training dataset: 260,297 EEG segments
    135,460 artifact segments
    124,837 clean segments
    Random Forest achieved 88.93% accuracy
    The classifier successfully detected most artifact types and their combinations
    Wavelet denoising significantly improved spectral clarity and removed high-frequency disturbances

Conclusion:

This project successfully developed an end-to-end system for automated EEG artifact detection and removal. By combining spectral and statistical features with robust machine learning models and wavelet-based denoising, the approach achieved high classification accuracy and preserved essential EEG signals.
A key strength of this work is the precise identification of artifact locations within the EEG timeline—an aspect often overlooked in prior studies. The cleaned EEG output is suitable for further diagnostic and research applications, such as seizure detection and neurofeedback.


Future Scope:

Integrate the artifact correction module into real-time EEG pipelines (e.g., neurofeedback, ICU monitoring)
Extend the system to multimodal datasets combining EEG with video or EMG for improved accuracy
Explore advanced deep learning methods (e.g., CNNs, Transformers) for end-to-end feature extraction


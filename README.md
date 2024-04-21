# Fileless Malware Detection Tool

This project aims to develop a desktop application for detecting fileless malware, a type of malware that operates solely in system memory to evade traditional detection tools. Leveraging memory forensics, machine learning algorithms, and cloud computing, our solution provides real-time monitoring and analysis of system memory to identify indicators of fileless malware activity. The application offers a user-friendly interface for proactive protection against fileless malware attacks, empowering users to mitigate security risks effectively.
## Process flow

The below image shows the process flow of our application.
![image](https://github.com/sravyaravulakolla/QuadSquad/assets/122299844/440e934d-836d-4565-bfa6-f2b36febdbae)

There are 4 main stages:
### 1)Dataset collection

In this stage, we gather the necessary data for training our machine learning model. We selected the dataset from [Kaggle](https://www.kaggle.com/datasets/luccagodoy/obfuscated-malware-memory-2022-cic), specifically the "Obfuscated Malware Memory 2022 (CIC)" dataset, which contains memory images of obfuscated malware samples. This dataset provides a diverse collection of memory images for training our model to detect fileless malware. It's essential to ensure the dataset is representative and sufficiently diverse to enable effective training.
### 2)Machine Learning Model

- **File**: `model.pkl` `Model_Training.ipynb`
- **Algorithm used**: Random forest
- **Purpose**: A trained machine learning model used for malware detection and prediction.
- **Functionality**: Predicts whether a file contains malware or is benign based on extracted features.
### 3) Feature Extraction

In this stage, we extract features from memory dumps using the Volatility tool. These features serve as the columns in our dataset, capturing various aspects of the memory image relevant to identifying fileless malware. Feature extraction involves analyzing the memory dump to identify key characteristics and extracting them into a structured format suitable for predicting using our machine learning model. These features provide valuable insights into the runtime behavior of the system and serve as inputs to our model for effective detection of fileless malware.
- **`bashscript.sh`**: Bash script for analyzing files using Volatility (a memory forensics tool).
### 4) Using Trained Model for Prediction

With the trained model and extracted features in hand, we can now use our model to make predictions on new or unseen data. This stage involves feeding the input data into the trained model and obtaining predictions or classifications based on the learned patterns from the training data.
- **`predict.py`**: Python script for making predictions using the machine learning model.

## Lambda Functions

### lambda1 (s3ToEC2)

- **Purpose**: Triggered by S3 events, this Lambda function copies uploaded files from an S3 bucket to an EC2 instance using AWS Systems Manager (SSM).
- **Functionality**:
  - Retrieves S3 bucket name and object key from the event.
  - Copies the file to the specified EC2 instance.
  - Executes commands on the EC2 instance to analyze the file.
- **Dependencies**: boto3, urllib.parse

### lambda2 (bash)

- **Purpose**: Another Lambda function for copying files to an EC2 instance and executing a bash script for analysis.
- **Functionality**:
  - Retrieves S3 bucket name and object key from the event.
  - Copies the file to the specified EC2 instance.
  - Executes a bash script (`bashscript.sh`) on the EC2 instance for analysis.
- **Dependencies**: boto3, urllib.parse

### lambda3 (predict)

- **Purpose**: Lambda function for executing Python script `predict.py` on an EC2 instance to perform predictions.
- **Functionality**:
  - Executes a Python script (`predict.py`) on the EC2 instance to perform predictions.
  - Saves the prediction results to a file (`r.txt`).
  - Copies the result file to an S3 bucket.
- **Dependencies**: boto3

### lambda4 (sendfinal)

- **Purpose**: Lambda function for sending final analysis results.
- **Functionality**:
  - Sends the final analysis results to an S3 bucket.
- **Dependencies**: None

## EC2 Instance

- **Purpose**: The EC2 instance serves as the computing environment for executing file analysis scripts.
- **Functionality**:
  - Receives files from S3 for analysis.
  - Executes bash scripts and Python scripts for malware detection and prediction.
  - Sends analysis results back to S3.

## Frontend

### frontend.py

- **Purpose**: PyQt5-based GUI application for uploading files and monitoring the analysis process.
- **Functionality**:
  - Allows users to upload files for analysis.
  - Provides visual feedback on the analysis progress.
  - Displays analysis results to the user.
- **Dependencies**: PyQt5

## Usage

1. Ensure proper configuration of AWS IAM roles, S3 bucket, EC2 instance, and Lambda functions.
2. Upload files to the configured S3 bucket.
3. Lambda functions will automatically trigger file analysis on the EC2 instance.
4. Use the frontend application to monitor the analysis progress and view results.
5. Results will be stored in the S3 bucket.

## Memory Samples

Below are few memory samples we used for testing our application.
- [Memory Sample 1](https://drive.google.com/file/d/148Xx4mrBbEpbbeC3Uk3Zi0R0xcDrMQg_/view?usp=sharing)
- [Memory Sample 2](https://drive.google.com/file/d/1CzTifXOpjYq4l3za7tStuvfU45EDwk6y/view?usp=sharing)
- [Memory Sample 3](https://drive.google.com/file/d/1rnCSRI9ORWoieZLcTKydTjxEW_H5gHpT/view?usp=sharing)
## Screenshots of Desktop application

### GUI Interface

![image](https://github.com/sravyaravulakolla/QuadSquad/assets/122299844/795041ad-2b37-4cd7-b103-be86b78f9a63)
This screenshot showcases the graphical user interface (GUI) of our application, providing users with an intuitive and user-friendly interface to interact with.
### Uploading memory dumps

![image](https://github.com/sravyaravulakolla/QuadSquad/assets/122299844/659842f2-f57d-443b-8d0e-aac1ed66550f)
![image](https://github.com/sravyaravulakolla/QuadSquad/assets/122299844/a4fdf59c-762d-418f-99ad-96284af3547c)
In this screenshot, users can see the uploading part of our application, where they can upload RAM Dump for analysis.
### Prediction

![image](https://github.com/sravyaravulakolla/QuadSquad/assets/122299844/83514415-bf62-419d-bb48-92f0cf909c97)
This screenshot demonstrates the prediction part of our application, displaying the results or predictions generated based on the uploaded data.

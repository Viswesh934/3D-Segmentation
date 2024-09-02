# 3D-Segmentation

## Overview
This assignment involves working with the FLARE22 dataset, which includes medical imaging data (CT scans) and their corresponding segmentation labels. The objectives include downloading, preprocessing, and analyzing the dataset using Python libraries such as `nibabel`, `matplotlib`, and `numpy`.

![image](https://github.com/user-attachments/assets/440f52aa-f297-4b3d-b445-5860338fda05)


## Objectives
1. **Data Download and Extraction**:
   - Download the FLARE22 training dataset from a specified URL.
   - Extract the dataset and confirm the extraction.

2. **Data Exploration**:
   - List the contents of the extracted dataset to understand its structure.
   - Load and visualize the CT scans and their corresponding segmentation labels.

3. **Data Preprocessing**:
   - Normalize the CT scan data for better analysis and visualization.
   - Ensure that the segmentation labels remain in their original categorical form.

## Prerequisites
- Python 3.x
- Required Libraries: `os`, `requests`, `zipfile`, `nibabel`, `matplotlib`, `numpy`

## Installation
Ensure that the necessary Python libraries are installed:

```bash
pip install nibabel matplotlib numpy requests
```

## Steps to Run the Script

### 1. Download and Extract the Dataset
Download the FLARE22 training dataset:

```python
import os
import requests
import zipfile

url = "https://zenodo.org/records/7860267/files/FLARE22Train.zip?download=1"
destination = "FLARE22Train.zip"

response = requests.get(url, stream=True)
with open(destination, "wb") as file:
    for chunk in response.iter_content(chunk_size=8192):
        if chunk:
            file.write(chunk)

# Confirm the file has been downloaded
if os.path.exists(destination):
    print(f"File downloaded successfully and saved as {destination}")
else:
    print("File download failed")

# Extract the dataset
with zipfile.ZipFile(destination, 'r') as zip_ref:
    zip_ref.extractall("FLARE22Train")
print("File extracted successfully")
```

### 2. Explore the Dataset
List the contents of the extracted dataset to understand its structure:

```python
dataset_dir = "FLARE22Train"
for root, dirs, files in os.walk(dataset_dir):
    for file in files:
        print(os.path.join(root, file))
```

### 3. Load and Visualize CT Scans
Load a sample CT scan and its corresponding segmentation label using `nibabel`:

```python
import nibabel as nib
import numpy as np

ct_scan_path = "FLARE22Train/FLARE22Train/images/FLARE22_Tr_0001_0000.nii.gz"
label_path = "FLARE22Train/FLARE22Train/labels/FLARE22_Tr_0001.nii.gz"

ct_scan = nib.load(ct_scan_path)
ct_data = ct_scan.get_fdata()

label = nib.load(label_path)
label_data = label.get_fdata()

print(f"CT Scan shape: {ct_data.shape}")
print(f"Label shape: {label_data.shape}")
```

![image](https://github.com/user-attachments/assets/b0861d54-d5e8-4915-9367-5cdc1ed2a1c5)


### 4. Normalize the CT Scan Data
Normalize the CT scan values to a range between 0 and 1:

```python
ct_data_normalized = (ct_data - np.min(ct_data)) / (np.max(ct_data) - np.min(ct_data))

# Ensure the segmentation labels are correctly formatted as integer values
label_data = label_data.astype(np.int32)

print(f"CT Scan after normalization: min {np.min(ct_data_normalized)}, max {np.max(ct_data_normalized)}")
```

## Results
- **Data Integrity**: Successfully downloaded, extracted, and verified the dataset.
- **Visualization**: Loaded and visualized the CT scans and labels, confirming their structure and consistency.

  ![image](https://github.com/user-attachments/assets/6b422d6d-2df8-42cd-b3aa-07ac6ec735c6)

- **Normalization**: Successfully normalized the CT scan data, with values ranging between 0 and 1.
  
![image](https://github.com/user-attachments/assets/2281f4ce-ea61-4fd9-aa24-65781c4c56cc)


## Future Work
This assignment lays the groundwork for more advanced analysis, such as:
- **Segmentation Model Training**: Use the preprocessed data to train deep learning models for automatic segmentation.
- **Advanced Visualization**: Implement 3D visualization techniques to better understand the CT scans and segmentation masks.
- **Further Preprocessing**: Apply techniques like data augmentation and noise reduction to enhance model training.

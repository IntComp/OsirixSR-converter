# OsirixSR Converter

Convert Osirix ROI into standardized formats.

## Overview

The [OsiriX dicom Viewer](https://www.osirix-viewer.com/) 
exports ROI annotations
into the proprietary "OsiriX SR" format
that is hard to parse and read.

This tool parseses data from OsiriX files and converts it into widely compatible formats for easier use and analysis.


### Key Features
- No predefined file structure required; automatically processes studies based on DICOM metadata.
- Supports batch processing.
- Generate multiple output formats:
    - NumPy arrays
    - PNG images
    - [DICOM RT Structure Sets](https://dicom.nema.org/dicom/2013/output/chtml/part03/sect_A.19.html)
    - NIFTI volumes


## Installation

```bash
# clone the code, do not omit the --recursive option.
git clone https://github.com/intcomp/OsiriXConvert --recursive
cd OsiriXConvert
# install dependencies
pip install -r requirements.txt
# convert
python convert.py /path/to/your/cases
```

## Usage

### 1. Export ROI in OsiriX
1. Select all the patients that you want to export.
2. right click -> export -> Dicom file(s)
3. Check the "include ROIs"
4. Choose an target directory to save.


Basic conversion:
```bash
python convert.py /path/to/your/exported/files
```

The converter will:
1. Recursively locate all DICOM and OsirixSR files
2. Parse and validate OsirixSR annotations
3. Associate annotations with corresponding DICOM series
4. Generate converted outputs in multiple formats


## Quick Start With an Example
The repo includes an example case from the public ProstateX dataset, containing T2-weighted MRI images with corresponding ROI annotations for the bladder and prostate.

You can try the included example:
```
python convert.py example
```

The converter will generate the following output files within the original study directory:
- DICOM RT Structure sets (.dcm)
- Binary masks as NumPy arrays (.npy) and PNG images
- NIFTI volumes (.nii.gz) for each ROI


### Directory structure before conversion:

```
example
└── Prostatex-0000
    ├── OsiriX_ROI_SR
    │   ├── IM-0002-0000-0001.dcm
    │   ├── IM-0002-0000-0002.dcm
    │   ├── ...
    └── t2_tse_tra
        ├── IM-0001-0001.dcm
        ├── IM-0001-0002.dcm
        ├── ...
```


### Directory structure afterwards:
```
example
└── Prostatex-0000
    ├── OsiriX_ROI_SR
    │   ├── IM-0002-0000-0001.dcm
    │   ├── IM-0002-0000-0002.dcm
    │   ├── ...
    ├── rt-struct
    │   └── t2_tse_tra_rtstruct.dcm
    ├── binary_masks
    │   ├── bladder_0001.npy
    │   ├── bladder_0001.png
    │   ├── bladder_0002.npy
    │   ├── bladder_0002.png
    │   ├── ...
    │   ├── prostate_0001.npy
    │   ├── prostate_0001.png
    │   ├── prostate_0002.npy
    │   ├── prostate_0002.png
    │   ├── ...
    ├── nifti-bladder.nii.gz
    ├── nifti-prostate.nii.gz
    └── t2_tse_tra
        ├── IM-0001-0001.dcm
        ├── IM-0001-0002.dcm
        ├── ...
```

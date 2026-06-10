# Custom Image Classifier

A desktop application for creating and using a custom image classifier through a graphical interface.

The application uses a pretrained **ResNet-18** network as a feature extractor. Users can define their own classes, train a classifier from example images, classify individual files, and process complete folders without writing training code manually.

## Features

- Create custom image classes
- Add and remove classes from the training set
- Train a classifier directly from the graphical interface
- Use a pretrained ResNet-18 backbone for image feature extraction
- Classify a single image
- Classify all supported images in a folder
- Display the predicted class in the preview panel
- Show classification progress and estimated time remaining
- Preview the image currently being processed
- Keep a detailed activity log
- Run long operations without freezing the interface
- Dark desktop interface built with Tkinter and `ttk`

## Application Interface

The interface is divided into three main areas:

### Preview

Displays the currently selected or processed image together with its predicted class.

### Activity Log

Shows model-loading messages, training progress, classification results, warnings, and errors.

### Controls

Provides controls for:

- Single-image classification
- Folder classification
- Progress and ETA
- Log clearing
- Class management
- Model training

## Requirements

- Python 3.9 or newer
- Tkinter
- PyTorch
- torchvision
- Pillow

Install the main Python dependencies with:

```bash
pip install torch torchvision pillow
```

Tkinter is included with most standard Python installations.

On Debian or Ubuntu, install it separately when required:

```bash
sudo apt update
sudo apt install python3-tk
```

## Installation

### 1. Download or clone the project

```bash
git clone <repository-url>
cd <repository-folder>
```

### 2. Create a virtual environment

#### Windows

```powershell
python -m venv .venv
.venv\Scripts\activate
```

#### Linux or macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install the dependencies

```bash
pip install torch torchvision pillow
```

## Running the Application

Assuming the main script is named `custom_image_classifier.py`, run:

```bash
python custom_image_classifier.py
```

On systems where Python 3 uses a separate command:

```bash
python3 custom_image_classifier.py
```

Wait until the status bar reports that the backbone has loaded.

A successful startup may display:

```text
ResNet-18 backbone loaded (feature extractor).
```

## Training a Custom Classifier

The classifier must be trained before it can classify images into your custom categories.

### 1. Add Classes

Select:

```text
+ Add Class
```

Create one class for each category that the model should recognize.

Example classes:

```text
cats
dogs
cars
flowers
```

### 2. Provide Training Images

Add representative images for every class.

For best results:

- Use several images per class
- Include different viewpoints
- Include different lighting conditions
- Avoid using nearly identical images only
- Keep class sizes reasonably balanced
- Use clear images where the target object is visible
- Avoid incorrect or ambiguous labels

A practical starting point is at least 20 to 50 images per class. More varied training data usually improves reliability.

### 3. Remove Incorrect Classes

Select a class and use:

```text
Remove Class
```

Removing a class may also remove its associated training configuration, depending on the application's implementation.

### 4. Train the Model

Select:

```text
Train
```

During training, the application extracts image features with ResNet-18 and learns to separate the custom classes.

Do not close the application while training is in progress.

When training completes, the training-status area should change from:

```text
Not trained
```

to a trained or ready state.

## Classifying a Single Image

1. Select **Browse**.
2. Choose an image.
3. Select **Classify**.
4. Review the image preview and predicted class.
5. Check the activity log for additional information.

The classifier must already be trained.

## Classifying a Folder

1. Select **Choose...** under **Folder Classification**.
2. Choose a folder containing images.
3. Select **Classify Folder**.
4. Monitor:
   - Current image preview
   - Predicted class
   - Progress percentage
   - Estimated time remaining
   - Activity log

If no folder is selected, the application uses the default folder:

```text
images
```

Create this folder next to the main script when using the default configuration.

## Suggested Project Structure

```text
project/
├── custom_image_classifier.py
├── README.md
├── images/
│   ├── test_01.jpg
│   └── test_02.png
├── training_data/
│   ├── cats/
│   │   ├── cat_01.jpg
│   │   └── cat_02.jpg
│   └── dogs/
│       ├── dog_01.jpg
│       └── dog_02.jpg
└── models/
    └── custom_classifier.*
```

The exact training-data and model paths may differ depending on the implementation.

## How It Works

### ResNet-18 Feature Extractor

The application uses a pretrained ResNet-18 model to convert each image into a numerical feature representation.

Instead of training the entire neural network from the beginning, the application reuses visual features learned from a large image dataset.

This approach provides several advantages:

- Faster training
- Lower hardware requirements
- Better results with smaller custom datasets
- No need to train a deep network from scratch

### Custom Classification Layer

The extracted features are used to train a classifier for the classes created by the user.

The custom classifier learns the differences between the supplied categories while ResNet-18 handles general visual feature extraction.

### Background Processing

Model loading, training, and folder classification should run outside the main Tkinter event loop so that the interface remains responsive.

GUI updates should be scheduled through Tkinter's `after()` method.

## Training Data Guidelines

The quality of the training dataset has a major effect on prediction quality.

### Use Balanced Classes

Avoid using 500 images for one class and only 10 for another.

### Include Variation

For each class, include variation in:

- Position
- Scale
- Rotation
- Background
- Lighting
- Camera
- Distance
- Object appearance

### Avoid Data Leakage

Do not use the same image in both training and evaluation data.

Near-duplicate images can also produce misleadingly high accuracy.

### Use Meaningful Class Names

Prefer:

```text
golden_retriever
sports_car
red_rose
```

Instead of:

```text
class1
class2
test
```

### Remove Incorrect Images

A single incorrectly labeled image can reduce classifier quality, especially when the dataset is small.

## Supported Image Formats

Typical supported formats include:

- JPEG (`.jpg`, `.jpeg`)
- PNG (`.png`)
- BMP (`.bmp`)
- GIF (`.gif`)
- WebP (`.webp`)

Actual folder support depends on the extension list configured in the Python script.

## Limitations

- The classifier can recognize only the custom classes included during training.
- It may still assign an unknown image to one of the known classes.
- Prediction confidence does not guarantee correctness.
- Small or unbalanced datasets can produce unreliable results.
- Similar-looking classes require more training images.
- Blurry, dark, cropped, or low-resolution images may reduce accuracy.
- CPU-only training and classification can be slow for large datasets.
- The application may need to be retrained after classes or training images change.
- The current interface does not visibly include a dedicated validation or test-set evaluator.
- Model persistence depends on whether the implementation saves the trained classifier to disk.

## Troubleshooting

### The application starts but classification does not work

Confirm that:

- At least two classes have been added
- Each class contains training images
- Training completed successfully
- The training status no longer says `Not trained`

### The Train button does nothing

Check the activity log for:

- Empty classes
- Missing folders
- Unsupported images
- Invalid file paths
- Permission errors
- Model-loading errors

### `ModuleNotFoundError: No module named 'torch'`

Install the required packages:

```bash
pip install torch torchvision pillow
```

### `ModuleNotFoundError: No module named 'tkinter'`

On Debian or Ubuntu:

```bash
sudo apt install python3-tk
```

### The ResNet-18 backbone does not load

Check:

- Internet connectivity during the first run
- Firewall or proxy restrictions
- Available disk space
- PyTorch and torchvision compatibility
- Permission to write model files to the cache directory

### Training accuracy is poor

Try the following:

- Add more images
- Remove incorrectly labeled images
- Balance the number of images per class
- Include more visual variation
- Use clearer and higher-resolution images
- Avoid categories that are visually almost identical
- Separate the main object from distracting backgrounds

### Folder classification finds no images

Confirm that:

- The selected folder exists
- It contains supported image formats
- The application has permission to access it
- The file extensions are included in the script's supported-extension list

## Recommended Improvements

Potential future enhancements include:

- Save and load trained classifiers
- Add validation and test-set evaluation
- Display accuracy, precision, recall, and confusion matrices
- Show prediction confidence scores
- Add an “unknown class” rejection threshold
- Support drag-and-drop
- Add image augmentation
- Add GPU and Apple Silicon acceleration
- Export folder results to CSV or JSON
- Add recursive folder scanning
- Add a cancel button for training and batch classification
- Display images assigned to the wrong class
- Prevent duplicate training images
- Add class renaming
- Store project settings in a configuration file
- Package the program as a standalone executable

## Privacy

Training and classification are performed locally unless the application code explicitly connects to an external service.

The pretrained ResNet-18 weights may be downloaded during the first run. User-selected images do not need to be uploaded to a remote API.

## License

Add the license used by the project.

Example:

```text
MIT License
```

## Acknowledgements

- PyTorch
- torchvision
- Pillow
- Tkinter
- ResNet
- ImageNet

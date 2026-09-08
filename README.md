# White Blood Cell Classification

This project uses deep learning to classify white blood cells from microscope
images. It takes one cell image as input and predicts four properties at the
same time:

- **Cell type:** Basophil, Eosinophil, Lymphocyte, Monocyte, or Neutrophil
- **Cell shape:** round or irregular
- **Nucleus shape:** one of six nucleus-shape categories
- **Cytoplasm vacuole:** whether a vacuole is present (`yes` or `no`)

The project is designed as a multi-task image-classification model. Instead of
training four separate models, one neural network learns from the image and
has four output heads, one for each prediction.

## What the project does

The notebook follows these steps:

1. Loads the image labels and checks the dataset.
2. Examines the class distribution and displays example images.
3. Resizes images to 224 x 224 pixels.
4. Applies small image changes during training, such as flips, rotations,
	zoom, brightness, and contrast changes.
5. Splits the data into training, validation, and test sets. Rare combinations
	of labels are kept in the training data so they are not lost.
6. Uses class weights to give more attention to under-represented classes.
7. Trains a MobileNetV2-based model in two stages:
	- train the new output layers while the base model is frozen;
	- fine-tune the last part of the base model with a small learning rate.
8. Evaluates the model and creates predictions for future images.

## Dataset

The labeled development dataset contains 5,000 images. Each row in
`dev_data.csv` contains an image ID and four labels:

```text
imageID,label,cell_shape,nucleus_shape,cytoplasm_vacuole
Img_00001,Lymphocyte,round,unsegmented-round,no
```

The `Hematoxylin/` directory contains the cell images used by the notebook.
`future_data.csv` contains image IDs for images that do not have labels. The
model uses these IDs to create predictions in `predictions.csv`.

## Results

The reported test-set accuracy is approximately:

| Prediction | Accuracy |
| --- | ---: |
| Cell type | 0.97 |
| Cell shape | 0.87 |
| Cytoplasm vacuole | 0.85 |
| Nucleus shape | 0.67 |

Nucleus shape is the most difficult prediction because it has six classes and
some classes have fewer examples. These results are model results on this
dataset; they are not a medical diagnosis.

## How to run it

1. Install Python with TensorFlow, pandas, NumPy, scikit-learn, Matplotlib,
	Seaborn, and Joblib.
2. Open `main.ipynb` in Jupyter Notebook or VS Code.
3. Make sure the notebook is using the project folder as its working folder.
4. Run the notebook cells from top to bottom.

The notebook performs the analysis, trains the model, evaluates it on the test
set, and writes prediction files and saved model information into the project
folder.

## Project files

- `main.ipynb`: complete data analysis, training, and evaluation workflow
- `dev_data.csv`: labels for the development images
- `future_data.csv`: image IDs to classify
- `predictions.csv`: model predictions for the future images
- `Hematoxylin/`: white blood cell images
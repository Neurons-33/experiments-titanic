# Exploring Training Behavior and Generalization on Titanic

## Experiment Setup

### Dataset
- [titanic.csv](https://www.kaggle.com/competitions/titanic/data?select=train.csv)  
- Train / Validation split: 80% / 20%  

### Feature Processing
- StandardScaler (fit on full dataset)  
- Missing values filled with median  

### Model
- MLP with 1 or 2 hidden layers  
- Architectures:
  - input_dim → 16 → 1  
  - input_dim → 16 → 16 → 1  
- Activation: ReLU  

### Training
- Loss: BCEWithLogitsLoss  
- Optimizer: Adam  
- Learning rate: 1e-3  
- Epochs: 1500  

<!-- spacer -->

## Training Behavior


### Depth = 1 Configuration
<img width="508" height="136" alt="image" src="https://github.com/user-attachments/assets/b3596088-fac7-4e8d-8c18-6e08d63f4a72" />

<!-- spacer -->
**Observation**
- Training and validation loss decrease at a similar pace.
- The generalization gap remains small (≈1–2%) across epochs.
- Validation loss stabilizes around 0.43–0.45.

**Interpretation**
- The model stays within a stable generalization regime.
- No clear signs of overfitting are observed within the training range.
- Further training may yield diminishing returns.

<!-- spacer -->

### Depth = 2 Configuration
<img width="509" height="139" alt="image" src="https://github.com/user-attachments/assets/cc8abb4e-d6f1-4510-bbcd-2cd2cde08217" />

<!-- spacer -->
**Observation**
- Validation loss improves rapidly in early epochs (200–400).
- Training loss continues to decrease steadily.
- The generalization gap increases significantly after early training.

**Interpretation**
- The generalization boundary becomes visible early.
- Continued training mainly improves training loss, not validation loss.
- This indicates the onset of overfitting.

## Summary

- Both configurations achieve similar best validation loss (≈0.43–0.45).
- Depth = 2 reaches lower validation loss earlier but overfits sooner.
- Depth = 1 converges more slowly and remains more stable.

## Feature Impact on Training Behavior

### Depth = 1 Configuration (With Additional Feature: Pclass)
<img width="508" height="137" alt="image" src="https://github.com/user-attachments/assets/0bf3008b-128d-49c7-a436-85882b1c6d83" />

<!-- spacer -->
**Observation**
- After incorporating `Pclass`, a clear generalization boundary becomes observable even in the depth = 1 configuration.
- Validation loss begins to plateau while training loss continues to decrease.
- Overfitting behavior emerges earlier compared to the depth = 1 setting without `Pclass`.

**Interpretation**
- The inclusion of `Pclass` increases the effective model capacity without changing network depth.
- As a structured and non-continuous feature, `Pclass` introduces discrete decision cues that the model can exploit.
- This suggests that feature selection can modulate model complexity and overfitting behavior, even in shallow architectures.

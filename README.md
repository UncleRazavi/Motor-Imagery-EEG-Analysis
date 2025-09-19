# BCI Competition IV Dataset 1 - Motor Imagery EEG Analysis

## Dataset Description

This project uses data from the BCI Competition IV, Dataset 1, which focuses on motor imagery EEG signals. The dataset was recorded from healthy subjects performing motor imagery tasks (e.g., imagining left hand, right hand, or foot movement) without feedback. The data is provided by the Berlin BCI group and is available at: https://www.bbci.de/competition/iv/desc_1.html

### Experimental Setup
- **Calibration Data**: Visual cues (arrows) instructed subjects to perform specific motor imagery tasks for 4 seconds, interleaved with rest periods. Markers indicate cue onset and class.
- **Evaluation Data**: Acoustic cues (words) instructed subjects, with variable-length imagery and rest periods. No markers are provided for evaluation data.
- **EEG Recording**: 59 channels, densely covering sensorimotor areas, sampled at 1000 Hz (downsampled to 100 Hz for some files). Data is stored in MATLAB `.mat` format with continuous EEG (`cnt`), marker info (`mrk`), and metadata (`nfo`).

### Data Structure
- `cnt`: Continuous EEG signals, shape [time x channels], INT16. Convert to microvolts with `cnt = 0.1 * cnt.astype(np.float32)`.
- `mrk`: Marker struct (for calibration data) with cue positions (`pos`) and class labels (`y`, -1 or 1).
- `nfo`: Metadata including sampling rate (`fs`), channel labels (`clab`), and class names.

## Project Implementation

This notebook implements a full pipeline for EEG motor imagery classification using the calibration data:

1. **Data Loading & Preprocessing**
   - Load `.mat` files and extract EEG, marker, and metadata.
   - Convert EEG to microvolts and apply a bandpass filter (8–30 Hz) to focus on motor imagery frequencies.
   - Epoch the data: extract time windows after each cue for analysis.

2. **Feature Extraction & Classification**
   - **CSP (Common Spatial Patterns)**: Extract spatial features that maximize variance between classes.
   - **Normalization**: Standardize features using `StandardScaler`.
   - **LDA (Linear Discriminant Analysis)**: Classify motor imagery using spatial features.
   - The pipeline is built using `sklearn`'s `Pipeline` for reproducibility.
   - Cross-validation is used to estimate performance, and the final model is evaluated on a held-out test set.

3. **Results & Visualization**
   - Outputs classifier predictions to `Result_BCIC_IV_ds1.txt` as required by the competition.
   - Visualizes raw EEG, CSP patterns, classifier decision boundaries, confusion matrix, and ROC curve.
   - Computes and plots Power Spectral Density (PSD) for frequency domain analysis.

4. **Additional Feature Extraction**
   - Computes bandpower features (e.g., alpha and beta bands) and evaluates an SVM classifier as an alternative approach.

## References
- BCI Competition IV: https://www.bbci.de/competition/iv/desc_1.html
- Blankertz, B., Dornhege, G., Krauledat, M., Müller, K.-R., & Curio, G. (2007). The non-invasive Berlin Brain-Computer Interface: Fast acquisition of effective performance in untrained subjects. NeuroImage, 37(2), 539-550. [PDF](http://ml.cs.tu-berlin.de/publications/BlaDorKraMueCur07.pdf)

## Acknowledgements
Data provided by the Berlin BCI group: Berlin Institute of Technology and Fraunhofer FIRST.

---

For more details on the dataset and competition, visit the [official description page](https://www.bbci.de/competition/iv/desc_1.html).
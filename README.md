Road Damage Patch Classification

Assignment 03 · Team Z

ABOUT THE PROJECT
Roads deteriorate constantly, and finding damage early is far cheaper than repairing it late. Traditionally, inspectors drive or walk along roads and judge damage by eye, which is slow, costly, and inconsistent from person to person. This project explores how a computer can do part of that job by looking at road images and deciding what kind of damage, if any, is present.

THE CORE IDEA
Instead of asking a model to understand a whole road scene at once, we break the problem into smaller pieces. A road image is divided into small square patches, and the model classifies each patch on its own, for example as cracked, pothole, or undamaged [adjust to your classes].

This works well for three reasons. First, the task is simpler: a patch contains one local texture, so the model only needs to learn what damage looks like up close. Second, there is more training data: one image produces many patches, which gives the model far more examples to learn from. Third, it opens a path to localization: once patches can be classified, running the model across every patch of a new image shows where the damage is.


HOW IT WORKS
Step 1: Preparing the data
Road images are cropped into fixed-size patches [e.g., 64x64 pixels], and each patch is labeled with its damage type. The patches are split into training, validation, and test sets. The model learns from the training set, the validation set guides tuning decisions, and the test set is held back until the end to give an honest measure of performance.

Step 2: Preprocessing and augmentation
Pixel values are resized and normalized so the model trains stably. Training patches are also randomly flipped, rotated, or brightness-adjusted [adjust to yours]. Since real roads appear under different lighting and angles, this teaches the model to recognize damage rather than memorize specific images.

Step 3: Building the model
The classifier is a convolutional neural network (CNN) [or a pretrained model fine-tuned on our data]. Early layers detect simple features like edges and lines, and deeper layers combine them into patterns such as the branching shape of a crack or the dark, irregular hollow of a pothole. A final layer outputs a probability for each damage class, and the highest one becomes the prediction.

Step 4: Training
The model predicts labels for batches of training patches, and a loss function measures how wrong it is. The optimizer then adjusts the model's weights to reduce that error. This repeats over many epochs, while validation performance is monitored to catch overfitting, where the model memorizes training data instead of learning general patterns.

Step 5: Evaluation
The final model is tested on patches it has never seen. Accuracy alone can be misleading when some classes are much rarer than others, so we also look at precision, recall, and F1-score for each class, plus a confusion matrix showing which damage types get mistaken for each other.

Step 6: Error analysis
Finally, we look at the patches the model got wrong. Are cracks confused with road markings or shadows? Are small potholes missed? These patterns show the model's real limits and suggest improvements.


LIMITATIONS
The model classifies isolated patches, so it has no sense of the wider scene. Shadows, stains, and road markings can look like damage, and performance depends heavily on how varied the training data is. Rare damage types are harder to learn when examples are scarce.


FUTURE WORK
Natural next steps include running the classifier over full images to map damage locations, moving to object detection or segmentation for exact boundaries, and training on more diverse roads and weather conditions.

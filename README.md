# Social Distancing Monitoring Application

![social_distance_output](https://github.com/user-attachments/assets/ff87058e-9492-49da-8e50-feb8358f56cc)

This project is a Python-based application that leverages computer vision techniques to monitor social distancing in public spaces. By detecting humans in video footage and identifying unsafe distances between them, this application aims to help promote social distancing practices.

---

## Key Features
- **Human Detection:** Utilizes the MobileNetSSD model to detect humans in video footage.
- **Distance Calculation:** Measures the distances between detected individuals' centroids.
- **Violation Detection:** Identifies individuals who are too close to each other based on a predefined safety threshold.
- **Real-Time Processing:** Processes video streams and marks detected individuals with visual indicators (green for safe, red for unsafe).

---

## Function Documentation

### 1. **`detect(frame, network)`**
This function is responsible for detecting humans in a given video frame.

#### Parameters:
- `frame`: A single video frame (image) from the video stream.
- `network`: The MobileNetSSD deep learning model for object detection.

#### Workflow:
1. **Blob Creation:** The input frame is pre-processed into a blob using `cv2.dnn.blobFromImage`. This scales and normalizes the image to match the model's input requirements.
2. **Forward Pass:** The blob is passed through the MobileNetSSD model to perform inference.
3. **Bounding Box Calculation:** For each detection with a high confidence score (above 0.7) and class ID of 15 (person), a bounding box is calculated.
4. **Centroid Calculation:** The center of the bounding box is computed to aid in distance measurement.

#### Output:
- A list of detected humans with their confidence scores, bounding boxes, and centroids.

The `detect` function provides the foundational capability of identifying people in the video, which is the first step in monitoring social distancing.

---

### 2. **`detect_violations(results)`**
This function identifies pairs of individuals who are too close to each other.

#### Parameters:
- `results`: A list of detection results containing bounding boxes and centroids from the `detect` function.

#### Workflow:
1. **Bounding Box Width Calculation:** Calculates the width of each bounding box to determine a reference distance.
2. **Distance Matrix:** Computes the pairwise Euclidean distances between centroids of detected individuals using the `euclidean_dist` function.
3. **Violation Detection:** Compares distances with a safety threshold (1.2 times the smallest bounding box width). If the distance is less than the threshold, the individuals are flagged as violations.

#### Output:
- A set of indices representing individuals who are too close to each other.

This function ensures that the application can identify and highlight unsafe interactions between people.

---

### 3. **`euclidean_dist(A, B)`**
Calculates pairwise Euclidean distances between two sets of points.

#### Parameters:
- `A`: An array of coordinates (e.g., centroids of bounding boxes).
- `B`: Another array of coordinates.

#### Workflow:
1. **Distance Formula:** Implements the formula \( \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2} \) using matrix operations for efficiency.
2. **Matrix Output:** Returns a matrix where each element represents the distance between a point in A and a point in B.

#### Output:
- A distance matrix with shape `(len(A), len(B))`.

Efficient distance computation is essential for determining proximity violations in real-time.

---

### 4. **Main Video Processing Logic**
The core loop processes the input video frame by frame and applies the above functions.

#### Workflow:
1. **Video Capture:** Opens the input video file using `cv2.VideoCapture`.
2. **Frame Processing:**
   - Each frame is passed through the `detect` function to get detections.
   - The `detect_violations` function identifies unsafe distances.
3. **Annotations:**
   - Bounding boxes are drawn around detected individuals.
   - Green boxes indicate safe individuals, while red boxes mark violations.
4. **Video Writing:** Annotated frames are saved to the output video file using `cv2.VideoWriter`.
5. **Performance Labeling:** Displays inference time on each frame to monitor processing speed.

This loop integrates all the components and ensures the application functions as intended from start to finish.

---


## How to Use the Application
1. **Setup:** Install dependencies and load the model files (`MobileNetSSD_deploy.prototxt` and `MobileNetSSD_deploy.caffemodel`).
2. **Input Video:** Replace `input.mp4` with the path to your video file.
3. **Run the Script:** Execute the script in your preferred environment (e.g., Jupyter Notebook or standalone Python).
4. **Output Video:** The annotated video will be saved as `output.mp4` in the project directory.

---

## Conclusion
This application demonstrates how computer vision can be applied to promote public health and safety. By monitoring social distancing, it provides a practical tool for ensuring compliance with safety guidelines in crowded spaces.


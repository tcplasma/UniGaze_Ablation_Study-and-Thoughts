1. Core Objective
This experiment evaluates the feature representation capabilities of UniGaze (domain-specific), DINOv2, and ViT (general-purpose) using the Olivetti Faces dataset. The study employs Linear Probing, where the backbone's filters (weights) and biases are frozen, and only a terminal GazeRegressor is trained.

2. Qualitative Superiority vs. Quantitative Metrics
While the numerical results (Angular Error) may appear similar across models, a qualitative assessment of the visual predictions (the gaze arrows) reveals a significant gap:

UniGaze’s Zero-shot Power: Despite never being trained on this specific dataset, UniGaze's predicted vectors are visibly more aligned with the actual "intent" and "look" of the human subjects.

Physical Plausibility: UniGaze produces physically-plausible gaze vectors that respect the biological constraints of the human eye, whereas general models sometimes produce mathematically correct but physiologically unlikely directions.

3. Critical Evaluation of the Experimental Design
The data metrics in this notebook might be deceptive due to the following structural limitations:

A. The "PnP Label" Trap
The Ground Truth (GT) in this experiment is derived via the Perspective-n-Point (PnP) algorithm.

The Flaw: PnP calculates Head Pose, not Fine-grained Gaze. It assumes the person is looking straight ahead relative to their face.

The Conflict: UniGaze is designed to capture subtle eyeball movements (eye-in-head). When a subject looks "sideways" without moving their head, UniGaze's accurate prediction is penalized by the PnP-based GT, which erroneously expects a forward-facing vector.

Ideal Setup: To truly validate UniGaze, one would need a Controlled Laboratory Setting equipped with high-precision Gaze Trackers or Vicon Motion Capture systems.

B. Robustness vs. Dataset Fitting (The Noise Test)
The experiment involving noise injection exposes the underlying strategy of the general models:

General Models (DINOv2/ViT): Their high scores on clean data rely heavily on Dataset Overfitting. They learn to map the specific, static background and lighting of the Olivetti dataset to the PnP labels. Once noise is introduced, they lose their "landmarks" and performance collapses.

UniGaze’s Robustness: UniGaze acts as a "Gaze Physicist." Its filters are pre-trained to ignore environmental noise and focus on stable Physiological Geometry. It maintains accuracy because it understands the structure of the eye, not just the pixels of the dataset.

4. Final Conclusion
Statisticians vs. Physicists: DINOv2 is a world-class "Image Statistician," capable of finding patterns in any data it's given. UniGaze is a "Gaze Physicist" that possesses Physical Priors and Cross-dataset Generalization.

Metrics are not Absolute: In specialized tasks like gaze estimation, L2 Loss or Angular Error can be misleading if the labels (GT) are proxies. Qualitative analysis—observing if the model "understands" the gaze—is often a more valid indicator of real-world utility.

The Value of Domain Pre-training: The experiment proves that specialized pre-training encodes a level of robustness and geometric logic that general-purpose training cannot match, even when general models have significantly more parameters.

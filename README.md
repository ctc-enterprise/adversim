# adversim

## Physical Adversarial Attack Simulation Framework

### Overview
This repository contains a research-grade simulation framework for evaluating the robustness of Object Detection models (specifically Faster R-CNN) against Physical Adversarial Attacks.

Unlike "Digital Attacks" (which manipulate pixels imperceptibly), these scripts simulate "Physical Attacks" (patches, stickers, and camouflage) that can theoretically be printed and placed in the real world to fool AI systems.

### 📂 Project Structure

#### 1. `adversarial_attack_simulation.py/adversarial_attack_simulation.ipynb` (Basic Prototype)
**Goal:** Compare the fundamental differences between Digital and Physical attack vectors.

*   **Digital Attack (PGD):** Implements Projected Gradient Descent to add invisible noise to the entire image. This demonstrates the model's fragility to pixel-level perturbations.
*   **Physical Attack (Patch):** Generates a localized "Adversarial Patch" using Expectation Over Transformation (EOT). This demonstrates how a visible, localized pattern can suppress detections even when the object is moved or rotated.

**Output:** Generates side-by-side comparisons of baseline detections, digital noise attacks, and physical patch attacks.

#### 2. `physical_attack_simulation.py/physical_attack_simulation.ipynb` (Advanced Research Lab)
**Goal:** A modular simulation of the three specific threat vectors defined in physical adversarial literature (Eykholt et al., Wei et al.).

*   **Feature 1: Object Vanishing (Hiding):** Optimizes a patch to suppress the "objectness" score, effectively acting as an "invisibility cloak" for specific classes (e.g., Person).
*   **Feature 2: Miscategorization (Label Flipping):** Optimizes a patch to force the detector to misclassify an object (e.g., turning a "Stop Sign" into a "Speed Limit Sign" or a "Dog" into a "Car").
*   **Feature 3: Object Fabrication (Hallucination):** Optimizes a patch to appear as a valid object (e.g., a "Toaster") even when placed in an empty background region.

**Architecture:** Supports switching between backbones (e.g., ResNet50 vs. MobileNetV3) to test transferability and model size effects.

### 🧠 Methodology: Expectation Over Transformation (EOT)
Both scripts utilize EOT to ensure the generated attacks are physically realizable. A static patch optimized for a single digital image often fails in the real world due to viewing angle changes.

**The EOT Loop:**

1.  **Transformation:** In every training step, the patch undergoes random affine transformations (rotation, scaling, shear) and color jittering (brightness/contrast noise).
2.  **Projection:** The transformed patch is overlaid onto the scene.
3.  **Optimization:** Gradients are averaged over these transformations, forcing the patch to learn robust features that survive real-world distortions.

### 🚀 Usage

#### Prerequisites
Install the required dependencies:

```bash
pip install torch torchvision numpy pillow requests matplotlib opencv-python tqdm
```

#### Running the Scripts

**Run the Basic Comparison:**

```bash
python adversarial_detection_demo.py
```
**Output:** Check `attack_results/` for comparative images.

**Run the Advanced Threat Simulation:**

```bash
python advanced_physical_simulation.py
```
**Output:** Check `advanced_results/` for specific attack vector visualizations (Vanishing, Miscategorization, Fabrication).

### 📚 References & Theory
This code is based on concepts from the following literature:

*   **Physical Adversarial Examples for Object Detectors (Eykholt et al., 2018)**
    *   Introduced the "Disappearance Attack" (Vanishing) and "Creation Attack" (Fabrication) using posters on Stop signs.
    *   Source: WOOT '18 Paper

*   **Physically Adversarial Infrared Patches (Wei et al., CVPR 2023)**
    *   Discusses shape and location optimization for patches in thermal/physical environments.
    *   Source: CVF Open Access

*   **Synthesizing Robust Adversarial Examples (Athalye et al., 2017)**
    *   The foundational paper for Expectation Over Transformation (EOT), proving that 3D-printed adversarial objects can exist.
    *   Source: arXiv:1707.07397

*   **ShapeShifter: Robust Physical Adversarial Attack on Faster R-CNN (Chen et al., 2018)**
    *   Specific techniques for attacking Region Proposal Networks (RPN) in two-stage detectors like Faster R-CNN.
    *   Source: arXiv:1804.05810

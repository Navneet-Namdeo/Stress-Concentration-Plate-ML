# Stress Concentration Analysis & ML Surrogate Modeling for Plate with Circular Hole

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Machine%20Learning-F7931E?style=for-the-badge&logo=scikit-learn)
![NumPy](https://img.shields.io/badge/NumPy-Scientific%20Computing-013243?style=for-the-badge&logo=numpy)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557c?style=for-the-badge)

A computational mechanics and machine learning framework that replicates and expands upon the classic **MathWorks MATLAB PDE Toolbox** tutorial: [*Stress Concentration in Plate with Circular Hole*](https://in.mathworks.com/help/pde/ug/stress-concentration-in-plate-with-circular-hole.html).

This project models the theoretical stress distribution using Kirsch's analytical equations for plane stress elasticity and trains a **Random Forest Regression Machine Learning surrogate model** to predict 2D stress fields ($\sigma_{xx}$) efficiently across the spatial domain.

---

## 📌 Repository Description (for GitHub About Section)

> **Short Description:** Python-based simulation and Machine Learning (ML) surrogate model replicating MathWorks' stress concentration in a plate with a circular hole using Kirsch's elasticity formulation and Random Forest Regression.

---

## 📐 Problem Formulation & Governing Physics

Consider an isotropic, linearly elastic rectangular plate of width $W$, length $L$, featuring a central circular hole of radius $R$ subjected to a uniform far-field tensile stress $S_{\text{far}}$ along the x-axis.

Due to the geometric discontinuity created by the hole, the stress field concentrates near the hole edges ($\theta = \pi/2, 3\pi/2$). 

### Kirsch Analytical Solution
Under plane stress assumptions for an infinite plate, the stress distribution in polar coordinates $(r, \theta)$ is given by:

$$ \sigma_r = \frac{S_{\text{far}}}{2} \left(1 - \frac{R^2}{r^2}\right) + \frac{S_{\text{far}}}{2} \left(1 - 4\frac{R^2}{r^2} + 3\frac{R^4}{r^4}\right) \cos(2\theta) $$

$$ \sigma_\theta = \frac{S_{\text{far}}}{2} \left(1 + \frac{R^2}{r^2}\right) - \frac{S_{\text{far}}}{2} \left(1 + 3\frac{R^4}{r^4}\right) \cos(2\theta) $$

$$ \tau_{r\theta} = -\frac{S_{\text{far}}}{2} \left(1 + 2\frac{R^2}{r^2} - 3\frac{R^4}{r^4}\right) \sin(2\theta) $$

Transforming these components to Cartesian coordinates ($\sigma_{xx}$):

$$ \sigma_{xx} = \sigma_r \cos^2\theta + \sigma_\theta \sin^2\theta - 2\tau_{r\theta}\sin\theta\cos\theta $$

### Theoretical Stress Concentration Factor ($K_t$)
At the boundary of the circular hole ($r = R$) at $\theta = \pm \pi/2$:

$$ K_t = \frac{\sigma_{\text{max}}}{S_{\text{far}}} = 3.0 $$

---

## 🛠️ Project Structure

```text
Stress-Concentration-Plate-ML/
│
├── 01_Stress_Concentration_Analysis.ipynb   # Main Jupyter Notebook
├── README.md                                 # Project Documentation
└── requirements.txt                          # Dependencies list
```

---

## 🚀 Workflow Overview

1. **Domain Discretization**: Mesh generation across the 2D spatial domain while removing the inner circular boundary ($r < R$).
2. **Analytical Stress Data Generation**: Evaluation of exact normal stress tensor fields ($\sigma_{xx}$) using Kirsch formulations.
3. **Machine Learning Surrogate Modeling**:
   - **Features ($X$)**: Spatial spatial coordinates $(x, y)$
   - **Target ($y$)**: Computed stress $\sigma_{xx}$
   - **Model**: Random Forest Regressor ($N_{\text{estimators}} = 100$)
4. **Validation**: Performance check using the $R^2$ coefficient of determination and spatial contour plots comparing ML predictions against analytical values.

---

## 📊 Model Performance & Results

- **Data Points Generated**: $\approx 15,000$ spatial coordinate pairs
- **Train / Test Split**: 80% / 20%
- **$R^2$ Score**: `> 0.998` (High fidelity surrogate representation)
- **Peak Predicted Stress**: $\approx 300\text{ MPa}$ ($K_t \approx 3.0$ matching theory)

---

## 💻 Installation & Usage

### 1. Clone the repository
```bash
git clone https://github.com/Navneet-Namdeo/Stress-Concentration-Plate-ML.git
cd Stress-Concentration-Plate-ML
```

### 2. Set up Virtual Environment & Install Dependencies
```bash
python -m venv venv
# On Windows:
venv\Scripts\activate
# On Linux/macOS:
source venv/bin/activate

pip install numpy matplotlib scikit-learn jupyter
```

### 3. Launch Notebook
```bash
jupyter notebook 01_Stress_Concentration_Analysis.ipynb
```

---

## 🔗 References

1. MathWorks PDE Toolbox Documentation: [Stress Concentration in Plate with Circular Hole](https://in.mathworks.com/help/pde/ug/stress-concentration-in-plate-with-circular-hole.html)
2. Timoshenko, S. P., & Goodier, J. N. (1970). *Theory of Elasticity*. McGraw-Hill.
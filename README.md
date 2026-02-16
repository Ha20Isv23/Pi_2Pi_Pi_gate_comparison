# Rydberg Blockade Example Notebook

This repository contains an executed Jupyter notebook demonstrating Rydberg blockade calculations.

---

## Blockade Interaction

The van der Waals interaction between two Rydberg atoms scales as

$$
V(R) = \frac{C_6}{R^6}
$$

where:
- $C_6$ is the van der Waals coefficient
- $R$ is the interatomic distance

---

## Blockade Radius

The blockade radius is defined by the condition

$$
\frac{C_6}{R_b^6} = \hbar \Omega
$$

which gives

$$
R_b = \left(\frac{C_6}{\hbar \Omega}\right)^{1/6}.
$$

---

## How to Run

1. Activate your conda environment:
   ```bash
   conda activate myenv

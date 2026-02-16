# Rydberg Interaction and Blockade Demonstration

This repository contains an executed Jupyter notebook demonstrating
Rydberg–Rydberg interaction physics and the emergence of the blockade regime.

The notebook includes analytical expressions and numerical illustrations
of distance-dependent interaction shifts.

---

## Van der Waals Interaction

In the far off-resonant regime, the interaction between two Rydberg atoms
is well approximated by a van der Waals potential

$$
V(R) = \frac{C_6}{R^6},
$$

where

- $C_6$ is the van der Waals coefficient,
- $R$ is the interatomic separation.

This $R^{-6}$ scaling arises from second-order dipole–dipole coupling.

---

## Dipole–Dipole Interaction

Near a Förster resonance, the interaction can take the resonant dipole–dipole form

$$
V_{\mathrm{dd}}(R) = \frac{C_3}{R^3}.
$$

In this regime, state mixing between pair channels becomes significant,
and the interaction can no longer be described perturbatively.

---

## Blockade Condition

The Rydberg blockade regime occurs when the interaction shift exceeds
the excitation bandwidth:

$$
\frac{C_6}{R_b^6} \approx \hbar \Omega,
$$

where

- $\Omega$ is the Rabi frequency,
- $\hbar$ is the reduced Planck constant.

Solving for the blockade radius gives

$$
R_b = \left( \frac{C_6}{\hbar \Omega} \right)^{1/6}.
$$

Atoms separated by $R < R_b$ cannot be simultaneously excited to the Rydberg state.

---

## Contents

- `notebooks/demo.ipynb` — Executed notebook including analytical derivations and numerical results.

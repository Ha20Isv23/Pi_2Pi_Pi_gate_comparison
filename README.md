# Multi-Channel Rydberg Blockade Gate Simulation  
## Walker–Saffman Effective Shift vs Full Pair-State Diagonalisation

---

## Overview

This repository contains numerical simulations of a two-qubit Rydberg blockade $\pi$ -- $2\pi$ -- $\pi$ entangling gate in neutral atoms.

We compare three treatments of the doubly-excited manifold:

1. **WS(all)** – Walker–Saffman effective blockade shift (channel sum) -> from paper https://journals.aps.org/pra/abstract/10.1103/PhysRevA.77.032723
2. **WS(dom)** – dominant-channel approximation -> same paper, but we only consider the channel with the biggest weight in the sum
3. **Full(2A)** – full diagonalisation of the ARC pair interaction matrix and coherent propagation including all channels

The objective is to determine when a scalar effective blockade model is valid and when multi-channel dipole–dipole mixing (“spaghetti” structure) leads to blockade breakdown.

---

## Gate Protocol

Standard blockade sequence:

$$
\pi \; (\text{control})
\;\rightarrow\;
2\pi \; (\text{target})
\;\rightarrow\;
\pi \; (\text{control})
$$

Laser Rabi frequency: $\Omega$  
Optional single-photon detuning: $\delta$

---

## Pair-State Interaction Model (ARC)

For each interatomic distance $R$, ARC constructs the pair interaction matrix:

$$
m(R) = m_{\text{diag}} + \sum_k \frac{m_{3+k}}{R^{3+k}}.
$$

Diagonalisation yields eigenchannels:

$$
m(R)\,|\nu\rangle = \Delta_\nu(R)\,|\nu\rangle.
$$

The overlap between the addressed bare pair state $|rr\rangle$ and eigenchannels is:

$$
c_\nu = \langle rr | \nu \rangle.
$$

Completeness is verified numerically:

$$
\sum_\nu |c_\nu|^2 \approx 1.
$$

---

## Effective Blockade Models

### 1. Walker–Saffman Effective Shift (WS2008)

The entire doubly-excited manifold is replaced by a single effective blockade shift $B$ defined by:


$$
\frac{1}{B^2}=
\sum_\nu
\frac{|c_\nu|^2}{\Delta_\nu^2}.
$$

This expression assumes perturbative, off-resonant coupling and neglects coherent multi-channel interference.

---

### 2. Dominant-Channel Approximation

Define the dominant channel:

$$
\nu^* = \arg\max_\nu |c_\nu|^2,
$$

and

$$
B_{\text{dom}} = |\Delta_{\nu^*}|.
$$

This approximation is used diagnostically to determine whether the system behaves effectively single-channel.

---

### 3. Full Diagonalisation (2A)

All eigenchannels are retained explicitly in coherent propagation:


$$ H=
H_{\text{laser}}
+
\sum_\nu
\Delta_\nu
|\nu\rangle\langle\nu|.
$$

Laser coupling to each channel scales with $c_\nu$.

No scalar reduction is applied.

---


Four computational basis vectors are propagated simultaneously.

The computational block $M$ (4×4) is extracted from the full propagator.

---

## Fidelity and Leakage

Average survival probability:


$$
P_{\text{surv}}=
\frac{1}{4}
\mathrm{Tr}(M^\dagger M).
$$

Leakage proxy:


$$
\text{Leakage}=
1 - P_{\text{surv}}.
$$

Leakage-aware average gate fidelity:


$$
F_{\text{avg}}=
\frac{
\mathrm{Tr}(M^\dagger M)
+
\left|
\mathrm{Tr}
\left(
U_{CZ}^\dagger
(Z \otimes Z)
U_{\text{eff}}
\right)
\right|^2
}{20}.
$$

Infidelity:

$$
1 - F_{\text{avg}}.
$$

---

## Physical Regimes

### Large $R$ (van der Waals regime)

- Weak dipole–dipole mixing
- Single-channel dominance
- $|c_{\nu^*}|^2 \rightarrow 1$
- All models converge:

$$
\text{WS(all)} \approx \text{WS(dom)} \approx \text{Full}.
$$

---

### Intermediate Regime

- Multiple channels contribute
- Small $|\Delta_\nu|$ begin to appear
- WS(all) sensitive to weakly coupled near-resonant channels
- WS(dom) often tracks Full more closely

---

### Small $R$ (Strong Mixing / “Spaghetti” Regime)

- Dipole–dipole coupling $\sim 1/R^3$ becomes large
- Dense avoided crossings
- Eigenvalue reshuffling
- Small $|\Delta_\nu|$ statistically likely
- Full model predicts blockade breakdown due to coherent multi-channel dynamics
- WS scalar reduction may significantly misestimate fidelity

---

## Interpretation of Model Differences

WS(all) computes a scalar sum:

$$
\sum_\nu
\frac{|c_\nu|^2}{\Delta_\nu^2},
$$

which is always positive and ignores phase information.

Full dynamics evolves coherent amplitudes:

$$
\sum_\nu
c_\nu e^{-i \Delta_\nu t},
$$

allowing interference and dynamical cancellation.

Agreement at large $R$ reflects restoration of perturbative single-channel blockade.



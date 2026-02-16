# Multi-Channel Rydberg Blockade Gate Simulation

### Walker--Saffman Effective Shift vs Full Pair-State Diagonalisation

## Overview

This repository contains numerical simulations of a two-qubit Rydberg
blockade π--2π--π entangling gate in neutral atoms.

The primary objective is to evaluate the validity range of the
**Walker--Saffman (WS2008) effective blockade model** by directly
comparing it against:

-   Full pair-state diagonalisation using ARC\
-   A single-dominant-channel approximation\
-   Complete coherent propagation in the hybrid computational--Rydberg
    basis

The project investigates how multi-channel dipole--dipole mixing
("spaghetti" structure) affects blockade physics and gate fidelity.

------------------------------------------------------------------------

## Physical Model

Two neutral atoms separated by distance R, driven by the standard
sequence:

π (control) → 2π (target) → π (control)

The addressed Rydberg level is configurable (typically nS or nP states).

Pair-state interaction matrices are generated using:

ARC -- Alkali Rydberg Calculator

------------------------------------------------------------------------

## Interaction Hamiltonian

For each interatomic distance R, ARC constructs:

m(R) = m_diag + Σ_k m\_(3+k) / R\^(3+k)

Diagonalisation yields eigenchannels:

m(R)\|ν⟩ = Δ_ν(R)\|ν⟩

The overlap with the addressed pair state is:

c_ν = ⟨rr\|ν⟩

Completeness is verified via:

Σ_ν \|c_ν\|² ≈ 1

------------------------------------------------------------------------

## Effective Blockade Models

### WS2008 Effective Shift

1/B² = Σ_ν \|c_ν\|² / Δ_ν²

This reduces the full doubly-excited manifold to a single effective
detuned state.

This model assumes perturbative, off-resonant coupling.

### Dominant-Channel Approximation

ν\* = argmax \|c_ν\|²\
B_dom = \|Δ_ν\*\|

Used diagnostically to determine if dynamics is effectively
single-channel.

### Full Diagonalisation (2A)

All eigenchannels are retained explicitly:

H = H_laser + Σ_ν Δ_ν \|ν⟩⟨ν\|

Laser coupling to each channel scales with c_ν.

No scalar reduction is applied.

------------------------------------------------------------------------

## Gate Propagation

Time evolution is performed using:

scipy.sparse.linalg.expm_multiply

The computational block M is extracted from the full propagator.

------------------------------------------------------------------------

## Fidelity Definition

Leakage-aware average gate fidelity:

F_avg = \[ Tr(M†M) + \|Tr(U_CZ† (Z⊗Z) U_eff)\|² \] / 20

Infidelity:

1 − F_avg

Leakage proxy:

1 − (1/4) Tr(M†M)

------------------------------------------------------------------------

## Key Physical Regimes

### Large R (vdW regime)

-   Weak dipole-dipole mixing\
-   Single-channel dominance\
-   WS(all) ≈ WS(dom) ≈ Full

### Intermediate Regime

-   Multiple channels contribute\
-   WS(all) sensitive to weakly coupled near-resonant channels\
-   WS(dom) often tracks Full more closely

### Small R (strong mixing / spaghetti regime)

-   Dense avoided crossings\
-   Small eigen-detunings appear\
-   Full model predicts blockade breakdown\
-   WS scalar reduction may misestimate fidelity

------------------------------------------------------------------------

## Scientific Motivation

This project addresses:

-   Validity limits of scalar blockade models\
-   Impact of multi-channel dipole-dipole mixing\
-   Spectral mechanisms behind blockade breakdown\
-   Quantitative fidelity deviations between reduced and full models

------------------------------------------------------------------------

## Requirements

-   Python ≥ 3.9\
-   numpy\
-   scipy\
-   matplotlib\
-   ARC (Alkali Rydberg Calculator)

Install ARC:

pip install ARC-Alkali-Rydberg-Calculator

------------------------------------------------------------------------

## Notes

This model is purely coherent unless additional decoherence mechanisms
are explicitly introduced.

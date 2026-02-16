# Multi-Channel Rydberg Blockade Gate Simulation
## Walker–Saffman Effective Shift vs Full Pair-State Diagonalisation

### Overview
This repository contains numerical simulations of a two-qubit Rydberg blockade π–2π–π entangling gate in neutral atoms.
We compare three models for the doubly-excited manifold:

- **WS(all)**: Walker–Saffman (2008) effective blockade shift using a channel sum
- **WS(dom)**: dominant-channel reduction (diagnostic)
- **Full(2A)**: full diagonalisation of the ARC pair interaction matrix and coherent propagation including all channels

The focus is on identifying when a scalar effective blockade model is valid and when multi-channel (“spaghetti”) physics leads to blockade breakdown.

---

### Gate protocol
Standard blockade CZ protocol:
1) π pulse on the control atom (Ωc=Ω, Ωt=0)
2) 2π pulse on the target atom (Ωc=0, Ωt=Ω)
3) π pulse on the control atom (Ωc=Ω, Ωt=0)

---

### Pair interaction model (ARC)
For each interatomic distance R, ARC provides a truncated pair-state Hamiltonian m(R) (in frequency units).

We diagonalise:
    m(R) |ν> = Δν(R) |ν>

where |ν> are interaction eigenchannels and Δν are channel shifts (converted to Hz in the code).

The addressed bare doubly-excited state is |rr>.
Overlaps:
    cν = <rr|ν>
Completeness sanity check:
    sumν |cν|^2 ≈ 1   (if the full spectrum is retained)

---

### Effective blockade models

**WS(all): Walker–Saffman effective blockade shift**
The doubly-excited manifold is reduced to a single effective detuning B defined by:
    1 / B^2 = sumν ( |cν|^2 / Δν^2 )

Important: this sum is positive term-by-term and is highly sensitive to small |Δν|.

**WS(dom): dominant-channel approximation**
Let ν* = argmaxν |cν|^2.
Then define:
    B_dom = |Δν*|
(Used as a diagnostic for “effectively single-channel” regimes.)

**Full(2A): full eigenchannel propagation**
We keep all channels explicitly with diagonal energies Δν and laser couplings proportional to cν.

---

### Gate propagation basis
Hybrid propagation basis:
- computational: |00>, |01>, |10>, |11>  (4 states)
- single Rydberg: |r0>, |r1>, |0r>, |1r>  (4 states)
- double Rydberg:
  - WS: one effective state (total dimension 9)
  - Full: channel basis {|ν>} (total dimension 8 + Nchannels)

Detuning convention used in code:
- single-excitation manifold: +δ
- double-excitation manifold: +2δ
(δ in Hz; Hamiltonian uses 2π×Hz if expressed in rad/s.)

Time evolution uses scipy.sparse.linalg.expm_multiply on a batched matrix of 4 initial computational vectors.

---

### Fidelity / leakage metrics
We extract the computational block M (4×4) from the full propagator (generally non-unitary due to leakage).

Survival probability (average over computational inputs):
    P_surv = Tr(M† M) / 4

Leakage proxy:
    Leakage = 1 - P_surv

A leakage-aware proxy for average fidelity is computed from M using:
- polar decomposition to get nearest unitary U_eff on the computational subspace
- maximization over local Z phases (analysis-only compensation) to compare to ideal CZ

Infidelity reported:
    1 - F_avg

---

### Physical regimes and expected behavior

**Large R (vdW / weak-mixing regime)**
- Dipole-dipole mixing weak; |rr> overlaps mostly with one eigenchannel:
      |cν*|^2 → 1
- Models converge:
      WS(all) ≈ WS(dom) ≈ Full(2A)

**Small R (strong mixing / “spaghetti” regime)**
- Dipole-dipole couplings scale ~ 1/R^3 and become large
- Dense avoided crossings; eigenvalues reshuffle; small |Δν| become likely
- Full(2A) can predict blockade breakdown due to coherent multi-channel dynamics
- WS(all) may either (i) become overly pessimistic (dominated by weakly coupled small-Δ channels via 1/Δ^2), or (ii) miss coherent multi-channel effects depending on regime
- WS(dom) tracks Full only when dynamics remains effectively single-channel

A useful diagnostic:
    tν = |cν|^2 / Δν^2
    f = max(tν) / sum(tν)
If f≈1 the WS(all) sum is dominated by one term and WS(all)≈WS(dom).

---

### Requirements
- Python ≥ 3.9
- numpy, scipy, matplotlib
- ARC (Alkali Rydberg Calculator)

Install ARC:
    pip install ARC-Alkali-Rydberg-Calculator

---

### Notes
- The baseline model is coherent (unitary) unless additional decoherence is explicitly added.
- ARC basis truncation parameters (n-range, l-range, Δmax, etc.) can strongly affect the channel spectrum in the strong-mixing regime.

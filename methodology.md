# Detailed Methodology — Jawalgaon Dam Safety Analysis

**Reference:** DBA-JWLG-2026  
**Prepared by:** Satwik Kamlakar Udupi  
**Standards:** IS:11223 · IS:5477 (Part-2) · CWC (1994) · NDSA EAP Guidelines

---

## 1. Scope

This document provides the detailed technical methodology for the Jawalgaon Dam Safety Analysis, covering:

1. **Reservoir Routing** — Level Pool Routing of SPF through the reservoir
2. **Breach Parametrization** — Empirical estimation of breach geometry
3. **Breach Discharge Computation** — Time-varying weir flow equations
4. **Downstream Flood Routing** — Wave propagation to downstream sections
5. **SPF Derivation** — CWC Dimensionless Unit Hydrograph procedure
6. **EAC Curve Development** — Prismoidal formula from GIS survey data

---

## 2. Inflow Design Flood

The **Standard Project Flood (SPF)** is adopted as the Inflow Design Flood (IDF) per NDSA guidelines for medium irrigation projects.

### 2.1 SPF Using CWC Dimensionless Unit Hydrograph

**Governing equation (CWC UH scaling):**
```
Q(t) = Qp × f(t/Tp)
```

Where `f(t/Tp)` is the CWC standard dimensionless UH ordinate (CWC, 1994) and:
- `Qp` = Peak discharge of the unit hydrograph = 1,838 m³/s
- `Tp` = Time to peak = 15 hours
- `f(·)` = Dimensionless shape function tabulated per IS:5477 Part-2

---

## 3. Dam Break Analysis

### 3.1 Level Pool Routing

The Modified Puls method transforms the inflow hydrograph `I(t)` into the headwater elevation time series `HW(t)`:

```
(I₁ + I₂)/2 - O₁ = [S₂ - S₁] / Δt

S/Δt + O/2 = (I₁ + I₂)/2 + (S₁/Δt - O₁/2)
```

Storage `S` and outflow `O` are related through the EAC curve and spillway rating.

### 3.2 Breach Parameter Estimation

**Froehlich (2008) — Breach Width:**
```
B_avg = 0.27 × k_o × V_w^0.32 × h_b^0.04
```

**Froehlich (2008) — Formation Time:**
```
t_f = 63.2 × √(V_w / (g × h_b²))
```

**Von Thun & Gillette (1990):**
```
B_avg = 2.5 × h_w + C_b
```

Where:
- `B_avg` = Average breach width (m)
- `V_w` = Reservoir volume at failure (m³)
- `h_b` = Height of breach (m)
- `h_w` = Depth of water above breach bottom (m)
- `k_o` = Overtopping correction factor (1.4 for overtopping, 1.0 for seepage)
- `C_b` = Volume-dependent coefficient (tabulated)

### 3.3 Breach Discharge (Broad-Crested Weir)

```
Q_breach(t) = (2/3) × C_d × L_b(t) × √(2g) × (H_w(t) - Z_b(t))^1.5
```

For SI units with `C_d` ≈ 0.546:
```
Q_breach(t) ≈ 1.7 × L_b(t) × (H_w - Z_b)^1.5
```

The **time-varying breach width** grows linearly during the formation time `t_f`:
```
L_b(t) = B_avg × (t - t_initiation) / t_f    [during breach formation]
L_b(t) = B_avg                                [after full formation]
```

### 3.4 Piping Flow

For the PIPG scenario, a constant piping flow of **7.0 m³/s** is maintained throughout the 72-hour simulation, representing a fully-developed seepage channel prior to catastrophic failure. The assumption is conservative (worst case for EAP planning).

---

## 4. Reservoir Capacity

### 4.1 Prismoidal Formula

Volume between successive contour elevations:
```
V_{n to n+1} = (h/3) × (A_n + A_{n+1} + √(A_n × A_{n+1}))
```

### 4.2 Surface Area

Computed from the volume–elevation relationship:
```
A(z) = dV/dz   [m² or km²]
```

Numerically:
```python
area[i] = (volume[i] - volume[i-1]) / (elevation[i] - elevation[i-1])
```

---

## 5. Plotting Convention

All engineering plots follow the **engineering graph-paper aesthetic**:
- **Background:** Off-white `#F8F9FA` with light grey gridlines
- **Major gridlines:** `rgba(150,150,150,0.55)` at primary tick intervals
- **Minor gridlines:** `rgba(190,190,210,0.35)` at 1/6th of major tick spacing
- **Font:** Courier New / Arial — monospace for axes, serif for annotations
- **Annotations:** CAD-style with white background boxes and thin black borders
- **Color coding:**
  - OVTP traces: `#1565C0` (deep engineering blue)
  - PIPG traces: `#B71C1C` (deep engineering red)
  - CWC UH: `#1B5E20` (deep engineering green)
  - Tailwater: `#E65100` (orange)

---

*End of Methodology Document*

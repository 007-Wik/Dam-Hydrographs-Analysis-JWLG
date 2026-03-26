# Data Dictionary — Jawalgaon Dam Safety Analysis

**Reference:** DBA-JWLG-2026 | Prepared by: Satwik Kamlakar Udupi

---

## JWLG-OVTP Hydrographs.xlsx

| Sheet Name | Rows | Δt | Columns |
|------------|------|----|---------|
| `Breach OVTP` | 51,841 | 5s | Time, Step, HW elevation (m), TW elevation (m), Total Q (m³/s), Weir Q (m³/s), Breach Q (m³/s) |
| `Breach Width` | 51,841 | 5s | Time, Step, Breach Width (m) |
| `Breach Velocity` | 51,841 | 5s | Time, Step, Breach Velocity (m/s) |
| `DS` | 51,841 | 5s | Time, Step, DS Elevation (m), DS Discharge (m³/s) |

**Column Index Map (0-based):**
```python
col_map_breach = {'hw': 2, 'tw': 3, 'total_q': 4, 'weir_q': 5, 'breach_q': 6}
col_map_width  = {'width': 2}
col_map_vel    = {'velocity': 2}
col_map_ds     = {'ds_elev': 2, 'ds_q': 3}
```

---

## JWLG-PIPG Hydrographs.xlsx

| Sheet Name | Rows | Δt | Columns |
|------------|------|----|---------|
| `Brech_Pipg` | 259,201 | 1s | Time, Step, HW elevation (m), TW elevation (m), Total Q (m³/s), Weir Q (m³/s), Breach Q (m³/s) |
| `Width` | 259,201 | 1s | Time, Step, Breach Width (m) |
| `Velocity` | 259,201 | 1s | Time, Step, Breach Velocity (m/s) |
| `DS` | 259,201 | 1s | Time, Step, DS Elevation (m), DS Discharge (m³/s) |

---

## Content Table.xlsx

### Sheet: `Content TABLE`

| Column | Description | Unit |
|--------|-------------|------|
| `Elevation (m)` | Reservoir water surface elevation | m (MSL) |
| `Volume (MCM)` | Cumulative storage volume | Million m³ (MCM) |
| `Remarks` | Critical level label (FRL, MWL, MDDL, etc.) | Text |

### Sheet: `DAM Station ELEV(GIS)`

| Column | Description | Unit |
|--------|-------------|------|
| `Crest Elevation (m)` | Dam crest elevation from GIS survey | m (MSL) |
| Other columns | Station chainage, GIS coordinates | m / degrees |

**Key derived value:**
```python
tbl_elev = df_dam['Crest Elevation (m)'].max()  # Top of Bund Level (TBL)
```

---

## SPF Hydrograph Data (Hard-coded in Notebook)

### OVTP Hydrograph

- **Time:** `range(0, 73)` — 0 to 72 hours, integer hours
- **Flow:** 73-element list, units m³/s, peak = 1,838 m³/s at t = 15h
- **Peak time Tp:** `time_hr[ovtp_q.index(max(ovtp_q))]` = 15 hr

### PIPG Hydrograph

- **Time:** `range(0, 73)` — 0 to 72 hours
- **Flow:** Constant 7.0 m³/s throughout

### CWC Dimensionless UH

- **t/Tp ordinates:** 33 points from 0.0 to 5.0
- **Q/Qp ordinates:** Corresponding dimensionless flows (peak = 1.000 at t/Tp = 1.0)
- **Source:** CWC (1994) Flood Estimation Report, IS:5477 Part-2

---

## Key Simulation Parameters

| Symbol | Value | Description |
|--------|-------|-------------|
| `Qp` | 1,838 m³/s | SPF Peak discharge (OVTP) |
| `Q_pipg` | 7.0 m³/s | PIPG constant seepage flow |
| `Tp` | 15 hr | SPF time to peak |
| `T_sim` | 72 hr | Total simulation duration |
| `Δt_OVTP` | 5 s | OVTP time step |
| `Δt_PIPG` | 1 s | PIPG time step |
| `n_OVTP` | 51,841 | OVTP records |
| `n_PIPG` | 259,201 | PIPG records |
| `BASE` | 2026-01-01 00:00:00 | Simulation epoch (t=0) |

---

*Data dictionary version: DBA-JWLG-2026-v1.0*

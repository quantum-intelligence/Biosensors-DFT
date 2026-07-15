# DFT Study of Biomolecule Adsorption on TMDs using JDFTx

This repository contains the input configurations, structure files, and analysis scripts for density functional theory (DFT) simulations investigating the adsorption of biomolecules on Transition Metal Dichalcogenide (TMD) monolayers.

Calculations were performed using the plane-wave density functional theory code **JDFTx (https://jdftx.org/)**.

\---

## Computational Methodology

### 1\. System Setup \& Parameters

* **Supercell Size:** 6x6 supercell of the TMD monolayer.
* **Exchange-Correlation Functional:** Generalized Gradient Approximation (GGA) in the **PBE** formulation.
* **Van der Waals Correction:** DFT-D3 dispersion correction.
* **Pseudopotentials:** Ultrasoft pseudopotentials.
* **Energy Cutoffs:**

  * Wavefunction cutoff (`elec-cutoff`): **20 Ry**
  * Charge density cutoff: **100 Ry**
* **Smearing:** Gauss smearing with a width of **0.001 Ry**.
* **SCF Convergence Threshold:** 1E-7 Ry.

### 2\. Simulation Workflow

The simulation is split into two steps:

#### Step 1: Ionic Relaxation

To find the stable adsorption configuration, we performed an ionic relaxation of the biomolecule's atomic positions while keeping the TMD lattice substrate fixed.

* **K-point Grid:** 3 3 1 
* **Command used:** `ionic-minimize`
* **Structure Inputs:**

  * `relax.ionpos` (defines initial atomic positions)
  * `relax.lattice` (defines the cell geometry)
* *Note: During relaxation, the optimized coordinates were updated and written back directly into these files.*

#### Step 2:SCF \& DOS Calculation

Once the structures were relaxed, a high-accuracy single-point self-consistent field (SCF) calculation was performed.

* **K-point Grid:** Increased to a denser 6 6 1 grid for accurate electronic structure properties.
* **Modifications:** The `ionic-minimize` command was removed.
* **Density of States (DOS) \& Projected DOS (PDOS):**
Extracted using the following block in the input file (using a smearing of $0.002$ Ry):

&#x20;   ```jdftx
    density-of-states Total \\
                      Orbital Mo 0 d \\
                      Orbital S  0 p \\
                      Orbital C  0 p \\
                      Orbital N  0 p \\
                      Orbital O  0 p \\
                      Orbital H  0 s \\
                      Esigma 0.002
    ```

\---

## Outputs and Data Extraction

We extracted the electrostatic potential, charge density, and key energy metrics using the following command:

```jdftx
dump End ElecDensity Ecomponents EigStats Fillings Dtot
```

### Output File Descriptions

* **`ElecDensity`**: Contains the electron density distribution.
* **`Ecomponents`**: Breakdown of the individual energy components of the system.
* **`EigStats`**: Contains eigenvalue statistics (Fermi level, HOMO, LUMO, etc.).
* **`Fillings`**: Occupations of the electronic states.
* **`Dtot`**: The total electrostatic potential (used to compute work functions and potential profiles).

> \*\*Units Note:\*\* In JDFTx, all lengths are reported in \*\*Bohr\*\* and all energy components are in \*\*Rydbergs\*\*.

\---

&#x20;   ```


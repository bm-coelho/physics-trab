---
layout: default
---

<script type="text/javascript" async
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/3.2.2/es5/tex-mml-chtml.js">
</script>



# 2D Eulerian Fluid Simulation

![Fluid Flow Simulation](/physics-trab/assets/evolution.gif)

This project simulates the flow of a two-dimensional incompressible fluid using the Eulerian approach and finite difference methods. The simulation is based on the Navier-Stokes equations, which describe the motion of fluid substances. For this demonstration, we assume:

1. The fluid is Newtonian, meaning viscosity is constant.
2. The fluid is incompressible, so density remains constant.
3. The fluid is isothermal, making temperature irrelevant.
4. No external forces are applied.
5. The system is two-dimensional.
6. The fluid is contained in a box where all walls have no velocity, except for the top wall, which moves.

> 🌎 Este documento também está disponível em [Português](/physics-trab/docs/README.pt.md).

---

## Features
- Numerical solution of the Navier-Stokes equations for 2D incompressible fluid flow.
- Explicit time-stepping scheme for temporal evolution.
- Finite difference methods for spatial discretization.
- Gauss-Seidel iterative solver for the pressure-Poisson equation.
- Visualization of velocity fields and flow dynamics.

---

## How It Works

### Governing Equations

The fluid's behavior is governed by the Navier-Stokes equations:

1. **Conservation of Momentum**:
   $$
   \rho \frac{\partial \vec{u}}{\partial t} = -\nabla p + \mu \nabla^2 \vec{u} + \vec{F},
   $$
   which accounts for the rate of change of momentum due to pressure, viscous forces, and external forces.

2. **Continuity Equation**:
   $$
   \nabla \cdot \vec{u} = 0,
   $$
   which enforces the incompressibility condition.

For two-dimensional flow with no external force, the Navier-Stokes equations expand into:

1. **$x$-momentum equation**:
   $$
   \frac{\partial u}{\partial t} 
   + u \frac{\partial u}{\partial x} 
   + v \frac{\partial u}{\partial y} 
   = 
   -\frac{1}{\rho}\frac{\partial p}{\partial x} 
   + \frac{1}{\rho} \nu \left( \frac{\partial^2 u}{\partial x^2}  + \frac{\partial^2 u}{\partial y^2} \right).
   $$

2. **$y$-momentum equation**:
   $$
   \frac{\partial v}{\partial t} 
   + u \frac{\partial v}{\partial x} 
   + v \frac{\partial v}{\partial y} 
   = 
   -\frac{1}{\rho}\frac{\partial p}{\partial y} 
   + \frac{1}{\rho}\nu \left( \frac{\partial^2 v}{\partial x^2} + \frac{\partial^2 v}{\partial y^2} \right).
   $$

3. **Continuity equation**:
   $$
   \frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} = 0.
   $$

---

### Variables and Parameters
| Variable  | Description                                              |
|-----------|----------------------------------------------------------|
| $\rho$    | Fluid density, measuring the mass per unit volume.       |
| $\vec{u}$ | Velocity vector $(u, v)$, where $u$ and $v$ are the $x$- and $y$-velocity components. |
| $p$       | Pressure field, representing compressive forces.         |
| $\mu$     | Dynamic viscosity, quantifying resistance to deformation.|
| $\nu$     | Kinematic viscosity, $\nu = \frac{\mu}{\rho}$.           |
| $\vec{F}$ | External forces, such as gravity.                        |

---

### Simulation Outline

1. **Initialization**:
   - Set up the initial velocity and pressure fields.
2. **Boundary Conditions**:
   - Apply boundary conditions for velocity and pressure at the walls.
3. **Momentum Equations**:
   - Compute intermediate velocity fields without considering pressure contributions.
4. **Pressure Correction**:
   - Solve the pressure-Poisson equation to ensure incompressibility.
   - Update velocity fields using the pressure gradient.
5. **Iterate**:
   - Advance the simulation by repeating the above steps for the desired number of time steps.

---

## Numerical Methods

### Spatial Discretization

Finite difference methods are used to discretize the governing equations:

- **First derivatives** (e.g., convective terms):
  $$
  \frac{\partial u}{\partial x} \approx \frac{u_{i+1,j} - u_{i-1,j}}{2 \Delta x}.
  $$

- **Second derivatives** (e.g., diffusive terms):
  $$
  \frac{\partial^2 u}{\partial x^2} \approx \frac{u_{i+1,j} - 2u_{i,j} + u_{i-1,j}}{\Delta x^2}.
  $$

- **Pressure-Poisson**:
  solved iteratively using the Gauss-Seidel method.

### Time Integration

The temporal evolution is calculated using explicit time-stepping, with the time step size ($\Delta t$) determined by:

1. **CFL condition**:
   $$
   \Delta t < \frac{\Delta x}{\text{max}(|u|)}.
   $$

2. **Viscous stability constraint**:
   $$
   \Delta t < \frac{\rho \Delta x^2}{\mu}.
   $$

The smaller of the two is chosen to ensure stability.

---

## Visualization

### Example Outputs

1. **Velocity Field**:

<img src="/physics-trab/assets/velocity_field.png" alt="Velocity Field" width="600" />

2. **Streamlines**:

<img src="/physics-trab/assets/streamlines.png" alt="Streamlines" width="600" />

3. **Pressure Distribution**:

<img src="/physics-trab/assets/pressure_contours.png" alt="Pressure Field" width="600" />

---

## How to Run

### Prerequisites
- Python 3.8+ with the following libraries:
  - `numpy`
  - `matplotlib`
  - `tqdm`
  - `Pillow`

### Instructions
1. Clone the repository:
   ```bash
   git clone https://github.com/your-repo/fluid-simulation.git
   cd fluid-simulation
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
   
3. Open the Jupyter Notebook:
   ```bash
   jupyter notebook main.ipynb
   ```

4. Run all cells in the notebook to execute the simulation and generate visualizations.

---

## References

1. Bitiușcă, L.-G. (2016). *Eulerian Fluid Simulator*. MSc thesis, Bournemouth University.
2. Saad, M. (2024 & 2019). *Computational Fluid Dynamics Lessons*. University of Utah. Available at [YouTube Playlist](https://www.youtube.com/playlist?list=PLEaLl6Sf-KICvBLrYFwt5h_LgedJyN59n).
3. Bridson, R. (2015). *Fluid Simulation for Computer Graphics*. CRC Press.


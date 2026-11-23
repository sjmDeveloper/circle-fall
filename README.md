# Falling Ball Fluid Simulation

## 1. About the Project & Motivation
* **Idea:** A real-time 2D physics simulation of falling spheres in a viscous fluid (air or liquid), accounting for gravity, fluid drag (Stokes' law), and buoyancy.
* **Motivation:** Built to translate analytical mechanics and differential equations into a clean, visual C++ & SFML simulation.

## 2. Mathematics & Physics Formulation
Applying Newton's Second Law of Motion along the downward vertical Y-axis:

$$\sum \vec{F}_{\text{ext}} = m \cdot \vec{a} \implies P - f - \Pi = m \cdot a$$

Where the acting forces are defined as:
* **Gravity ($P$):** $P = m \cdot g$
* **Drag Force ($f$):** $f = k \cdot v$ (Stokes' drag for laminar flow)
* **Buoyancy ($\Pi$):** $\Pi = \rho_f \cdot V_f \cdot g$

Substituting the forces and setting $a = \frac{dv}{dt}$ yields a first-order linear differential equation:

$$\frac{dv}{dt} + \frac{k}{m} v = g \left(1 - \frac{\rho_f \cdot V_f}{m}\right)$$

Solving this differential equation gives the instantaneous velocity $v(t)$:

$$v(t) = v_{\text{lim}} \left(1 - e^{-\frac{t}{\tau}}\right)$$

Where:
* **Terminal Velocity ($v_{\text{lim}}$):** $v_{\text{lim}} = \frac{m \cdot g}{k} \left(1 - \frac{\rho_f \cdot V_f}{m}\right)$
* **Characteristic Time ($\tau$):** $\tau = \frac{m}{k}$
* **Drag Coefficient ($k$):** $k = 6 \pi \eta r$
* **Mass ($m$):** $m = \rho \cdot V$

## 3. Software Architecture (C++ & SFML)

### `Circle` Class
* Encapsulates the sphere's **radius** (`r`) and rendering resolution (`numOfPoints = 30`).
* Implements physical property methods: `Circle::m()`, `Circle::k()`, `Circle::tau()`, and `Circle::v_lim()`.
* Computes instantaneous velocity dynamically via `Circle::v()` using the analytical exponential formula.

### Simulation State & Loop
* `t` (`sf::Clock`): Tracks elapsed simulation time.
* `circles` (`sf::CircleShape[999]`): Fixed-size array storing up to 999 active spheres.
* `sizeOfCircles` (`size_t`): Tracks the count of spawned spheres.
* **Two-Phase Execution:**
  1. **Spawning Phase:** Prompts the user via console to enter radius values (`r`). Entering `0` terminates input and launches the physics loop.
  2. **Render Loop:** Updates each sphere's vertical displacement using `circle.v()` and renders frames in real time.

## 4. Prerequisites & Installing SFML
Before compiling the project, ensure you have **SFML** and a C++ compiler (`g++`) installed:

### Linux
*(Watch step-by-step video guide: [How to install SFML on Linux](https://www.youtube.com/watch?v=Z7CAWvQN8oY))*

* **Ubuntu / Debian / Mint:**
  ```bash
  sudo apt-get update && sudo apt-get install libsfml-dev g++
  ```
* **Fedora / RHEL / CentOS:**
  ```bash
  sudo dnf install SFML-devel gcc-c++
  ```
* **Arch Linux / Manjaro:**
  ```bash
  sudo pacman -S sfml gcc
  ```
* **openSUSE:**
  ```bash
  sudo zypper install sfml2-devel gcc-c++
  ```
* **Alpine Linux:**
  ```bash
  sudo apk add sfml-dev g++
  ```
* **Other Distributions:** Download the source or SDK from the [Official SFML Download Page](https://sfml-dev.org).

### Windows
Download the SDK from the [Official SFML Download Page](https://sfml-dev.org) and link it to your compiler (Visual Studio / MinGW).
*(Watch step-by-step video guide: [How to Setup SFML on Windows](https://www.youtube.com/watch?v=qvg8BXXWpCE))*

## 5. Build & Run

### 1. Clone & Compile
Open your Terminal (Linux) or Command Prompt (Windows) and run:
```bash
git clone https://github.com/sjmDeveloper/circle-fall
cd circle-fall
g++ main.cpp -o physics_sim -lsfml-graphics -lsfml-window -lsfml-system
```

### 2. Run the Simulation
* **On Linux:**
  ```bash
  ./physics_sim
  ```
* **On Windows (CMD):**
  ```cmd
  physics_sim.exe
  ```

## 6. Version
`1.0.0`

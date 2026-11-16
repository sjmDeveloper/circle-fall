# 1. Idea
A real-life physics simulation of a falling ball in a fluid (air/liquid) accounting for gravity, air resistance (drag), and buoyancy.

# 2. Why I Made This
I wanted to apply the mathematics and physics concepts I studied in high school to build a realistic visual simulation.

# 3. Mathematics & Physics Side
By applying Newton's Second Law of Motion:
$$\sum \vec{F}_{\text{ext}} = m \cdot \vec{a}$$
$$\vec{P} + \vec{f} + \vec{\Pi} = m \cdot \vec{a}$$

Projecting onto the vertical Y-axis (downward positive):
$$P - f - \Pi = m \cdot a$$

Where the forces are defined as:
* **Gravity ($P$):** $P = m \cdot g$
* **Drag force ($f$):** $f = k \cdot v$
* **Buoyancy ($\Pi$):** $\Pi = \rho_f \cdot V_f \cdot g$

Substituting the forces into the equation:
$$m \cdot g - k \cdot v - \rho_f \cdot V_f \cdot g = m \cdot a$$

Dividing the entire equation by mass ($m$) and substituting $a = \frac{dv}{dt}$:
$$g - \frac{\rho_f \cdot V_f \cdot g}{m} - \frac{k}{m} v = \frac{dv}{dt}$$

Rearranging into a first-order linear differential equation:
$$\frac{dv}{dt} + \frac{k}{m} v = g \left(1 - \frac{\rho_f \cdot V_f}{m}\right)$$

The analytical solution for the velocity $v(t)$ is:
$$v(t) = v_{\text{lim}} \left(1 - e^{-\frac{t}{\tau}}\right)$$

Where:
* **Characteristic time ($\tau$):** $\tau = \frac{m}{k}$
* **Drag coefficient ($k$):** $k = 6 \pi \eta r$ (Stokes' law)
* **Mass ($m$):** $m = \rho \cdot V$

In the code, this function calculates and returns $v(t)$ dynamically using $r$, $m$, and $k$.

# 4. Programming Side (C++ & SFML)

## `Circle` Class
* Defined by its **radius** (`r`) and **point count** (`numOfPoints = 30`).
* Includes member methods to calculate physical properties: `Circle::m()`, `Circle::tau()` ($\tau$), `Circle::k()`, and terminal velocity `Circle::v_lim()`.
* Computes instantaneous velocity using: `Circle::v() = Circle::v_lim() * (1 - exp(-t / Circle::tau()))`.

## Main Loop Variables
* `t` (`sf::Clock`): Measures the elapsed time since the start of the simulation.
* `circles` (`sf::CircleShape[999]`): An array storing up to 999 circle objects.
* `sizeOfCircles` (`size_t`): Tracks the active number of circles.

## User Input & Simulation Loop
* **Input:** Prompts the user to enter a radius `r` (`float`). Entering a non-zero value creates a new circle with radius `r`. Entering `0` stops the input phase and starts the simulation.
* **While Loop:** Iterates through all active circles, updates their vertical positions using `circle.v()`, and renders them on the window frame by frame.

# 5. How It Works
1. Enter radius values (`r`) one by one to spawn balls of different sizes.
2. Enter `0` to finish adding balls and launch the real-time falling simulation (note: inputting values pauses spawning until `0` is entered, after which the simulation runs at full speed).
   
# 6. Prerequisites & Installing SFML
Before compiling the project, make sure you have **SFML** installed on your system:

* **On Linux (Ubuntu/Debian):** Install directly via terminal:
  ```bash
  sudo apt-get update && sudo apt-get install libsfml-dev
  ```
  *(Watch step-by-step video guide: [How to install SFML on Linux](https://www.youtube.com/watch?v=Z7CAWvQN8oY))*

* **On Windows:** Download the SDK from the [Official SFML Download Page](https://sfml-dev.org) and link it to your compiler.
  *(Watch step-by-step video guide: [How to Setup SFML on Windows](https://www.youtube.com/watch?v=qvg8BXXWpCE))*

# 7. How to Run
1. Open your Terminal (Linux) or CMD (Windows) and navigate to the project folder:
```bash
cd your_file_path
```

2. Compile the code with SFML libraries linked:
```bash
g++ main.cpp -o physics_sim -lsfml-graphics -lsfml-window -lsfml-system
```


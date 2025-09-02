---
title: 'Simulation of Wave Functions in Quantum Mechanics'
published: 2025-08-02
updated: 2025-09-02
description: 'Project of Cambridge Summer School'
image: 'https://image.haoye.plus/QMVideo/2D.png'
tags: [Physics, Summer School]
category: 'Physics'
draft: false 
lang: 'en'
---

# Simulation of Wave Functions in Quantum Mechanics

(Cambridge Summer School's Presentation)

written in jupyter notebook by Yiran Feng (Leon) 2025-08-02


```python
Step = QMSim
Step.Gaussian = [-5, 0.5, 4]
Step.Potential = np.array([0] * 120 + [20] * 80)
%manim -qh Step
```

<video src="https://image.haoye.plus/QMVideo/wall.mp4" controls autoplay loop style="max-width: 100%;">
      Your browser does not support the <code>video</code> element.
    </video>

---

## Introduction

In chapter 2 and chapter 3, we've learnt about wave function in quantum mechanics and its characteristics, however, in a rather mathematical level, with equations and deductions. In this presentation, I'll present you a python simulation of wave functions in different situations, giving you a intuitive way of understanding quantum mechanics. 

For simplicity, all constants are set to one in this simulation, including mass, planck's constant, etc. 


```python
# Libraries
import numpy as np
import scipy as sp
from matplotlib import pyplot as plt
from manim import *
from IPython.display import HTML
from copy import deepcopy as dc
from random import random
import tqdm

# Variables & Constants
inf = 100
x = np.linspace(-10, 10, 200)
y = np.linspace(-10, 10, 200)
X, Y = np.meshgrid(x, y)

# Useful functions
def find_nearest(array, value):
        array = np.asarray(array)
        idx = (np.abs(array - value)).argmin()
        return idx
    
def find_nearest_val(array, value):
        array = np.asarray(array)
        idx = (np.abs(array - value)).argmin()
        return array[idx]
```

---

## Simulation on 1D

Basically, the simulation consists of two parts: Initialization and Evolution. 

### Initialization

The state of the system can be initialized with two functions: **the initial state of the wave function** and **the potential function**.  

By considering different **initial states** of the wave function, we can change the position, the spread, and the momentum of the particle. 

By changing the **potential function**, we can simulate different situations, for example, harmonic oscillator and quantum channel. 

Look into the code. The class `Simulation1D` takes in two variables. `phi_0` means the initial state of the wave function, and `v` defines the potential. The wave function is automatically normalised by the function `_normalise`, which is called inside the initialization, and `probability`  ( which is basically $|\phi(x)|^2$ ) gives us the probability, of course. 


```python
class Simulation1D():
    def __init__(self, phi_0, v):
        self.phi = dc(phi_0); self.v = dc(v)
        self.dx = x[1] - x[0]; self.len = len(phi_0)
        self._normalise(); self._hamiltonian()
        self.prob = []; self.prob_updated = False
        
    def _normalise(self):
        self.phi /= np.linalg.norm(self.phi)
        self.prob_updated = True
        self.prob = (self.phi * self.phi.conjugate()).real
        
    def probability(self, pos = None):
        if not self.prob_updated:
            self._normalise()
        if pos:
            return self.prob[find_nearest(x, pos)]
        else:
            return self.prob
```

### Evolution

 From chapter 2 and chapter 3, we've learnt that when a system is free from interactions with measuring devices, the evolution of the wave function can be calculated by the **Schrödinger equation** (44) (Postulate 6)

$$
i\hbar \frac{\partial ψ(x,t)}{\partial t} = \left [ -\frac{\hbar ^ 2}{2m} \frac{\partial^2}{\partial x^2} + V(x) \right ] ψ(x,t)
$$

In our cases, the mass and $\hbar$ is set to 1. Introducing the hamiltonian operator $\hat H$, the **1-dimensonal time-dependent Schrödinger equation (TDSE)** can be deduced

$$
\frac{\partial}{\partial t} ψ(x, t) = -i\hat H ψ(x, t)
$$

where

$$
\hat H = -\frac{1}{2} \nabla ^2 + V(x)
$$

In this equation, the discrete Laplace operator can be given as convolution with **the finite-difference kernel**, $[1, -2, 1]$, results in a symmetric **tridiagonal Toeplitz matrix** with the finite-difference coefficients along the diagonals. Combining this matrix with the potential $V=[V_0, V_1, ..., V_N]$, a full **Hamiltonian operator** in matrix form can be obtained

$$
\hat H = -\frac{1}{2\Delta x^2}
\begin{bmatrix}
-2 & 1 & \quad & \quad & \quad \\
1 & -2 & 1 & \quad & \quad \\
\quad & \quad & \ddots & \quad & \quad \\
\quad & \quad & 1 & -2 & 1 \\
\quad & \quad & \quad & 1 & -2 \\
\end{bmatrix}
+
\begin{bmatrix}
V_0 & \quad & \quad & \quad & \quad \\
\quad & V_1 & \quad & \quad & \quad \\
\quad & \quad & \ddots & \quad & \quad \\
\quad & \quad & \quad & V_{N-1} & \quad \\
\quad & \quad & \quad & \quad & V_N \\
\end{bmatrix}
$$


```python
def _hamiltonian(self):
    """Returns Hamiltonian using finite differences."""
    L = [[0 for j in range(self.len)] for i in range(self.len)]
    for i in range(self.len):
        for j in range(self.len):
            if i == j:
                L[i][j] = -2
            if abs(i-j) == 1:
                L[i][j] = 1
    
    # Somehow it doesn't work...
    # L = sp.sparse.diags([1, -2, 1], offsets=[-1, 0, 1], shape=(self.len, self.len))
    
    L = np.array(L) / self.dx ** 2
    H = -L / 2 + sp.sparse.spdiags(self.v, 0, self.len, self.len)
    self.H = sp.sparse.csc_matrix(H)
    
Simulation1D._hamiltonian = _hamiltonian
```

If the Hamiltonian is independent of time, a **time evolution operator**(U) can be obtainted

$$
ψ(t+\delta t) = Uψ(t)
$$

where

$$
U = e^{-i\hat H \delta t}
$$

Multiplying the wave function with the operator $m$ times, we can advance the time steps to $t + m\delta t$

$$
ψ(t+m\delta t) = U^mψ(t)
$$

The code below calculates the time evolution operator based on the hamiltonian and `dt`, and applies the operator to the wave function, which is also normalized after the procedure. 


```python
def simulate(self, dt = 1e-3):
    """Generates wavefunction and time at the next time step."""
    U = sp.sparse.linalg.expm(-1j * self.H * dt)
    #U[1E-10 > (U.real**2 + U.imag**2)] = 0
    self.phi = U @ self.phi
    self.phi[self.v > inf-1] = 0
    self._normalise()
    
Simulation1D.simulate = simulate
```

### Wave Function Representation

We use a localized Gaussian wave packet centered at $x_0$ with average initial momentum, $p_0$ , and Gaussian width, $\sigma_0$ . The wave function is given by

$$
ψ(x, 0) = \left ( \frac{1}{2 \pi \sigma_0^2 } \right ) ^ \frac{1}{4} \mathrm {exp} \left ( -\frac{(x-x_0)^2}{4\sigma_0^2} + i p_0 x\right )
$$


```python
def gaussian_wavepacket(x0, sigma, p0):
    """Gaussian wavepacket at x0 +/- sigma, with average momentum, p0."""
    global x
    return np.exp(1j*p0*x - ((x - x0)/(2 * sigma))**2)
```

### Simulation

After the calculations, some simulations can be done. 

The class `QSim` takes in the parameters of the gaussian wavepacket and the potential function. It produces an animation of the evolution of the wave function in 2 seconds using `manim`.


```python
class QMSim(Scene):
    def construct(self):
        phi_0 = gaussian_wavepacket(self.Gaussian[0], self.Gaussian[1], self.Gaussian[2])
        V = self.Potential
        o = Simulation1D(phi_0, V)
        
        axes = Axes(
            x_range=[-10, 10, 1],
            y_range=[0, 0.17, 1],
            x_length=10,
            axis_config={"color": GREEN},
            x_axis_config={
                "numbers_to_include": np.arange(-10, 10.01, 2),
                "numbers_with_elongated_ticks": np.arange(-10, 10.01, 2),
            },
            tips=True,
        )
        
        axes_labels = axes.get_axis_labels()
        axes_ref = axes.copy()
        
        pot_graph = axes.plot((lambda k: (V/100)[find_nearest(x,k)]), color=BLUE)
        pot_label = axes.get_graph_label(
            pot_graph, "V(x)", x_val=10, direction=UP / 2
        )
        
        prob_graph = axes.plot(o.probability, color=ORANGE)
        prob_label = axes.get_graph_label(
            prob_graph, r"|\phi(x)|^2", x_val=-10, direction=UP / 2
        )

        plot = VGroup(axes, pot_graph, prob_graph)
        labels = VGroup(axes_labels, prob_label, pot_label)

        def prob_updater(mobj, dt):
            o.simulate(dt=dt*2)
            mobj.become(axes.plot(o.probability, color=ORANGE))
            return mobj
            

        prob_graph.add_updater(prob_updater)
        prob_label.add_updater(lambda x:x.become(axes.get_graph_label(prob_graph, r"|\phi(x)|^2", x_val=-10, direction=UP / 2)))
        self.add(plot, labels)
        self.wait(2 + random()) # force update
```

**Harmonic Oscillator**


```python
Oscillator = QMSim
Oscillator.Gaussian = [-5, 0.5, 0.1]
Oscillator.Potential = x*x
%manim -qh Oscillator
```

<video src="https://image.haoye.plus/QMVideo/oscillator.mp4" controls autoplay loop style="max-width: 100%;">
      Your browser does not support the <code>video</code> element.
    </video>

**Potential Step**


```python
Step = QMSim
Step.Gaussian = [-5, 0.5, 4]
Step.Potential = np.array([0] * 140 + [7] * 60)
%manim -qh Step
```

<video src="https://image.haoye.plus/QMVideo/step.mp4" controls autoplay loop style="max-width: 100%;">
      Your browser does not support the <code>video</code> element.
    </video>

**Quantum Channel**


```python
Channel = QMSim
Channel.Gaussian = [-5, 0.5, 4]
Channel.Potential = np.array([0] * 100 + [6] * 10 + [0] * 90)
%manim -qh Channel
```

<video src="https://image.haoye.plus/QMVideo/channel.mp4" controls autoplay loop style="max-width: 100%;">
      Your browser does not support the <code>video</code> element.
    </video>

The real and imagery part of the wave function can also be illustrated


```python
class QMSim2(Scene):
    def construct(self):
        phi_0 = gaussian_wavepacket(-5, 0.4, 1.4)
        V = np.array([0] * 200)
        o = Simulation1D(phi_0, V)
        
        axes = Axes(
            x_range=[-10, 10, 1],
            y_range=[-0.3, 0.3, 1],
            x_length=10,
            axis_config={"color": GREEN},
            x_axis_config={
                "numbers_to_include": np.arange(-10, 10.01, 2),
                "numbers_with_elongated_ticks": np.arange(-10, 10.01, 2),
            },
            tips=True,
        )
        
        axes_labels = axes.get_axis_labels()
        axes_ref = axes.copy()
        
        real_graph = axes.plot(lambda x:find_nearest_val(o.phi.real,x), color=BLUE)
        real_label = axes.get_graph_label(
            real_graph, r"Re{\phi(x)}", x_val=-10, direction=UP / 2
        )
        
        imag_graph = axes.plot(lambda x:find_nearest_val(o.phi.imag,x), color=YELLOW)
        imag_label = axes.get_graph_label(
            imag_graph, r"Im{\phi(x)}", x_val=10, direction=UP / 2
        )

        plot = VGroup(axes, real_graph, imag_graph)
        labels = VGroup(axes_labels, real_label, imag_label)

        def real_updater(mobj, dt):
            o.simulate(dt=dt*2)
            mobj.become(axes.plot(lambda k:(o.phi.real)[find_nearest(x,k)], color=BLUE))
            return mobj

        def imag_updater(mobj, dt):
            mobj.become(axes.plot(lambda k:(o.phi.imag)[find_nearest(x,k)], color=YELLOW))
            return mobj
            

        real_graph.add_updater(real_updater)
        imag_graph.add_updater(imag_updater)
        real_label.add_updater(lambda x:x.become(axes.get_graph_label(real_graph, r"Re{\phi(x)}", x_val=-10, direction=UP / 2)))
        imag_label.add_updater(lambda x:x.become(axes.get_graph_label(imag_graph, r"Im{\phi(x)}", x_val=10, direction=UP / 2)))
        
        self.add(plot, labels)
        self.wait(2 + random()) # force update
        
%manim -qh QMSim2
```

<video src="https://image.haoye.plus/QMVideo/realimag.mp4" controls autoplay loop style="max-width: 100%;">
      Your browser does not support the <code>video</code> element.
    </video>

---

## Simulation on 2D

Applying the Schrödinger equation in 2D, we can simulate how a 2D wave function evolves. 

However, the calculation for 2D simulation is too complicated for my laptop to run an animation of it. Hence merely an image of the initial state of a 2D wave function is obtained in a relatively low resolution.  


```python
# Constants
x = np.linspace(-10, 10, 100)
y = np.linspace(-10, 10, 100)
X, Y = np.meshgrid(x, y)
```


```python
class Simulation2D(): # For simplicity, some code's folded up
    def __init__(self, phi_0, v):
        self.phi = dc(phi_0); self.v = dc(v)
        self.dx = x[1] - x[0]; self.dy = y[1] - y[0]
        self.lenx = len(phi_0[0]); self.leny = len(phi_0)
        self._normalise(); self._hamiltonian()
        
    def _normalise(self):
        self.phi /= np.linalg.norm(self.phi)

    def _hamiltonian(self):
        """Returns Hamiltonian using finite differences."""
        L1 = [[0 for j in range(self.lenx)] for i in range(self.lenx)]
        for i in range(self.lenx):
            for j in range(self.lenx):
                if i == j:
                    L1[i][j] = -2
                if abs(i-j) == 1:
                    L1[i][j] = 1
                    
        L2 = [[0 for j in range(self.leny)] for i in range(self.leny)]
        for i in range(self.leny):
            for j in range(self.leny):
                if i == j:
                    L2[i][j] = -2
                if abs(i-j) == 1:
                    L2[i][j] = 1

        # 2D Laplacian using Kronecker products
        L1 = np.array(L1) / self.dx ** 2; L2 = np.array(L2) / self.dy ** 2
        I_x = sp.sparse.identity(self.lenx); I_y = sp.sparse.identity(self.leny)
        L2D = sp.sparse.kron(I_y, L1) + sp.sparse.kron(L2, I_x)
        H = (-L2D / 2).toarray()
        for i in range(len(self.v)): # Somehow sp.sparse.diags doesn't work qaq
            H[i+i*self.lenx] = self.v[i//self.lenx][i%self.lenx]
            
        self.H = sp.sparse.csr_matrix(H)
        
    def simulate(self, dt = 1e-3):
        """Generates wavefunction and time at the next time step."""
        U = sp.linalg.expm(-1j * self.H.toarray() * dt)
        U[(U.real**2 + U.imag**2) < 1E-10] = 0
        phi_flat = self.phi.flatten()
        phi_flat = U @ phi_flat
        self.phi = phi_flat.reshape((self.lenx, self.leny))
        self.phi[self.v > inf-1] = 0

    def probability(self):
        return (self.phi * self.phi.conjugate()).real
```


```python
def gaussian_wavepacket_2D(x0, y0, sigma, p0):
    """Gaussian wavepacket at x0 +/- sigma, y0 +/- sigma with average momentum, p0."""
    global x, y
    return np.exp(1j*p0*x - (((X - x0)**2+(Y - y0)**2)/(2 * sigma**2)))
```


```python
def Free2D():
    phi_0 = gaussian_wavepacket_2D(3, 0, 0.5, -1)
    V = X*X+Y*Y
    o = Simulation2D(phi_0, V)
    for i in tqdm.trange(0):
        o.simulate(0.5)

    from matplotlib import cm
    from matplotlib.ticker import LinearLocator
    
    fig, ax = plt.subplots(subplot_kw={"projection": "3d"})
    
    
    # Plot the surface.
    surf = ax.plot_surface(X, Y, o.probability(), cmap=cm.coolwarm,
                           linewidth=0, antialiased=False)
    
    # Customize the z axis.
    #ax.set_zlim(-1.01, 1.01)
    ax.zaxis.set_major_locator(LinearLocator(10))
    # A StrMethodFormatter is used automatically
    ax.zaxis.set_major_formatter('{x:.02f}')
    
    # Add a color bar which maps values to colors.
    fig.colorbar(surf, shrink=0.5, aspect=5)
    
    plt.show()
    
Free2D()
```
    
![png](https://image.haoye.plus/QMVideo/2D.png)

## Prospect

Some improvements could also be made regarding this simulation and animation ...

- Record physical quantities (momentum, energy, ...) during the simulation
- Increase the resolution for a higher accuracy
- Complete simulations in 2D
- ...

Thanks for listening ~

## Reference


> https://www.astro.utoronto.ca/~mahajan/notebooks/quantum_tunnelling.html
> 
> https://docs.manim.community/

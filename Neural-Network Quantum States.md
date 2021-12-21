# Neural-Network Quantum States
#### [bens-planet.com](https://bens-planet.com/neural-network-quantum-states/) ▪ Friday, December 17, 2021, 2:34 PM

**Notes on a talk by Guiseppe Carleo given at the** [**SCQM 2020**](https://www.pks.mpg.de/scqm20/) **winter school.**

Describing a single spin-$\\frac{1}{2}$ particle can be achieved by the simple ansatz

$$| \\psi \\rangle = C\_{\\uparrow} |\\uparrow\\rangle + C\_{\\downarrow} | \\downarrow\\rangle

$$

and the normalization condition $|C\_{\\uparrow}|^2 + |C\_{\\downarrow}|^2 = 1$. While this is simple, for $N$ spin-$\\frac{1}{2}$ particles the ansatz gets more elaborate

$$\\begin{aligned}| \\psi \\rangle & = C\_{\\uparrow \\uparrow \\dots \\uparrow} |\\uparrow \\uparrow \\dots \\uparrow\\rangle + C\_{\\uparrow \\uparrow \\dots \\downarrow} |\\uparrow \\uparrow \\dots \\downarrow\\rangle + \\dots + C\_{\\downarrow \\downarrow \\dots \\downarrow} | \\downarrow \\downarrow \\dots \\downarrow\\rangle = \\sum\_k C\_k | k \\rangle \\end{aligned}

$$

where

$$| k\\rangle = | \\sigma^z\_1 \\sigma^z\_2 \\dots \\sigma^z\_N\\rangle = | \\sigma^z\_1\\rangle \\otimes | \\sigma^z\_2\\rangle \\otimes \\dots \\otimes | \\sigma^z\_N\\rangle

$$

And, at the same time, the Hilbert space becomes exponentially large as $k \\in \[1, 2^N\]$.

More generally, a quantum many-body problem is described by a Hamiltonian $\\hat{H}$ and our goal is to figure out the ground state

$$\\hat{H} | \\psi\_0\\rangle = E\_0 | \\psi\_0\\rangle

$$

and its properties, but $| \\psi\_0\\rangle$ is not known. For a small number $N$, the problem can be solved by means of exact diagonalization of $\\hat{H}$, but the computational cost of doing so is at least of the order $\\mathcal{O}(2^N)$ for ‘local’ Hamiltonians. Here, a Hamiltonian is considered local, if

*   it has only k-body interactions
*   k is not extensive

An example for such a local Hamiltonian is the transverse field Ising model

$$\\hat{H} = \\sum\_{i < j} J\_{ij} \\hat{\\sigma}^z\_i \\hat{\\sigma}^z\_j + \\sum\_i h\_i \\hat{\\sigma}^x\_i

$$

Luckily, most interactions in physics are local, and when we fix $k$, the number of non-zero matrix elements,

$$|H\_{k k^\\prime}| = |\\langle k^\\prime | \\hat{H} | k\\rangle| \\neq 0

$$

grows only polynomially $\\mathrm{poly}(N)$. But still, this is only practical up to $N \\sim 40$ (for comparison: $2^{120}$ is approximately the number of atoms on earth).

**Variational Methods**

So in order to simulate larger system sizes one resorts to variational methods, which sample only a tiny sub-space of physically relevant states of the entire Hilbert space (e.g. constructed from the eigenstates $| \\psi \\rangle$ of the Hamiltonian $\\hat{H}$). So we assume (!), that a quantum state $| \\psi \\rangle$ can be parameterized by a polynomially large set of parameters $w$ in the way

$$| \\psi(w) \\rangle = \\sum\_k C\_k(w) | k\\rangle

$$

where

$$C\_k = \\langle k | \\psi \\rangle = \\psi(k) = \\psi(\\sigma^z\_1, \\dots, \\sigma^z\_N)

$$

To approximate the true ground state $| \\psi\_0 \\rangle$ one can tune the parameters based on the variational principle that the energy expectation value

$$E(w) = \\frac{\\langle \\psi(w) | \\hat{H} | \\psi(w)\\rangle}{\\langle \\psi(w) |\\psi(w) \\rangle} = \\frac{\\sum\_n |b\_n|^2 E\_n}{\\sum\_n |b\_n|^2} \\geq E\_0

$$

is minimized.

**Variational Classes**

**$E(w)$ can be calculated exactly**

There is one class of quantum many-body problems, for which $E(w)$ can be calculated exactly, i.e. apart from rounding errors. One example are **mean-field states**

$$| \\psi(w) \\rangle = | \\phi\_1 \\rangle \\otimes | \\phi\_2 \\rangle \\otimes \\dots \\otimes | \\phi\_N \\rangle

$$

with the single particle wave functions $| \\phi\_i \\rangle$ and $\\langle \\phi\_x | \\phi\_y \\rangle = \\delta\_{xy}$. Physical quantities such as the expectation value

$$\\langle \\psi(w) | \\hat{\\sigma}^x\_i | \\psi(w)\\rangle

$$

can be calculated exactly. Another example are matrix product states

$$| \\psi(w) \\rangle = \\sum\_k C\_k(w) | k \\rangle

$$

where

$$C\_k(w) = \\hat{M}\_1(\\sigma^z\_1) \\cdot \\hat{M}\_2(\\sigma^z\_1) \\cdot \\dots \\cdot \\hat{M}\_N(\\sigma^z\_N)

$$

is expressed by $\\chi \\times \\chi$ matrices.

**$E(w)$ can be calculated approximately**

There is another class of quantum many-body problems, where $E(w)$ can only be calculated approximately, but in a controlled way, so that the accuracy can be increased by putting in more computational effort.

Here, the quantum many-body states $| \\psi \\rangle$ needs to be “computationally trackable” that is

1) $\\langle k | \\psi \\rangle$ can be computed efficiently

2) one can sample efficiently from $P(k) = |\\langle k | \\psi \\rangle$

where “efficiently” means “polynomially in time” in terms of computational complexity. An example where condition 1) is violated are PEPS.

In addition, we also need the theorem by van den Nest that any expectation value of a local operator $\\hat{O}$ can be estimated with polynomial accuracy.

**Estimating Operator Properties**

The expectation value of an operator $\\hat{O}$ can be estimated the following way:

$$\\begin{aligned}\\langle \\hat{O}\\rangle &= \\frac{\\langle \\psi | \\hat{O} | \\psi \\rangle}{\\langle \\psi | \\psi \\rangle} = \\frac{\\sum\_{k k^\\prime} \\langle \\psi | k \\rangle \\langle k | \\hat{O} | | k^\\prime \\rangle \\langle k^\\prime | \\psi \\rangle}{\\sum\_k |\\langle \\psi | k \\rangle|^2} \\\\&= \\frac{\\sum\_k |\\langle \\psi | k \\rangle|^2 \\sum\_{k^\\prime} O\_{k k^\\prime} \\frac{\\langle k^\\prime | \\psi \\rangle}{\\langle k | \\psi \\rangle}}{\\sum\_k |\\langle \\psi | k \\rangle|^2} = \\sum\_k P(k) O^{\\mathrm{loc}}(k)\\end{aligned}

$$

with the probability distribution

$$P(k) = \\frac{|\\langle \\psi | k \\rangle|^2}{\\sum\_k |\\langle \\psi | k \\rangle|^2}

$$

and the local form of the operator

$$O^{\\mathrm{loc}}(k) = \\sum\_{k^\\prime} O\_{k k^\\prime} \\frac{\\langle k^\\prime | \\psi \\rangle}{\\langle k | \\psi \\rangle}

$$

To obtain a stochastic estimate, first values $k^{(1)}$, $k^{(2)}$, $\\dots$ $k^{(M)}$ need to be drawn from $P(k)$. Then the expectation value can be estimated via

$$\\langle \\psi | \\hat{O} | \\psi \\rangle \\approx \\frac{1}{M} \\sum\_{i=1}^M O^{\\mathrm{loc}}(k^{(i)})

$$

The error

$$\\Delta \\langle \\hat{O} \\rangle\_M \\sim \\sqrt{\\frac{\\mathrm{var}(O^{\\mathrm{loc}})}{M}} \\overset{M \\to \\infty}{\\longrightarrow} 0

$$

can be improved systematically for larger $M$, provided that the variance $\\mathrm{var}(O^{\\mathrm{loc}})$ is finite after all.

**Sampling from $P(k)$**

Using Markow-Chain Monte Carlo (MCMC) one can construct a series of values

$$k^{(1)} \\to k^{(2)} \\to k^{(3)} \\to \\dots \\to k^{(M)}

$$

that samples from $P(k)$, if the transition probbility $\\tau(k^{(i)} \\to k^{(i+1)})$ fulfills the detailed balance condition

$$P(k) \\tau(k \\to k^\\prime) = P(k^\\prime) \\tau(k^\\prime \\to k)

$$

Employing the Metropolis-Hastings algorithm one splits the transition probability into a selection probability $\\Omega$ and an acceptance ratio $A$

$$\\tau(k \\to k^\\prime) = \\Omega(k \\to k^\\prime) A(k \\to k^\\prime)

$$

where the Metropolis acceptance ratio is given by

$$A(k \\to k^\\prime) = \\min \\left(1, \\frac{P(k^\\prime)}{P(k)} \\frac{\\Omega(k^\\prime \\to k)}{\\Omega(k \\to k^\\prime)} \\right)

$$

**Quick Recap on Machine Learning**

**The Machine**

Generally speaking, machine learning deals with algorithms that approximate functionals $F(x\_1, \\dots, x\_N)$, where $x\_1, \\dots, x\_N$ are e.g. the pixel values of an image and the output is a single number, classifying the image as either “dog” or ” cat”. So $F(\\vec{x})$ is the “machine” and within the subfield of deep learning it may be a feed-forward deep neural network of the form

$$F(\\vec{x}, \\vec{w}) = g^{(D)} \\circ w^{(D)} \\dots \\circ g^{(2)} \\circ w^{(2)} \\circ g^{(1)} \\circ w^{(1)} \\vec{\\sigma}

$$

It consists of two parts

1) first a \\sum of the input values $\\vec{x}$ weighted with the weights $vec{w}$

$$\\sum\_y w\_{iy}^{(1)} x\_y = h^{(i)}\_i \\in \\mathbb{R}^{l\_1}

$$

2) and secondly the application of a non-linear function $g$ also called the activation function

$$(g \\circ h^{(1)})\_i = h^{\\prime (1)}\_i = g(h^{(1)}\_i) \\in \\mathbb{R}^{l\_1}

$$

Here $l\_1, l\_2, \\dots$ represent the width of the network and $D$ the depth (number of layers). The activation value switches from 0 (small input value) to 1 (large input value), so possible choices are for example $\\tanh(x)$ or $\\ln(\\cosh(x))$.

**The Learning**

The “learning” involves optimization of the weights $vec{w}$ to solve a certain task. There are generally three forms of learning:

1) supervised learning to make predictions based on labeled data

2) unsupervised learning to cluster data and identify patterns without labels

3) reinforcement learning, where a score function (in physics: energy function) is either maximized or minimized without labels

We are going to employ reinforcement learning to optimize the neural network quantum states. Let’s dive in.

**Neural Network Quantum States – Explained**

We represent the many-body wave function of a system of $N$ spin-$\\frac{1}{2}$-particles as an N-dimensional function

$$\\langle k | \\psi \\rangle = \\psi(\\sigma\_1^z, \\dots, \\sigma\_N^z; w)

$$

where $\\sigma\_i^z \\in {-1,1}$ are the projections across the $z$-axis. Using machine learning and especially deep learning this wave function is expressed as a neural network quantum state (NQS) in the form

$$\\psi(\\vec{\\sigma}) = g^{(D)} \\circ w^{(D)} \\dots \\circ g^{(2)} \\circ w^{(2)} \\circ g^{(1)} \\circ w^{(1)} \\vec{\\sigma}

$$

Importantly, for such a network the scalar products $\\langle \\sigma\_1^z, \\sigma\_2^z, \\dots, \\sigma\_N^z | \\psi \\rangle$ can be computed efficiently, i.e. in polynomial time.

**Why do NQS work?**

The main idea of NQS is to approximate a complicated function $F(vec{x})$ through simpler basis functions. That this even works is based on two theorems.

First, the **Kolmogorov-Arnold theorem** states, that any continuous, $n$-dimensional function $F(\\vec{x})$ can be approximated in the way

$$F(vec{x}) = \\sum\_{q = 0}^{2n} \\Phi \\left( \\sum\_{p=1}^n \\lambda\_p \\phi(x\_p + \\eta q) + q \\right)

$$

where $\\Phi$ and $\\phi$ are (hard to compute) univariate functions.

In addition, if for neural networks the form of the activation function is fixed, the **Universal Approximation Theorem** tells us

$$F(\\vec{x}) = \\sum\_{i=1}^N v\_i \\phi \\left( \\sum\_j w\_{ij} x\_j + b\_i \\right)

$$

which essentially reflects the structure of a neural network: a weighted sum of inputs $x\_j$ and weights $w\_{ij}$, followed by the application of a non-linear activation function $\\phi$.

**Reinforcement Learning of Quantum States**

For reinforcement learning we need to perform three steps:

1) obtain a flexible representation of $| \\psi \\rangle$

2) find an algorithm to compute $\\langle \\hat{H}\\rangle = E(w)$

3) perform the learning: determine the optimal weights $w$ from $\\min w(E(w))$

While a flexible representation of $| psi \\rangle$ is given by a deep neural network, for the actual learning we need to estimate the expectation value

$$E(w) = \\frac{\\langle \\psi\_M | \\hat{H} | \\psi\_M \\rangle}{\\langle \\psi\_M | \\psi\_M \\rangle} = \\frac{\\sum\_k P(k) E^{\\mathrm{loc}}(k)}{\\sum\_k P(k)} = \\langle E^{\\mathrm{loc}}(k) \\rangle\_P

$$

where

$$E^{\\mathrm{loc}}(k) = \\sum\_{k^\\prime} H\_{k k^\\prime} \\frac{\\langle k^\\prime | \\psi \\rangle}{\\langle k | \\psi \\rangle} = \\frac{\\langle k | \\hat{H} | \\psi \\rangle}{\\langle k | \\psi \\rangle}

$$

Here, we see why the requirement of $\\hat{H}$ being a local operator is important: now we just have a polynomial number of matrix elements $H\_{k k^\\prime}$ in the sum and thus $E^{\\mathrm{loc}}(k)$ can be computed efficiently.

**Optimization Procedure**

Minimization of $E(w)$ can be achieved via gradient descent, for which we need the gradient $\\frac{\\partial E(w)}{\\partial w\_p}$. It can be shown that the following relation holds:

$$\\frac{\\partial E(w)}{\\partial w\_P} = \\langle E^{\\mathrm{loc}}(k) O\_P(k) \\rangle\_P – \\langle E^{\\mathrm{loc}}(k)\\rangle\_P \\langle O\_P(k)\\rangle\_P

$$

with

$$O\_P = \\frac{\\partial}{\\partial w\_p} \\log \\langle k | \\psi \\rangle

$$

Thinking of the gradient $\\frac{\\partial E(w)}{\\partial w\_P}$ as the average of a function $G(k)$

$$\\frac{\\partial E(w)}{\\partial w\_P} = \\langle G(k)\\rangle\_P

$$

it can be estimated from stochastic samples

$$\\frac{\\partial E(w)}{\\partial w\_P} \\approx \\frac{1}{M} \\sum\_i^M G(k^{(i)})

$$

Now the gradient descent can be performed via

$$w^{(s+1)}\_P = w^{(s)}\_P – \\eta \\frac{\\partial E}{\\partial w\_P}

$$

where $\\eta$ is the learning rate, which should not be chosen to large. As another minimization method one could use stochastic gradient descent, where Gaussian noise is added to the gradient in order to avoid getting stuck in local minima

$$\\frac{\\partial E}{\\partial w\_p} + \\mathrm{Normal} \\left( \\mu=0,\\sigma^2 = \\frac{v}{M} \\right)

$$

centered around the origin with the variance $v$. This is equivalent to the langevin dynamics of a stochastic process $x\_p(t)$

$$x\_p(t + \\delta\_t) = x\_p(t) – \\nabla\_p v(t) \\delta\_t t + \\mathrm{Normal}(\\mu = 0, \\sigma^2 = 2 \\sigma\_t T)

$$

where the probability distribution pf $x\_p$ tends toward the Boltzmann distribution for $t \\to \\infty$

$$\\lim\_{t \\to \\infty} P(x) = \\frac{e^{-\\frac{v(x)}{T}}}{Z}

$$

In our analogy, the learning rate corresponds to the step size $\\eta = \\delta\_t$ and the variance $\\sigma^2 = \\frac{v}{M} = 2 \\sigma\_t T$ with $T = \\frac{\\eta}{M}$. So also the probability distribution of weights tends towards a Boltzmann distribution

$$\\lim\_{s \\to \\infty} P(\\vec{w}) \\sim \\frac{e^{-\\frac{E(\\vec{w})}{T}}}{Z}

$$

**Time-dependent Problems**

NQS cannot only be employed to learn ground states, but also to determine the time evolution of a quantum many-body state variationally, i.e. to solve the Schördinger equation

$$i \\frac{\\partial}{\\partial t} | \\phi(t) \\rangle = \\hat{H}(t) | \\phi(t) \\rangle$$

Therefore, the true state is approximated by an NQS $| \\phi(t) \\rangle \\approx | \\psi, w(t) \\rangle$, where the time dependence is moved into the variational parameters.

**Time-Dependent Variational Principle**

Our goal is to derive a time-dependent variational principle, t\\hat is based on minimizing the angle between the true time-evolved state $|\\phi(t+\\delta\_t) \\rangle$ and the time-evolved NQS $| \\psi, w(t+\\delta\_t) \\rangle$. Based on the work of Dirac and Frenkel, the time evolution for a small time step $\\delta\_t$ is approximated by

$$|\\phi(t+\\delta\_t) \\rangle = (\\hat{1} – i \\delta\_t \\hat{H}) |\\phi(t) \\rangle + \\mathcal{O}(\\delta^2\_t)$$

and

$$|\\psi, w(t+\\delta\_t) \\rangle = (\\hat{1} + \\delta\_t \\hat{\\Delta}) |\\psi, w(t) \\rangle + \\mathcal{O}(\\delta^2\_t)

$$

The first equation effectively discretizes the time evolution operator $U = e^{i \\hat{H} t}$ for $\\hbar = 1$. For the second equation we can determine $\\hat{\\Delta}$ from expression the time evolution step in the computational basis

$$\\langle k | \\psi, w(t+\\delta\_t) \\rangle = \\langle k | \\psi, w(t) \\rangle + \\delta\_t \\sum\_P \\frac{\\langle k | \\psi, w(t) \\rangle}{w\_P} \\dot{w}\_P + \\dots

$$

so that

$$\\hat{\\Delta} = \\sum\_P \\frac{\\partial \\langle k | \\psi, w(t) \\rangle}{\\partial w\_P} \\frac{\\dot{w}\_P}{\\langle k | \\psi, w(t) \\rangle} = \\sum\_P O\_P(k) \\dot{w}\_P

$$

To obtain a time-dependent variational principle we need to match the two states

$$\\begin{aligned}&|A \\rangle = (\\hat{1} – i \\delta\_t \\hat{H}) |\\phi(t) \\rangle \\\\ &|B \\rangle = \\left(\\hat{1} + \\delta\_t \\sum\_P O\_P(k) \\dot{w}\_P \\right) |\\phi(t) \\rangle\\end{aligned}

$$

locally in time and thereby may as well get the global time evolution. This is achieved by defining the fidelity

$$F\_{AB}(\\dot{w}\_P) = \\frac{|\\langle A|B\\rangle|^2}{\\langle A|A\\rangle \\langle B|B\\rangle}

$$

which is going to be quadratic in the lowest order in $dot{w}\_P$ and which is maximized. So from the time-dependent variational principle

$$\\max\_{\\dot{w}\_P} F\_{AB}(\\dot{w}\_P)

$$

follows the equation of motion for $\\dot{w}\_P$ and one can show that

$$i \\sum\_{P^\\prime} \\dot{w}\_{P^\\prime} S\_{P P^\\prime} = \\langle O\_P E^{\\mathrm{loc}}\\rangle – \\langle O\_P\\rangle \\langle E^{\\mathrm{loc}}\\rangle

$$

with the quantum geometric tensor (also called metric of the variational manifold)

$$S\_{P P^\\prime} = \\langle O^\*\_P O\_{P^\\prime}\\rangle – \\langle O^\*\_P\\rangle \\langle O\_{P^\\prime}\\rangle = \\left\\langle \\frac{\\partial \\psi}{\\partial w\_P} \\Big| \\frac{\\partial \\psi}{\\partial w\_{P^\\prime}} \\right \\rangle

$$

**Time-dependent Variational Monte Carlo (t-VMC)**

1.  sample $P(k,t) = \\frac{|\\langle k | \\psi, w(t) \\rangle |^2}{N(t)}$
2.  measure $S\_{P P^\\prime}$, $G\_P$ from the sample
3.  solve the linear system to find $\\dot{w}\_P(t)$
4.  $w\_P(t+\\delta\_t) = w\_P + \\delta\_t \\dot{w}\_P$ or use a better scheme

**Imaginary Time**

Also problems in imaginary time with

$$| \\phi(\\tau)\\rangle = e^{-\\tau \\hat{H}} | \\phi(0) \\rangle $$$$\\frac{\\partial}{\\partial \\tau} | \\phi(\\tau) \\rangle = – \\hat{H} | \\phi(\\tau)\\rangle

$$

and

$$\\lim\_{\\tau \\to \\infty} | \\phi(\\tau) \\rangle = | \\psi\_0 \\rangle

$$

if $\\langle \\psi\_0 | \\phi(0)\\rangle \\neq 0$ can be treated analogeously, leading to

$$\\sum\_{P^\\prime} \\dot{w}\_{P^\\prime} S\_{P P^\\prime} = -\\langle E^{\\mathrm{loc}} O^\*\_P\\rangle + \\langle O^\*\_P\\rangle \\langle E^{\\mathrm{loc}}\\rangle

$$

This is equivalent to the “stochastic reconfiguration” optimization scheme \[Sorella Book\] and very close to “natural gradient descent” \[Amari, 1998\].

**Learning from Experiments**

Here we perform projective measurements on an actual quantum system, e.g. a gas of atoms, of a quantity

$$Q(k) = |\\langle k | \\phi \\rangle|^2

$$

Our goal is to reconstruct $\\langle k | \\phi \\rangle$, i.e. the wave function, from these projective measurements in some basis $|k \\rangle$ (therefore also called quantum state reconstruction).

**Variational Principle**

Once again we need a variational principle to perform the learning. Starting with the probability distribution

$$P(k) = \\frac{|\\langle k | \\psi, w \\rangle|^2}{N(w)} = \\frac{F(k,w)}{N(w)}

$$

generated from a neural network, and the actually measured exact probabilities

$$Q(k) = |\\langle k| \\phi \\rangle |^2

$$

one can define as a metric between both probability distributions the Kullback-Leibler-Divergence

$$D\_{KL}(P||Q) = \\sum\_k Q(k) \\left\[ \\log(Q(k)) – \\log(P(k)) \\right\]

$$

So if $P(k) = Q(k)$ we have $D\_{KL} = 0$ and thus our variational principle is to minimize $D\_{KL}(w)$. It can be shown, that the gradient of $D\_{KL}(w)$ is given by

$$\\frac{\\partial D\_{KL}}{\\partial w\_P} = – \\left\\langle \\frac{\\partial}{\\partial w\_P} \\log(F(k)) \\right \\rangle\_Q + \\left \\langle \\frac{\\partial}{\\partial w\_P} \\log(F(k)) \\right \\rangle\_P

$$

with

$$\\frac{\\partial}{\\partial w\_P} \\log(F(k)) = 2 O\_P(k)

$$

Importantly, this gradient can be evaluated efficiently.

**The Phase**

So far, we were able to reconstruct the amplitutde of the quantum state, but not its phase. To obtain the phase, we need to perform measurements in several bases, connected to the original basis $|k \\rangle$ by a unitary transformation $\\hat{U}\_B$. Thus,

$$P\_B(k) = \\frac{|\\langle k | \\hat{U}^\*\_B | \\psi, w \\rangle|^2}{N\_B(w)}

$$

$$Q\_B(k) = |\\langle k | \\hat{U}^\*\_B | \\phi \\rangle|^2

$$

and

$$D\_{KL} = \\sum\_{k,B} Q\_B(k) \\left\[ \\log(Q\_B(k)) – \\log(P\_B(k)) \\right\]

$$

and the variation can be performed without issues if $\\hat{U}\_B$ is only a local rotation.

---

Clipped on Tuesday, December 21, 2021, 1:34 PM
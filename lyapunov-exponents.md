# Lyapunov Exponents

Originally introduced by A. M. Lyapunov in 1892, Lyapunov exponents are a tool to quantify the characteristic sensitive dependence on initial conditions for chaotic dynamical systems.

 Let $\vec{x}(0)$ be the initial state of a chaotic dynamical system in phase space and let $\vec{x}(0) + \vec{\delta}_0$ be a set of nearby initial states, where $||\vec{\delta}_0||$ is very small (on the order of $||\vec{\delta}_0|| \sim 10^{-15}$ determined by floating point accuracy). In numerical studies one finds, that the distance between such initially close orbits can be described by
$$
||\vec{\delta}(t)|| \sim ||\vec{\delta}_0|| e^{\lambda t}
$$
with $\lambda$ being the Lyapunov exponent, more specifically the largest Lyapunov exponent.

 Positive Lyapunov exponents mean that initially close orbits separate exponentially fast and thus indicate sensitive dependence on initial conditions. Negative or zero Lyapunov exponents hint at fixed points or stable, periodic orbits.

 But this is just part of the story, because for a $N$-dimensional dynamical system phase space needs to be described by an $N$-dimensional basis of orthogonal basis vectors and thus there are $N$ distinct either expanding, contracting or invariant directions and thus an entire spectrum of  $N$ distinct Lyapunov exponents.

 Let's consider an infinitesimally small hypersphere of initial states with main axes $\delta x^i_0$ and $i = 1, \dots, N$. Over time this hypersphere will deform into an ellipsoid, due to the presence of expanding and contracting directions, whose main axes are given by $\delta x^i_t$. Now, the Lyapunov exponents can be defined via
$$
\lambda_i = \lim_{t \to \infty} \frac{1}{t} \ln \left( \frac{\delta x^i_t}{\delta x^i_0} \right)
$$
This definition is based on the Multiplicative Ergodic Theorem, also called Oseledecs theorem as it was first proven by Valery Oseledec in 1965 \cite{Oseledec1968} and subject to several theoretical studies later on \cite{Raghunathan1979, Ruelle1979, Johnson1987, Walters1993}.

 Yet this definition is not very practical when it comes to actually calculating Lyapunov exponents, especially since due to the nature of a chaotic system we cannot guarantee that our hypersphere stays infinitesimally small over the time scale needed for the convergence of the Lyapunov spectrum. Therefore we will follow an alternative numerical approach in the following.


\textbf{Standard Method using Gram-Schmidt Orthogonalization}

We are using the standard method to determine the Lyapunov spectrum, originally developed by Benettin et al. \cite{Benettin1980}. For a more pedagogical introduction and an extension to time series data have a look at the work of Wolf et al. \cite{Wolf1985} and recent textbooks, such as \cite{Skokos2010, Strogatz2015, Vallejo2017}.

 Following \cite{Wolf1985}, we will consider the time evolution of one particular initial state according to the non-linear equations of motion, yielding the fiducial trajectory. But instead of an infinitesimally small hypersphere of initial conditions we will consider equivalently the time evolution of an initially orthonormal basis of vectors via the linearized equations, anchored on the fiducial trajectory.

 To be specific, let our $N$-dimensional dynamical system by described by the $N$-component, non-linear equation of motion
$$
\dot{\vec{x}} = \vec{f}(\vec{x})
$$
yielding the fiducial trajectory $\vec{x}^*(t)$. Then each of the orthonormal basis vectors $\vec{e}_i$ with $i = 1, \dots, N$ evolves according to the linearized equations of motion
$$
\dot{\vec{e}}_i = \hat{A} \dot{\vec{e}}_i
$$
with $\hat{A} = \frac{d \vec{f}}{d \vec{x}} \big|_{\vec{x} = \vec{x}^*}$ evaluated on the fiducial trajectory, yielding $N \times N$ additional equations of motion. Thus, the entire system of $N$ plus $N \times N$ equations is solved simultaneously. But even the linearized time evolution suffers from two numerical issues:

a) With time the vectors will grow exponentially large / small  for positive / negative Lyapunov exponents. 

b) Over time the vectors will collapse along the direction of greatest expansion. 

To overcome these two issues we make repeated use of the [[gram-schmidt]] orthonormalization procedure on the basis of vectors (or apply alternatively some other QR-decomposition method like a Householder transformation \cite{Geist1990}).

 Thus, in practice the system of $N + N \times N$ equations of motion is evolved for a certain time $\Delta t$ (typically of the order of one orbital period), starting with the initial ONB $\vec{e}^k = [\vec{e}^k_1, \dots, \vec{e}^k_N]$ and finally obtaining the set of vectors $\vec{u}^k = [\vec{u}^k_1, \dots, \vec{u}^k_N]$ during the $k$-th iteration.

 This system is orthogonalized using [[gram-schmidt]] yielding the set of vectors $\vec{v}^{k} = [\vec{v}^{k}_1, \vec{v}^{k}_2, \vec{v}^{k}_3]$. And that system is finally normalized and used as an initial ONB for the next round of iteration.

 The GS-orthogonalization never alters the direction of the first vector in the system, which therefore -- over time -- seeks out the most rapidly growing direction (i.e. characterised by the largest Lyapunov exponent).

 Due to the GS-orthogonalization procedure, the second vector has its component along the direction of greatest expansion removed. Throughout the iteration process we are constantly changing its direction, so it is also not free to seek out the second most rapidly expanding direction. But, the vectors $\vec{u}_1$ and $\vec{u}_2$ span the same two-dimensional subspace as the vectors $\vec{v}_1$, $\vec{v}_2$. So despite repeated vector replacements, these two vectors explore the two-dimensional subspace whose area is growing most rapidly. This area is governed by the largest and second largest Lyapunov exponent and grows according to $e^{(\lambda_1+\lambda_2) t}$ \cite{Benettin1980}.

 Thus, by monitoring the length of the largest vector, proportional to $e^{\lambda_1 t}$, and the area spanned by the first two vectors, both Lyapunov exponents can be determined. In practice, since vectors $\vec{v}_1$ and $\vec{v}_2$ are orthogonal, we can determine $\lambda_2$ directly from the mean growth rate vector $\vec{v}_2$.

 This reasoning can be generalized to the $k$-volume spanned by the first $k$ vectors, which grows according to $e^\mu$ where $\mu = \sum_{i=1}^k \lambda_i t$ and accordingly the mean grow rates of $k$-first vectors provide an estimate for the $k$ largest Lyapunov exponents $\lambda_k$:
$$
\lambda_k = \frac{1}{k} \sum_{k} \frac{\ln |\vec{v}_k|}{\Delta t}
= \frac{1}{k \Delta t} \sum_{k} \ln |\vec{v}_k|
$$
Upon thousands of iterations, the fiducial trajectory transverses the chaotic sea for $t \to \infty$ and thus the Lyapunov spectrum is independent of the specific initial conditions, as long as they are within the chaotic sea rather than some regular basin of attraction.

 While the largest Lyapunov exponents is an indicator of [[chaotic-dynamics]] and characterises single trajectories, the entire Lyapunov spectrum characterises the dynamical system as a whole e.g. by the rate of information loss. Employing the binary logarithm, the Lyapunov exponents give the rate of information loss (positive exponent) or gain (negative exponent) in bits/second.
 
A beautiful example is given by \cite{Wolf1985}: If the initial state of say the Lorenz attractor is prepared with an accuracy of 20 bits (one part in a million) and the largest Lyapunov exponent $\lambda_1 = 2.16$ represents the rate of information loss, after about $9 \, \mathrm{s} = 20 \, \mathrm{bits} /(2.16 \,\mathrm{bits}/\mathrm{s})$ the uncertainty about its state has spread over the entire attractor.

The Lyapunov spectrum can also serve to approximate the fractal dimension of a strange attractor according to the [[kaplan-yorke-conjecture]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[gram-schmidt]: gram-schmidt "Gram Schmidt"
[chaotic-dynamics]: chaotic-dynamics "Chaotic Dynamics"
[kaplan-yorke-conjecture]: kaplan-yorke-conjecture "Kaplan Yorke Conjecture"
[//end]: # "Autogenerated link references"
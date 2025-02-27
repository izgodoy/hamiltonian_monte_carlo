# hamiltonian_monte_carlo
A Markov-Chain Monte Carlo Simulation technique that uses Hamiltonian dynamics and a leapfrog integrator to improve accuracy in high-dimensional probability spaces.
Implementation based upon the work of Michael Betancourt's "A Conceptual Introduction to Hamiltonian Monte Carlo" accessible at https://arxiv.org/abs/1701.02434 and code from Colin Carroll's “Hamiltonian Monte Carlo from Scratch" accessible at https://colindcarroll.com/2019/04/11/hamiltonian-monte-carlo-from-scratch/.

**Introduction**\
The Hamiltonian Monte Carlo is a scaled-up Markov chain Monte Carlo method for statistical analysis that uses the gradient of the density function being sampled to create efficient sampling  representative of the distribution. It was originally developed in the late 1980s as the Hybrid Monte Carlo to tackle calculations in Lattice Quantum Chromodynamics, which studies the structure of protons and neutrons. Later, its potential for applied statistics was discovered. Instead of relying heavily on trial-and-error, its rigorous theoretical foundation allows applications in challenging high-dimensional problems.\

**Intuition in Higher Dimensions**\
	Consider a “target sample space, Q, parameterized by the real numbers such that every point q  Q can be specified with D real numbers. Given a parameter space, , we can then specify the target distribution as a smooth probability density function, (q), while expectations reduce to integrals over a parameter space,
E[f]=dq(q)f(q)” (Betancourt 2018, 4).
To balance computational efficiency and precision, it would be unwise to focus computational resources on evaluating the target density and relevant functions in regions of parameter space that do not have statistically significant contributions to the desired expectation. Intuition suggests to focus on regions where the integrand of the expectation value (i.e., the target density and target function) take on their largest values. Because expectation values are computed by integrating over a volume of parameter space and volume is not very large near the mode of the probability distribution, the behavior of both the density and the volume must be considered when identifying regions that dominate the expectation values.
	As the number of dimensions of the parameter space increases, the amount of volume concentrated around a given neighborhood increases. To understand this phenomenon, consider an example in which parameter space is partitioned into rectangular boxes centered around the mode [Figure 1]. In one dimension there are only two partitions neighboring the center partition, which leaves a significant amount of volume neighboring the mode. However, adding one more dimension creates eight neighboring partitions, and in three dimensions, there are 26 partitions neighboring the mode.

![image](https://github.com/user-attachments/assets/9b0caea8-7044-430e-b620-40d6ebd543da)

Figure 1: Visualization of volume partitions around the mode as the number of dimensions increases (Betancourt 2018, 6).

This principle can also be proven to occur in spherical coordinates and continues as the number of dimensions increases indefinitely. Thus, “volume is largest out in the tails of the target distribution away from the mode, and this disparity grows exponentially with the dimension of the parameter space” and the far regions of the integrated volume can give a significant contribution to the target expectation despite their smaller density (Betancourt 2018, 6).
	The neighborhood closest to the mode, though containing the highest density, has such a small volume in higher dimensions that its contribution to the expectation values is often not significant. The same is true for the neighborhood far away from the mode due to its vanishing densities. Thus, the neighborhood with the most significant contribution lies between the two extremes and is called the “typical set”. The typical set becomes more singular with increasing dimension due to the tension between density and volume. It is best to evaluate the integrand of expectation values only within the typical set, as including regions outside this is a waste of computing power. The Metropolis-Hastings algorithm, which consists of a proposal step and a correction (rejection) step for proposals that stray too far from the typical set of the target distribution, is used to identify the typical set.\

**Markov Chain Monte Carlo**\
	A Markov chain process simulates a sequence of events in which the probability of each event is dependent only on the state achieved in the previous event. For example, one can evolve the movement of a particle through time in discrete steps in which its trajectory is defined by the position and trajectory from the previous step. The resulting Markov chain is a progression of points in parameter space. Markov chain Monte Carlo makes use of this process to stochastically explore the typical set by using a Markov transition that preserves the target distribution. When a Markov chain is generated using this distribution, the resulting points should explore the typical set in three phases:
The Markov chain converges towards the typical set from its initial position in parameter space and the Markov chain Monte Carlo estimators suffer from strong biases.
The Markov chain finds the typical set and begins an initial exploration, causing the Markov chain Monte Carlo estimators to rapidly improve in accuracy as biases vanish.
The Markov chain refines its exploration of the typical set and the precision of the Monte Carlo estimators improves (Betancourt 2018, 11).
The Hamiltonian Monte Carlo adapts this Markov chain process to simulate Hamiltonian dynamics [Figure 2].

![image](https://github.com/user-attachments/assets/a962981c-fd70-4991-aaff-0f97a19e5e62)

Figure 2: Some examples of Hamilton Markov chains traversing a 2D Gaussian distribution with varying “step” sizes (Carroll 2019).

**Conclusion**\
	In implementation, there are more considerations to make about the Hamiltonian Monte Carlo, namely optimizing choices of kinetic energy, integration time, integrators, and bias correction practices. This theoretical overview aims to provide some geometric intuition for the algorithm and, hopefully, prompt the reader’s interest. When thoughtfully implemented, Hamiltonian Monte Carlo is a powerful yet efficient method of statistical analysis.

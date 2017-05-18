% Spectral Function in NuWro
% Tomasz Golan
% OPUS meeting, 17.05.2017

# Spectral Function

* The cross section formula (as in AA thesis)

$$\sigma = \frac{G_F^2\cos^2\theta_C}{8\pi^2E_k}\int\frac{dEd^3p'd^3k'}{E_{k'}E_pE_{p'}}P(\vec p, E)\delta(\omega+M-E-E_{p'})L_{\mu\nu}H^{\mu\nu}$$

* $P(\vec p, E)$ - spectral function
* $\vec p$ - target nucleon momentum
* $E = E_{A-1} + M - M_A$ - removal energy (excitation + $T_k$)
* $\omega = E_k - E_{k'}$ - energy transfer
* $k = (E_k, \vec k)$ , $k' = (E_{k'}, \vec k')$ - incoming, outgoing lepton
* $p = (E_p, \vec p)$, $p' = (E_{p'}, \vec p')$ - target, outgoing nucleon


# Spectral function II

* Lets rewrite the cross section formula

$$\sigma = A \int dEd^3p P(\vec p, E)\int \frac{d^3k'}{E_{k'}}\delta(E_{k'}+E_{p'} - E_0)\frac{LH}{E_pE_{p'}}$$

* $E_0 = E_k + M - E$
* First integral is performed (in MC) by selecting $\vec p$ and $E$ randomly according to $P(\vec p, E)$ distribution
* Thus, we need to solve

$$\sigma = \tilde A\int \frac{d^3k'}{E_{k'}}\delta(E_{k'}+E_{p'} - E_0)\frac{LH}{E_pE_{p'}}$$

for fixed $\vec p$ and $E$

# Going to CMS

* Velocity of CMS frame

$$\vec v = \frac{\vec p' + \vec k'}{E_{p'} + E_{k'}} = \frac{\vec p + \vec k}{E_0}$$

* $\frac{d^3k'}{E_{k'}}$ is Lorentz invariant

$$\sigma = \tilde A\int \frac{d^3k'_{CMS}}{E_{k',CMS}}\delta(E_{k'}+E_{p'} - E_0)\frac{LH}{E_pE_{p'}}$$

* Lets rewrite $\delta$ in CMS frame

$$\delta(E_{k'}+E_{p'} - E_0) = \delta(\gamma(E_{k', CMS} + E_{p', CMS}) - E_0)$$

# Delta in CMS

* Since $\vec k'_{CMS} + \vec p'_{CMS} = 0$

$$\delta(\gamma\sqrt{|\vec k'_{CMS}|^2 + m^2} + \gamma\sqrt{|\vec k'_{CMS}|^2 + M^2} - E_0)$$

* It is easy to show that

$$|\vec k'_{CMS}| = \sqrt{\frac{(\tilde E_0^2 + m^2 - M^2)^2}{4\tilde E_0^2} - m^2}$$

where $\tilde E_0 \equiv \frac{E_0}{\gamma}$

# Czarek's trick

$$\int \delta(f(x))g(x)d^3x = \oint\limits_{f(x)=0}\frac{g(x)}{|\nabla f(x)|}dS$$

* In our case

$$|\nabla_{\vec k'_{CMS}}(\gamma(E_{k', CMS} + E_{p', CMS}) - E_0)| = \gamma |\frac{\vec k'_{CMS}}{E_{k',CMS}} - \frac{\vec p'_{CMS}}{E_{p',CMS}}|$$

* In CMS $\vec k'_{CMS} = -\vec p'_{CMS}$

$$\gamma |\frac{\vec k'_{CMS}}{E_{k',CMS}} - \frac{\vec p'_{CMS}}{E_{p',CMS}}| = \gamma(|\vec v_{k', CMS}| + |\vec v_{p', CMS}|)$$

# Finally

* In CMS 

$$dS = |\vec k'_{CMS}|^2d\phi_{CMS}d\cos\theta_{CMS}$$

* Thus, the cross section

\begin{eqnarray*}
\sigma & = & \tilde A\int \frac{d^3k'_{CMS}}{E_{k',CMS}}\delta(E_{k'}+E_{p'} - E_0)\frac{LH}{E_pE_{p'}} \\
& = & \tilde A\int \frac{|\vec k'_{CMS}|^2d\phi_{CMS}d\cos\theta_{CMS}}{E_{k',CMS}}\frac{1}{\gamma(|\vec v_{k', CMS}| + |\vec v_{p', CMS}|)}\frac{LH}{E_pE_{p'}}
\end{eqnarray*}

# NuWro implementation

* In NuWro the final formula

$$\sigma = \tilde A \int \frac{|\vec k'_{CMS}|^2d\phi_{CMS}d\cos\theta_{CMS}}{E_{k'}}\frac{\gamma\sqrt{1-(\vec v\vec n)^2}}{|\vec v_{k'} - \vec v_{p'}|}\frac{LH}{E_pE_{p'}}$$

where $\vec n$ is the direction of outgoing lepton in CMS

* Comparing to my calculations in CMS

$$\sigma = \tilde A\int \frac{|\vec k'_{CMS}|^2d\phi_{CMS}d\cos\theta_{CMS}}{E_{k',CMS}}\frac{1}{\gamma(|\vec v_{k', CMS}| + |\vec v_{p', CMS}|)}\frac{LH}{E_pE_{p'}}$$

# From Czarek's talk

$$\int \delta(f(x))g(x)d^3x = \oint\limits_{f(x)=0}\frac{g(x)}{|\nabla f(x)|}dS$$

* Gradient

$$|\nabla_{\vec k'}(E_{k'} + E_{p'} - E_0)| = |\vec v_{k'} - \vec v_{p'}|$$

* Surface element

$$dS = J\cdot dS_0 = \sqrt{\sin^2\theta_0 + \gamma\cos^2\theta_0}\cdot|\vec k'_{CMS}|^2d\phi_{CMS}d\cos\theta_{CMS}$$

where $\theta_0$ is the angle between $\vec k'_{CMS}$ and $\vec v$

# J check

\begin{eqnarray*}
J & = & \gamma\sqrt{1-(\vec v\vec n)^2} \\
& = & \sqrt{\gamma^2\cdot(\sin^2\theta_0 + \cos^2\theta_0) - v^2\cos^2\theta_0} \\
& = & \sqrt{\gamma^2\sin^2\theta_0 + (\gamma^2 - v^2)\cos^2\theta_0}
\end{eqnarray*}
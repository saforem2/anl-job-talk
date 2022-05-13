---
title: "Scientific Data Science"
width: 1280px
height: 720px
center: true
margin: 0.5
highlightTheme: monokai-sublime
css:
 - 'custom/custom_theme.css'
 - 'custom/graze-pro.css'
transition: slide
revealOptions:
    transition: 'slide'
    scripts:
      - plugin/reveal.js-menu/menu.js
      - plugin/reveal.js-plugins/chalkboard/plugin.js
      - plugin/reveal.js-plugins/customcontrols/plugin.js
      - plugin.js
    menu:
      themes: true
      transitions: true
      transition: 'slide'
...
---

<!-- # [<img width=7% src="assets/github-sketch.svg" align="center" style="vertical-align:middle;" />`l2hmc-qcd`](https://github.com/saforem2/l2hmc-qcd) <br> -->
<br>
<br>

# Scientific Data Science:
## An Emerging Symbiosis

<br>

#### Sam Foreman

May, 2022

<div align="bottom">

<br>
<br>
<br>

<a href="https://github.com/saforem2/anl-job-talk"><img src="assets/github-sketch.svg" align="left" width=10% style="vertical-align:bottom;"></a>
<img src="assets/static/logo.png" style="width:15vw;vertical-align:bottom!important;margin-top:2%;" align="right">

</div>

<!-- <h1 display=inline:block;></h1> -->
<!-- # [<img width=7% src="assets/github.svg" align="top">](https://github.com/saforem2/l2hmc-qcd) [l2hmc-qcd](https://github.com/saforem2/l2hmc-qcd) -->
<!-- <i class="fab  fa-twitter faa-float animated "></i> @saforem2</a></h4> -->
<!-- <div id="float" align="bottom"> -->

<!-- [<img align="left" width=10% src="assets/github-sketch.svg" />](https://github.com/saforem2/l2hmc-qcd) -->
<!-- <img align="left" style="padding-left:45px;" width=10% src="assets/qr.svg"> -->

<!-- <img src="assets/static/logo.png" style="width:10vw;vertical-align:bottom;padding-right:4%;" align="right"> -->

<!-- </div> -->


---

<div style="text-align: left;">

### Hamiltonian Monte Carlo (HMC)

- Hamiltonian Dynamics:

</div>

<div id="note" style="width=60%;max-width:60%;font-size:120%;line-height:150%;">

<!-- `\[ -->
<!-- \left(\begin{smallmatrix} \dot x\\ \dot v \end{smallmatrix}\right) -->
<!-- = \left(\begin{smallmatrix}+\partial_v & \cdot \\ \cdot & -\partial_x\end{smallmatrix}\right)  -->
<!-- \]` -->

$\dot{x} = \frac{\partial H}{\partial v}$,
$\quad$
$\dot{v} = - \frac{\partial H}{\partial x}$

</div>

![](assets/hmc1.svg)  <!-- .element width="90%" -->

---

<div style="text-align:left;">

### HMC: Leapfrog Integrator

- Hamiltonian: $H(x, v) = S(x) + \frac{1}{2} v^{2}$

- Target distribution: <span id="blue">$p(x)$
  $\propto e^{-S(x)}$</span>, <span id="green">$p(v)$ $\propto
  e^{-\frac{1}{2}v^{2}}$ </span> $\Longrightarrow$
  - $p(x, v) \propto $<span id="blue"> $e^{-S(x)}$</span> $\cdot$ <span id="green">$e^{-\frac{v^{2}}{2}}$</span>$ = e^{-H(x, v)}$

- Hamilton's equations:
</div>

<span id="note" style="font-size:1.0em">$\dot{x} = \frac{\partial H}{\partial v}$</span>
$\hspace{10pt}$
<span id="note" style="font-size:1.0em">$\dot{v} = - \frac{\partial H}{\partial x}$</span>

<div style="text-align:left;">

- <span id="red">Leapfrog Integrator</span>:

</div>

<div id="note" style="max-width:50%;">

1. $ \tilde{v} = v + \frac{\varepsilon}{2} \partial_{x} U$

2. $ x' = x + \varepsilon \tilde{v} $

3. $ v' = \tilde{v} + \frac{\varepsilon}{2} \partial_{x} U$

</div>

---

# HMC 

<section data-background-iframe="https://chi-feng.github.io/mcmc-demo/app.html"
          data-background-interactive>

---

<div id='dark'>

# <span style="color: #3B4CC0;">Critical Slowing Down</span>

<div id="left" style="width: 45%; font-size: 100%;text-align:left;align:left;margin-left:25pt;margin-top:20pt;font-size:1.1em;">

<h6><span id="note" style="color:rgb(123,159,249);background-color:rgba(123,159,249,0.15);">Charge Freezing</span></h6>

- <span style="color:rgb(192,212,245);font-weight:600;">$Q$ gets stuck!
<br>
<br>
- <span style="color:rgb(242, 203, 183);">$\delta Q \longrightarrow 0$</span> <span style="color:rgb(242, 203, 183);"> </span>
<br>
<br>
- <span style="color:rgb(238, 132, 104);">need to wait $N_{\mathrm{configs}}\longrightarrow \infty$</span>

<br><span id="note" style="color:rgb(255,82,82);background-color:rgba(255,82,82,0.15);padding:10px;margin-left:35pt;align:center;">
$\tau_{\mathrm{int}}^{Q} \longrightarrow \infty$
</span>

</div>

<div id="right" style="align:right;">

<img src="assets/critical_slowing_down.svg" style="max-width:85%; height:auto;align:right;">

</div>

</div>

---

<div id='dark' style="vertical-align:center;text-align:left;">

# Motivation

<span style="font-size:0.8em;">

- For independent samples: 
  $$\langle \mathcal{O}\rangle \propto\int\left[\mathcal{D}x\right]\mathcal{O}(x)e^{-S(x)}
  \simeq\frac{1}{N}\sum_{n=1}^{N}\mathcal{O}(x_{n})$$
  $$\Rightarrow \sigma^{2}=\frac{1}{N}\text{Var}\left[\mathcal{O}(x\right)]$$
- Accounting for <span id="blue">autocorrelations</span>:
  $$ \sigma^{2}=\frac{\color{#0091ea}{\tau_{\mathrm{int}}^{\mathcal{O}}}}{N}\text{Var}\left[\mathcal{O}(x)\right] $$
- <span id="blue">$\tau_{\mathrm{int}}^{\mathcal{O}}$</span> is known to scale <span
  id="red">exponentially</span> as we approach physical lattice spacing.

</span>

</div>

<!-- --- -->
<!-- ## Lattice QCD -->

<!-- - Non-perturbative approach to solving the QCD theory of the strong interaction -->
<!--   between quarks and gluons. -->
<!-- - Calculations in LatticeQCD proceed in 3 steps: -->
<!--   1. <span id="red">Gauge Field Generation</span>: Use Markov Chain Monte Carlo (MCMC) methods -->
<!--      for sampling _independent_ gauge field (gluon) configurations. -->
<!--   2. <span id="red">Propagator calculations</span>: Compute how quarks propagate in these fields -->
<!--      (_quark propagators_) -->
<!--   3. <span id="red">Contractions</span>: Method for combining quark propagators into correlation -->
<!--      functions and observables. -->

---

<div id='dark'>

#### L2HMC: LeapfrogLayer

<div id="float" style="width:100%;align:center;">

<img src="assets/drawio/update_steps.svg" style="width:31%;align:left;">
<img src="assets/drawio/leapfrog_layer_dark2.svg" style="width:64%;align:right;">
<img src="assets/drawio/network_functions.svg" style="width:90%;align:center;">

</div>

---

<div style="text-align:left;">

## Algorithm

1. <span style="background-color:#303030;border-radius:6px;">`input:`</span> $x$ (lattice configuration)

2. $v\sim\mathcal{N}(0, \mathbb{1})$

3. `\((x'',v'') \gets \texttt{TransitionKernel}(x, v)\)`

    1. $v' = \color{#ff5252}{\Gamma^{\pm}}\left[x, \partial_{x}U\right]$

    2. Update $x$ in two parts:

    `\[\begin{align}
    x' &= m_{A}\odot x + m_{B}\odot\color{#0091ea}{\Lambda^{\pm}}\left[x, v'\right]\\
    x'' &= m_{B}\odot x' + m_{A}\odot \color{#0091ea}{\Lambda^{\pm}}\left[x', v'\right]
    \end{align}\]`

    3. $v'' = \color{#ff5252}{\Gamma^{\pm}}\left[x'', \partial_{x}U''\right]$

</div>
    

---
<div id='dark' style="text-align:left;">

## Algorithm

- **Input**: <span id="red" style="text-align:left;">$x$</span>

<div class="column" style="width=100%;font-size:0.77em;">

1. Resample <span id="red">$\mathbf{v} \sim \mathcal{N}(0, \mathbb{1})$</span>,
   construct <span id="red">$\xi$</span>$=($<span id="red">$\mathbf{x}$</span>$,$ <span id="red">$\mathbf{v}$</span>$)$, also construct $\xi' = (x', v)$ where $x' = R x$, $\xi'' = (x'', v)$, etc
 
2. Generate <span style="color:#0091ea;">proposal $\xi^{\ast}$</span> by passing <span id="red">initial
   $\xi$</span> through $N_{\mathrm{LF}}$ **leapfrog
   layers**:

   <div id="note" style="width:55%;color:rgb(255, 255, 255);background-color:rgba(255, 255, 255, 0.1);text-align:center;padding:5px;">

   <span id="red">$\xi$</span> $ \hspace{1pt}\xrightarrow[]{\tiny{\mathrm{LF} \text{ layer}}}\xi_{1} \longrightarrow\cdots \longrightarrow \xi_{N_{\mathrm{LF}}} =$ <span style="color:#0091ea;">$\xi^{\ast}$</span>

   </div>

3. Compute the **Metropolis-Hastings** (MH) acceptance (with Jacobian
   $\mathcal{J}$) 

   <div id="note" style="width:55%;align:center;text-align:center;padding:5px; color:rgb(255,255,255);background-color:rgba(255,255,255,0.1);padding:5px;">

   $A(\color{#0091ea}{\xi^{\ast}}|\color{#ff5252}{\xi})=
   \mathrm{min}\left\\{1,
   \frac{p(\color{#0091ea}{\xi^{\ast}})}{p(\color{#ff5252}{\xi})}\mathcal{J}\left(\color{#0091ea}{\xi^{\ast}},\color{#ff5252}{\xi}\right)\right\\}$

   </div>

4. <span id="green">`if training`</span>: Evaluate the **loss function** $\mathcal{L}\gets
   \mathcal{L}_{\theta}(\color{#ff5252}{\xi^{\ast}}, \color{#ff5252}{\xi})$ and backprop

5. Evaluate MH criteria and assign the next state in the chain according to

   <div id="note" style="width:65%;align:center;text-align:center;padding:5px; color:rgb(255,255,255);background-color:rgba(255,255,255,0.15);padding:5px;">

   $\mathbf{x}_{i+1}\gets
   \begin{cases}
     \color{#0091ea}{\mathbf{x}^{\ast}} \small{\text{ w/ prob }} A(\color{#ff5252}{\xi^{\ast}}|\color{#0091ea}{\xi}) \hspace{25pt}✅ \\\\
     \color{#ff5252}{\mathbf{x}} \hspace{14px}\small{\text{ w/ prob }} 1 - A(\color{#ff5252}{\xi^{\ast}}|\color{#0091ea}{\xi}) \hspace{11pt}❌
     \end{cases}$

   </div>

</div>

</div>
---

<div id='dark'>

### Lattice Gauge Theory

<div class="row">

<div class="column" style="width: 65%; font-size: 90%; align:right;">

<h5><b><u> <span style="color:#cfcfcf;">Link variables</span></u></b></h5>
   $U_{\mu}(x) = e^{i x_{\mu}(n)}\in U(1)$, <br>with <span id="note" style="color:rgb(255,255,255);background-color:rgba(255,255,255,0.1);padding:5px;">$x_{\mu}(n)\in[-\pi,\pi]$</span><br><br>

<h5><b><u><span style="color:#cfcfcf;">Wilson Action</span></u></b></h5>
  <span id="note"
  style="color:rgb(255,255,255);padding:5px;background:rgba(255,255,255,0.15);"> $S_{\beta}(x)
  = \beta\sum_{P} 1 - \cos \color{#0091Ea}{x_{P}}$</span>
  <span id="blue" style="font-size:0.65em;align:right;">$x_{P}= x_{\mu}(n) + x_{\nu}(n+\hat{\mu})-x_{\mu}(n+\hat{\nu})-x_{\nu}(n)$</span>
<!-- <br><br><h5><b><u> Topological Charge</u></b></h5> -->

</div>

<div class="column" style="width:30%;vertical-align:center;">

![](assets/plaq.drawio.svg)  <!-- .element align="center" width="90%"-->

</div>

</div>

<div style="align=center;">
<h5><b><u>Topological Charge</u></b></h5>

<span id="green">**Continuous:** </span> <span id="note" style="padding:7px;background:#D0F3D5;color:#1c1c1c!important;">$Q_{\mathbb{R}} = \frac{1}{2\pi}\sum_{P} \sin x_{P}\in\mathbb{R}$ </span>

$\hspace{10pt}$ <span id="red"> **Discrete:**</span> <span id="note" style="padding:7px;background:#F7C2CC;color:#1c1c1c;text-align:right;">$Q_{\mathbb{Z}} = \frac{1}{2\pi}\sum_{P} \left\lfloor x_{P}\right\rfloor\hspace{18px}\in\mathbb{Z}$</span>

  $\hspace{45pt}$ with $\left\lfloor x_{P}\right\rfloor = x_{P}-2\pi\left\lfloor\frac{x_{P}+\pi}{2\pi}\right\rfloor$</span>

<br>
</div>


  <!-- <span id="note" style="padding:8px;background:rgba(232, 245, 233,0.8);color:rgb(76, 175, 80);">✅ $Q_{\mathbb{R}} = \frac{1}{2\pi}\sum_{P} \sin x_{P}\in\mathbb{R}$ </span> -->

  <!-- <span id="note" style="padding:8px;background:rgba(255, 235, 238,0.8);color:rgb(244, 67, 54);align:right;">❌ $Q_{\mathbb{Z}} = \frac{1}{2\pi}\sum_{P} \left\lfloor x_{P}\right\rfloor\hspace{18px}\in\mathbb{Z}$</span> -->


</div>

---

<div style="text-align:left;">

# Networks

- Stack gauge links as `x.shape = [Nb, 2, Nt, Nx, 2]`

`\[
x_{\mu}(n) \gets \left[\cos(x), \sin(x)\right]
\]`

- <span id="blue">`\(x\)`</span> network: 
  - <span id="blue">`\(\Lambda^{\pm}\)`</span> `\((x, v) \rightarrow \left[S_{x}, T_{x}, Q_{x}\right]\)`
  - $S_{v}, T_{v}, Q_{v}$ used to update $v$ L2HMC update
- <span id="red">`\(v\)`</span> network: 
  - <span id="red">`\(\Gamma^{\pm}\)`</span> `\((x, \partial_{x}U) \rightarrow \left[S_{v}, T_{v}, Q_{v}\right]\)`
  - $S_{x}, T_{x}, Q_{x}$ used to update $v$ L2HMC update

</div>

---

<div style="text-align:left; font-size:smaller;">

# $x$ Networks $\Lambda^{\pm}$:

- **input**: $\color{#0091EA}{(x, v)}$

`\[
\begin{align}
h_{1} &= \sigma\left(W_{x}\, \color{#0091EA}{x} + W_{v}\, \color{#0091EA}{v} + b\right)\\
h_{2} &= \sigma\left(W_{1} h_{1} + b_{1}\right)\\
\quad\vdots&\\
h_{n} &= \sigma\left(W_{n} h_{n} + b_{n}\right)\\
\color{#00CF53}{S_{x}} &= \color{#ff5252}{\lambda_{S}}\tanh\left(W_{S} h_{n} + b_{S} \right)\\
\color{#00CF53}{Q_{x}} &= \color{#ff5252}{\lambda_{Q}}\tanh\left(W_{Q} h_{n} + b_{Q} \right)\\
\color{#00CF53}{T_{x}} &= W_{T} h_{n} + b_{T}
\end{align}
\]`


- **output**: $\color{#00CF53}{S_{x}, T_{x}, Q_{x}}$

- **Note**: $\color{#ff5252}{\lambda_{S}}$, $\color{#ff5252}{\lambda_{Q}}$ are trainable parameters

</div>

---

<div style="font-size:75%;">

## Detail

1. **Update** $v$:
`\[v' = v \cdot \exp\left[\varepsilon \color{#ff5252}{S_{v}}\left(x, \partial_{x}U\right)\right] - \frac{\varepsilon}{2} \bigg[\partial_{x} U\cdot \exp\left[\varepsilon \color{#ff5252}{Q_{v}}\left(x, \partial_{x}U\right)\right] + \color{#ff5252}{T_{v}}\left(x, \partial_{x}U\right)\bigg]\]`
2. **Update** $x$ (in two parts):
    1. `\(x' = x_{A} + \bigg\{x_{B}\cdot\exp\left[\varepsilon \color{#00CCFF}{S_{x}}(x_{B}, v')\right] + \varepsilon\cdot\big[v'\cdot \exp\left[\varepsilon \color{#00CCFF}{Q_{x}}\left(x_{B}, v'\right)\right] + \color{#00CCFF}{T_{x}}\left(x_{B}, v'\right)\big]\bigg\}\)`
    2. `\(x'' = x'_{B} + \bigg\{x'_{A}\cdot\exp\left[\varepsilon \color{#00CCFF}{S_{x}}(x'_{A}, v')\right] + \varepsilon\cdot\big[v'\cdot \exp\left[\varepsilon \color{#00CCFF}{Q_{x}}\left(x'_{A}, v'\right)\right] + \color{#00CCFF}{T_{x}}\left(x'_{A}, v'\right)\big]\bigg\}\)`
3. **Update** $v$:
`\[v'' = v' \cdot \exp\left[\varepsilon \color{#ff5252}{S_{v}}\left(x'', \partial_{x}U\right)\right] - \frac{\varepsilon}{2} \bigg[\partial_{x} U\cdot \exp\left[\varepsilon \color{#ff5252}{Q_{v}}\left(x'', \partial_{x}U''\right)\right] + \color{#ff5252}{T_{v}}\left(x'', \partial_{x}U''\right)\bigg]\]`

</div>


---
<div style="text-align:left;">

## Loss Function

</div>

<div style="font-size:0.9em;">

<div style="text-align:left;">

- Maximize the <span id="blue">_expected squared charge difference_ </span>:

</div>

<span id="note" style="text-align:center;align:center;">`\(\mathcal{L}(\theta) = \color{#228BE6}{\mathbb{E}_{p(\xi)}}
\left[-\color{#FA5252}{{\delta Q}}^{2}_{\color{#FA5252}{\mathbb{R}}}(\xi', \xi)\cdot
A(\xi'|\xi)\right]\)`</span>

<div style="text-align:left;">

<br>

- $\color{#FA5252}{\delta Q_{\mathbb{R}}}$ is the <span id="red">tunneling rate</span>
  $$\color{#FA5252}{\delta Q_{\mathbb{R}}}(\xi',\xi)=\left|Q_{\mathbb{R}}(x') - Q_{\mathbb{R}}(x)\right|$$
- $A(\xi'|\xi)$ is probability of accepting the proposal $\xi'$.
  $$ A(\xi'|\xi) = \min\left(1, \frac{p(\xi')}{p(\xi)}\left|\frac{\partial \xi'}{\partial \xi^{T}}\right|\right\)$$

</div>

</div>

</div>

---

<div style="text-align:left;font-size:smaller;">

## HMC Sweep

- Fix $N_{\mathrm{LF}}$
- Sweep over $\varepsilon$ for generic HMC
- **Goal:** Maximize $\langle \delta Q_{\mathbb{Z}}\rangle$, averaged over _thermalized_ configs

<figure>
<p><img src="./assets/hmcNlf16_sweep2.svg" align="center" style="max-width:90%;">
<figcaption>HMC Sweep with $N_{\mathrm{LF}}=16$ on $16\times16$ lattice at $\beta = 4$</figcaption>
</figure>

</div>

---

<div style="font-size:smaller;text-align:left;">

## Comparison

- Maintain a buffer of:
  - $N_{\mathrm{b}}$ chains ran to generate $M$ configurations (accept/reject steps)
- To calculate averages, we drop the first $\sim 25\\%$ of chains (thermalize), and average

`\[
  \langle \delta Q_{\mathbb{Z}}\rangle = \frac{1}{N_{b}}\sum_{n=1}^{N_{b}}\bigg\{\frac{1}{M}\sum_{m=1}^{M} \left|Q'_{\mathbb{Z}} - Q_{\mathbb{Z}}\right|\bigg\}
\]`

<figure>
<img src="./assets/dQint_eval.svg" align="left" style="max-width:49%;">
<img src="./assets/dQint_hmc.svg" align="right" style="max-width:49%;">
<p><figcaption>Comparison of the tunneling rate between trained model (left) and HMC (right).</figcaption>
</figure>


</div>

---

<div id='dark'>

#### Integrated Autocorrelation time: <span style="color:#FF2052">$\tau_{\mathrm{int}}$</span>

<div id="left" style="margin-left:6%;max-width:75%;">

<img src="assets/autocorr_new.svg" style="width:100%;">

</div>

<div id="right" style="align:left;text-align:left;width:30%;margin-right:15%;">

We can measure the performance by comparing $\tau_{\mathrm{int}}^{Q}$ for the
<span style="color:#FF2052">**trained model**</span> to <span
style="color:#9F9F9F;">**HMC**</span>.
</div>

<img src="assets/charge_histories.svg" style="width:90%;margin-top:-4%;">

</div>
---

<div id="dark">

### Integrated Autocorrelation Time


<img src="assets/tint1.svg" style="width:100%;">

Comparison of $\tau_{\mathrm{int}}^{Q}$ for <span
style="color:#5BC461;">**trained models** </span> vs <span
style="color:#9F9F9F;">**HMC** </span> with different trajectory lengths,
$N_{\mathrm{LF}}$, at $\beta = 4, 5, 6, 7$

</div>

---


<div id="dark">

# Interpretation

<span style="font-size:0.6em; align:center;">
$\hspace{32pt}$
Deviation in $x_{P}\hspace{20pt}$
$\hspace{10pt}$ Topological charge mixing
$\hspace{20pt}$ Artificial influx of energy
</span>

![](assets/ridgeplots.svg)

<span style="font-size:0.7em;">
Illustration of how different observables evolve over a
single L2HMC trajectory.
</span>

</div>

---
<div id="dark">

## Interpretation

![](assets/plaqsf_ridgeplot.svg) <!-- .element width="48%" align="center" -->
![](assets/Hf_ridgeplot.svg)  <!-- .element width="48%" align="center" -->

<div class="row">

<div class="column" style="font-size:0.6em;margin-left:11%;max-width:55%;text-align:center;">

<div id="note" style="color:rgb(255,255,255);background-color:rgba(255,255,255,0.15);margin-top:-40px;">

Average plaq $\langle x_{P}\rangle$

</div>

</div>
<div class="column" style="font-size:0.6em;margin-left:12%;text-align:center;margin-right:4%;">

<div id="note" style="color:rgb(255,255,255);background-color:rgba(255,255,255,0.15);margin-top:-40px;">

Average energy $H - \sum\log|\mathcal{J}|$
</div>
</div>
</div>
<small><b>Fig.</b> Illustration of how the trained model artificially
increases the energy towards the middle of the trajectory, allowing the sampler
to tunnel between isolated sectors.

---

<div id="dark">

## Plaquette analysis: $x_{P}$


![](assets/plaqsf_vs_lf_step1.svg) <!-- .element width="100%" -->

<div class="row">
<div class="column" style="font-size:0.6em;margin-left:11%;max-width:55%;text-align:center;">

<div id="note" style="color:rgb(255,255,255);background-color:rgba(255,255,255,0.15);margin-top:-40px;">

Deviation from $V\rightarrow\infty$ limit,  $x_{P}^{\ast}$

</div>

</div>
<div class="column" style="font-size:0.6em;margin-left:12%;text-align:center;margin-right:4%;">

<div id="note" style="color:rgb(255,255,255);background-color:rgba(255,255,255,0.15);margin-top:-40px;">

Average $\langle x_{P}\rangle$, with $x_{P}^{\ast}$ (dotted-lines)
</div>
</div>
</div>
<small><b>Fig.</b> Plot showing how <b>average plaquette</b>, $\left\langle
x_{P}\right\rangle$ varies over a single trajectory for models trained at
different $\beta$, with varying trajectory lengths $N_{\mathrm{LF}}$</small>

</div>
---

<div align="left">

## $SU(3)$

- **Plaquettes**:

</div>

![](./assets/plaquetteUxy.svg)  <!-- .element width="40%" align="center" -->

<br>

![](./assets/plaquetteUtx.svg)  <!-- .element width="50%" align="center" -->

---

<div align="left">

## Topological Charge $Q$

- **Field Theoretic Definition**:

$$Q = \int d^{4}x q(x)$$

where

$$q(x) = \frac{1}{32\pi^{2}} \varepsilon_{\mu\nu\rho\sigma} \mathrm{Tr}\left[F_{\mu\nu} F_{\rho\sigma}\right]$$

</div>

---

<div align="left">

### Topological Charge

- Discretize $Q$ on the lattice as 

$$Q = a^{4} \sum_{x} q_{L}(x)$$

- Discretize $q_{L}$ using $C_{\mu\nu}^{\mathrm{plaq}}(x)$

$$q_{L}^{\mathrm{plaq}} = \frac{1}{32\pi^{2}} \varepsilon_{\mu\nu\rho\sigma}\mathrm{Tr}\left(C_{\mu\nu}^{\mathrm{plaq}} C_{\rho\sigma}^{\mathrm{plaq}}\right)$$

with

</div>

![](./assets/qplaq.svg)  <!-- .element width="50%" align="center" -->


---

<div id="dark">

# [<img width=7% src="assets/github.svg" align="top">](https://github.com/saforem2/l2hmc-qcd) [l2hmc-qcd](https://github.com/saforem2/l2hmc-qcd)

- [arXiv: 2105.03418](https://arxiv.org/abs/2105.03418)
- [arXiv: 2112.01582](https://arxiv.org/abs/2112.01582)
- [arXiv: 2112.01586](https://arxiv.org/abs/2112.01586)

- Source code publicly available

- Both `pytorch` and `tensorflow` implementations with support for distributed training, automatic checkpointing, etc.

- Generic interface, easily extensible

- <b>Work in progress</b> scaling up to 2D, 4D $SU(3)$

</div>

---

# Gauge Equivariance

- Transform untraced plaquettes (matrix valued) `\(P'_{\mu\nu}(x)\gets P_{\mu\nu}(x)\)`

<div id="note" align="center" style="text-align:center;max-width:80%;">

`\[
P_{\mu\nu}(x) \equiv U_{\mu}(x)\,U_{\nu}(x+\hat{\mu})\,U^{\dagger}(x+\hat{\nu})\,U^{\dagger}(x)
\]`

</div>

- Change back to links and update gauge configuration

`\[
U'_{\mu}(x) = P'_{\mu\nu}(x)P^{\dagger}_{\mu\nu}(x)U_{\mu}(x)
\]`


---

## Non-Compact Projection 
<small>[arXiv:2002.02428](https://arxiv.org/abs/2002.02428)</small>

<div style="font-size:0.8em;">

- Project $x \in[-\pi, \pi]$ onto $\mathbb{R}$ using a transformation $z = g(x)$:
$$ z = \tan\left(\frac{x}{2}\right) $$
- Perform the update in $\mathbb{R}$:
  $$ z' = m^{t}\odot z + \bar{m}^{t}\odot \left[\alpha z + \beta\right]$$
- Project back to $[-\pi, \pi]$ using $x = g^{-1}(z)$:
  $$ x = 2 \tan^{-1}(z) $$

</div>

</div>

---
<!-- .slide: data-background="#1c1c1c" -->

<div id="dark">

## Non-Compact Projection
<small>[arXiv:2002.02428](https://arxiv.org/abs/2002.02428)</small>

- Combine into a single update:
  $$ x' = \color{#228BE6}{m^{t}}\odot x +
  \color{#FA5252}{\bar{m}^{t}}\odot\left[2\tan^{-1}\left(\alpha\tan\left(\frac{x}{2}\right)\right)+\beta\right]
  $$
- With corresponding Jacobian:
  $$ \frac{\partial x'}{\partial x} = \frac{\exp(\varepsilon s_{x})}{\cos^{2}(x/2)+exp(2\varepsilon s_{x})\sin(x/2)} $$


</div>


---


<!-- <div style="text-align:left;font-size:0.7em;"> -->

<!-- # [Adversarial Training](https://mdpi-res.com/d_attachment/entropy/entropy-24-00415/article_deploy/entropy-24-00415-v2.pdf) -->


<!-- - Use Real-NVP flows as generator object -->
<!-- - First update $v$ via  -->

<!-- $$ -->
<!-- v' = v + \varepsilon \cdot t_{x}(x) -->
<!-- $$ -->

<!-- - Next we update $x$ using -->

<!-- $$ -->
<!-- x' = x \odot \exp\left[\varepsilon \cdot s(v) \right] + t(v')\odot \exp\left[\varepsilon \cdot s(v')\right] -->
<!-- $$ -->

<!-- <div align="center"> -->

<!-- > Note that we update $x$ in a single step -->

<!-- </div> -->

<!-- - When a part of $x$ is updated through another part of $x$, the correlation between data will inevitably increase -->
<!--   - Thus, we can try instead only updating $x$ based on $v$ -->

<!-- </div> -->

<!-- <div style="text-align:left;"> -->

<!-- # Adversarial Training -->

<!-- - At each epoch of training, generate samples through the <span id="red">transition kernel</span> -->
<!--   - Treated as <span id="red">fake</span> samples -->
<!-- - Use <span id="blue">MC algorithm </span> to compute $A(\xi'|\xi)$ -->
<!--   - Treated as <span id="blue">true</span> samples -->

<!-- - A discriminator will be trained to distinguish the <span id="blue">"true samples"</span> from the <span id="red">"fake samples"</span> -->

<!-- - Our transition kernel will be trained to fool the discriminator. -->
<!-- <div id="note"> -->

<!-- A discriminator will be trained to distinguish the <span id="blue">"true -->
<!-- samples"</span> from the <span id="red">"fake samples"</span> -->

<!-- </div> -->


<!-- </div> -->

<!-- --- -->

<!-- <div style="text-align:left;"> -->

<!-- # Adversarial Training -->

<!-- - After getting new samples, we: -->
<!--   1. <span id="red">Drop</span> some samples of the buffer in a constant ratio -->
<!--   2. <span id="green">Insert</span> new accepted samples -->
<!--     - <span id="note">Improves the quality of the sampler</span> -->
<!-- - Train using GANs w/ objective: -->

<!-- `\[ -->
<!-- \begin{align} -->
<!-- \min_{K}\,\,\max_{D} V(D, K) =\,\,\min_{T}\,&\,\max_{D}\bigg\{\mathbb{E}_{x\sim B(x)}\left[D(x)\right]\\ &\,-\mathbb{E}_{z\sim\mathcal{N}(0, 1)}\left[D(K(z))\right]\bigg\} -->
<!-- \end{align} -->
<!-- \]` -->

<!-- $$ -->
<!-- - \mathbb{E}_{z\sim\mathcal{N}(0, 1)}\left[D(K(z))\right] -->
<!-- $$ -->


<!-- </div> -->

---

<div id="dark">

## Acknowledgements

<div id="left">

### Collaborators:
 - Xiao-Yong Jin
 - James C. Osborn

### References:
 - [Link to slides](https://bit.ly/phys-sem)
 - [Link to github](https://github.com/saforem2/l2hmc-qcd)
 - [reach out!](mailto://foremans@anl.gov)
 - [Link to HMC demo](https://chi-feng.github.io/mcmc-demo/app.html)
 - [arXiv:2105.03418](https://arxiv.org/abs/2002.02428)
 - [arXiv:2002.02428](https://arxiv.org/abs/2002.02428)

</div>

<div id="right">

### Huge thank you to:
 - Yannick Meurice
 - Norman Christ
 - Akio Tomiya
 - Luchang Jin
 - Chulwoo Jung
 - Peter Boyle
 - Taku Izubuchi
 - ECP-CSD group
 - ALCF Staff + Datascience Group

</div>

<small> 

<br>

This research used resources of the Argonne Leadership Computing Facility,
which is a DOE Office of Science User Facility supported under Contract
DE-AC02-06CH11357.

</small>

</div>

---
# Thank you!


<!-- - 🔗 Slides: [bit.ly/phys-sem](https://bit.ly-phys-sem) -->
- Reach out! [foremans@anl.gov](mailto:///foremans@anl.gov)
  - <i class="fab  fa-twitter faa-float animated "></i><a href="https://www.twitter.com/saforem2"> @saforem2</a>
  - <i class="fab  fa-github faa-float animated "></i><a href="https://www.github.com/saforem2"> saforem2</a>
<!-- - <a href="https://twitter.com/saforem2/status/1505576506696900608?s=20&t=qtuJbHVVIfYKoWwFWUjcNw"> -->
<!-- <i class="fab  fa-twitter faa-float animated "></i> What advice would you give to yourself as an undergrad?</a> -->


<style>

@import url('https://fonts.googleapis.com/css2?family=Coming+Soon&display=swap');

:root {
    --r-heading-text-transform: none;
    --r-heading-margin: 0 0 20px 0;
    --r-heading-font: "Inter", serif; "Merriweather", "Inter", "OpenSans-Bold","Inter","graze-pro", "Open Sans", Helvetica, Impact, sans-serif;
    --r-main-background-color: #1c1c1c;
    --r-main-font: "Inter", sans-serif, "Inter", "SourceSansPro", "Open Sans", "SourceSansPro", Helvetica Neue, sans-serif;
    --r-main-font-size: 34px;
    --r-block-margin: 5px;
    --r-heading-margin: 0 0 20px 0;
    --r-heading-line-height: 1.2;
    --r-heading-letter-spacing: -0.45px;
    --r-heading-word-spacing: 0.5px;
    --r-heading-text-transform: none;
    --r-heading-text-shadow: none;
    --r-heading-font-weight: 700;
    --r-heading1-text-shadow: none;
    --r-heading1-size: 2em;
    --r-heading2-size: 1.75em;
    --r-heading3-size: 1.5em;
    --r-heading4-size: 1.25em;
    --r-heading5-size: 1.1em;
    --r-heading6-size: 1.05em;
    --r-code-font: "agave Nerd Font", monospace;
    --r-link-color: #007DFF;
    --r-link-color-dark: #f92672;
    --r-link-color-hover: #63ff51;
    --r-controls-color: #228BE6;
    --r-progress-color: #404040;
    --r-selection-background-color: rgba(255, 255, 0, 0.15);
    --r-selection-color: rgb(255, 255, 0);
    --r-main-color: #bbbbbb;
    --text-muted: #757575;
    --text-faint: #404040;
    --r-heading-color: #cfcfcf;
    --r-background-color: #fff;
    -webkit-font-smoothing:subpixel-antialiased;
}

.reveal {
    font-family: var(--r-main-font), sans-serif;
    font-size: var(--r-main-font-size);
    font-weight: 400 !important;
    color: var(--r-main-color);
    background-color: var(--r-main-background-color);
}

.reveal h1,
.reveal h2,
.reveal h3,
.reveal h4 {
    margin: var(--r-heading-margin);
    color: var(--r-heading-color);
    font-family: var(--r-heading-font);
    line-height: var(--r-heading-line-height);
    word-spacing: var(--r-heading-word-spacing);
    text-transform: var(--r-heading-text-transform);
    text-shadow: var(--r-heading-text-shadow);
    font-weight: var(--heading-font-weight);
    word-wrap: break-word;
}

.reveal h1 {
    font-weight: 800;
    font-size: var(--r-heading1-size);
}

.reveal h2 {
    font-weight: 700;
    font-size: var(--r-heading2-size);
}

.reveal h3 {
    font-weight: 700;
    font-size: var(--r-heading3-size);
}

.reveal h4 {
    font-weight: 700;
    font-size: var(--r-heading4-size);
}

.reveal h5 {
    font-weight: 600;
    font-size: var(--r-heading5-size);
}
.reveal h6 {
    font-weight: 500;
    font-size: var(--r-heading6-size);
}


.reveal ul ul,
.reveal ul ol,
.reveal ol ol,
.reveal ol ul {
  margin-bottom: 10px;
}

.reveal ul:not(:last-child) {
    margin-bottom: 2px;
}

.reveal ul:last-child {
    margin-bottom: 20px;
}

.reveal ul ul {
    list-style: circle;
}
.container {
  position: relative;
}

.make-it-pop {
  filter: drop-shadow(0 0 10px purple);
}

.bottomright {
  position: absolute;
  bottom: 8px;
  right: 16px;
  font-size: 18px;
}

@media (max-width: 600px) {
  section {
    -webkit-flex-direction: column;
    flex-direction: column;
  }
}

.row {
  display: flex;
}

.column {
  flex: 50%;
}


#left {
  margin: 0 0 5px 5px;
  text-align: left;
  float: left;
  z-index: -10;
  width: 48%;
  font-size: 0.85em;
}

#right {
  margin: 0 0 5px 0;
  float: right;
  max-width: 48%;
  text-align: left;
  z-index: -10;
  width: 48%;
  font-size: 0.85em;
}

#darkBack {
    background-color: #1c1c1c;
    color: #efefef;
    .reveal a {
        color: #F92672;
        transition: color 0.15s ease;
    }
    .reveal a:hover {
        color: var(--r-link-color-hover);
    }
}

.multiCol {
    display: table;
    table-layout: fixed; /* don't fudge depending on content */
    width: 100%;
    text-align: left; /* matter of taste, makes imho sense */
    .col {
        display: table-cell;
        vertical-align: top;
        width: 50%;
        padding: 2% 0 2% 3%; /* some vertical, and between columns */
        &:first-of-type { padding-left: 0; } /* there's nothing before col1 */
    }
}

.reveal blockquote:before {
  content: "❝";
  font-size: 2.5em;
  margin-right: .05em;
  line-height: 0.1em;
  vertical-align: -0.3em;
  text-align: left !important;
  color: var(--text-faint);
  font-style: normal !important;
}

.reveal blockquote p {
  color: var(--text-muted);
  font-style: normal !important;
  font-align: left;
  display: inline;
  text-align: left;
}

.reveal blockquote em{
  color: var(--text-muted);
  text-align: left;
}

.reveal blockquote {
  border-radius: 8px !important;
  margin: 0.5rem 0rem 0.5rem 0rem;
  text-align: center;
  padding-top: 1rem;
  padding-left: 2rem;
  padding-bottom: 1rem;
  padding-right: 2rem;
  width: auto;
  max-width: 66%;
  font-style: normal !important;
}

.reveal hljs {
    background: #1c1c1c !important;
}

.hljs {
    background: #202020 !important;
    border-radius: 8px !important;
}

#blue {
    color: #0091EA;
}
#bright {
    color: #00A2FF;
}
#cyan {
    color: #00CCFF;
}
#purple {
    color: #AE81ff;
}
#green {
    color: #00CF53;
}
#yellow {
    color: #FFFF00;
}
#lightpink {
    color: #E64980;
}
#pink {
    color: #F92672;
}
#red {
    color: #FA5252;
}
#grey {
    color: #666666;
}

#noteinverse {
    background-color: #1c1c1c;
    border-radius: 5px;
    border-color: #1c1c1c;
    color: #efefef;
    padding: 8px;
    margin: 4px;
}

#note {
    background-color: rgba(255, 255, 255, 0.1);
    padding: 10px;
    border-radius: 8;
    border-color: rgba(240, 240, 240, 1.0);
    color: rgb(255, 255, 255);
    margin: auto;
}

#mark {
    border-radius: 7;
    margin: 4px;
    padding: 8px;
    background-color: #FFFF00;
    color: #1c1c1c;
    font-weight: 700;
}

.reveal .mark {
    border-radius: 7;
    margin: 4px;
    padding: 8px;
    background-color: #FFFF00;
    color: #1c1c1c;
    font-weight: 700;
}

#dark {
    background-color: #1c1c1c;
    color: #efefef;
    --r-link-color: #228bE6;
    --r-header-color: #666666;
    -webkit-font-smoothing:subpixel-antialiased;
}

#halfsize {
    font-size: 0.5em;
}

#large {
  font-size: 1.25em;
}

</style>

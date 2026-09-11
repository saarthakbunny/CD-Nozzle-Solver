# CD Nozzle Surrogate Solver

A live, self-improving neural network surrogate model for a 2D axisymmetric
convergent-divergent (CD) nozzle — trained on ANSYS Fluent CFD data, deployed
as a shared web app.

**🔗 Live tool:** https://saarthakbunny.github.io/CD-Nozzle-Solver/

## What this does

Instead of treating nozzle design as fixed "inputs → outputs" the way ANSYS
Workbench forces you to, this tool lets you fill in *any* combination of
known values — geometry, boundary conditions, or target performance — and
automatically solves for whatever's missing:

- Give it all 7 design inputs → get instant performance predictions (forward mode)
- Give it some boundary conditions plus a target mass flow rate or exit
  pressure → it solves for the throat/exit radius needed to hit that target
  (inverse design)

## How it works

- A small feedforward neural network (2 hidden layers, 20 neurons each) is
  trained on 51 CFD design points, generated via Latin Hypercube Sampling
  and solved in ANSYS Fluent.
- The network is implemented from scratch — forward pass, backpropagation,
  and the Adam optimizer — first in MATLAB, then ported to JavaScript for
  this web version.
- Forward prediction and inverse design use the *same* underlying mechanism:
  backpropagation adjusts either the network's weights (during training) or
  the input values themselves (during inverse solving), depending on what's
  being solved for.

## The shared, growing dataset

This isn't a static tool. When a result is checked against a real Fluent
run, it can be marked "validated" and added to the training set — instantly,
for everyone who opens this link, via a shared Firebase database. The model
retrains itself on the larger dataset automatically. The more it's used and
verified, the better it gets.

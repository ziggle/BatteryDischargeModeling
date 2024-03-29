# Battery Discharge Modeling

## Introduction

A critical part of my job is estimating how long a system will run off of a single battery charge. Historically, these estimates have been made using simple linear models using average currents. I have recently run some tests that show this approach is not accurate enough for some loads. People generally are satisfied with run time estimates that are accurate within ±5%. I recently had some estimates that were off by 20% using a simple linear model and it is time to improve my model.

## Background

### Objectives

* Establish the process needed to produce accurate operating time estimates
* Specify the information required for this process.<br>Things like (1) HA current vs voltage, (2) battery capacity variability, (3) SOC.
* Specify the calculations needed to generate estimates.

### Completion Criteriea

This effort will be complete when my residuals show no systematic variation — the residuals are normally distributed with zero mean. This will allow me to estimate the accuracy of my modeling.

## Accu30 Discharge Measurement

I am including an Excel spreadsheet that I am including as a candidate for a template to use for Keithley 2401A-based discharge analysis. The spreadsheet is simple, but does the job.
Ideally, I need to include charging and data from multiple batteries. This work will come, it just takes time.

## Mathematical Model

I am starting out with the simple first-order ODE model shown below.

$$Q = Q_0 · f\(V_{Battery}\)$$

$$\frac{dQ}{dt} = I_{Load}\(V_{Battery}\)$$

For my work here, I will just use Euler method to solve this equation.

## Alternate Modeling Approach

The ODE-based approach has proven quite accurate. I have started to wonder if there might be an easier way to obtain the same result. Consider the following derivation, where Q<sub>0</sub> is the battery's capacity at full charge.

$$Q = Q_0 \cdot SOC\(V\) \rightarrow dQ = Q_0 \cdot SOC'\(V\) \cdot dV$$

$$dt = -\frac{dQ}{I\(V\)}$$

$$T =  \int_{Q_0}^{0} -Q_0 \cdot \frac{SOC'\(V\)}{I\(V\)}\cdot dV $$

I should be able to easily evaluate this in a tool like Mathcad. This will be one line of reasoning. 

Numerically, this approach is a problem because it involves computing a numerical derivative. These rarely work because they are noisy. In my case, I do not have well-characterized current vs voltage data on the hardware.

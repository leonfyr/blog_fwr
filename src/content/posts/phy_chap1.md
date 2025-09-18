---
title: 'Chapter 1-Physical quantities and units'
published: 2025-09-15
updated: 2025-09-15
description: 'A-Level Physics Chapter 1'
image: ''
tags: [Physics, A-Level, Note]
category: '[Series] AL Physics'
draft: false 
lang: 'en'
---

<input type="checkbox" class="hooray"> Test

- [1 Physical quantities and units](#1-physical-quantities-and-units)
  - [1.1 Physical quantities](#11-physical-quantities)
  - [1.2 SI units](#12-si-units)
  - [1.3 Errors and uncertainties](#13-errors-and-uncertainties)
  - [1.4 Scalars and Vectors\*](#14-scalars-and-vectors)
  - [1.? Measurements](#1-measurements)
    - [Length](#length)
    - [Mass](#mass)
  - [1.?? Uncertainties](#1-uncertainties)

# 1 Physical quantities and units

Updated on 2025-09-15

## 1.1 Physical quantities

**Physical quantity** := a quantity that can be measured and *consists of a numerical magnitude and unit*. 

| Quantity                              | Size                       |
| :------------------------------------ | :------------------------- |
| Diameter of an atom                   | $10^{−10} \mathrm{m}$      |
| Wavelength of UV radiation            | $10 \mathrm{nm}$           |
| Height of an adult human              | $2 \mathrm{m}$             |
| Distance between Earth and Sun (1 AU) | $1.5 × 10^{11} \mathrm{m}$ |
| Mass of a hydrogen atom               | $10^{−27} \mathrm{kg}$     |
| Mass of an adult human                | $70 \mathrm{kg}$           |
| Mass of a car                         | $1000 \mathrm{kg}$         |
| Seconds in a day                      | $90 000 \mathrm{s}$        |
| Seconds in a year                     | $3 × 10^7 \mathrm{s}$      |
| Speed of sound in air                 | $300 \mathrm{ms^{−1}}$     |
| Power of a light bulb                 | $60 \mathrm{W}$            |
| Atmospheric pressure                  | $1 × 10^5 \mathrm{Pa}$     |

> From Save My Exams

For AS: $g = 9.81ms^{-2}$

## 1.2 SI units
**Base quantities** are the quantities on the basis of which other quantities are expressed. 

**Derived quantities** are the quantities that are expressed in terms of base quantities. 

A derived quantity has an equation which links to other quantities (e.g. $F=ma$). 

| Base Quantities     | SI Units                 |
| ------------------- | ------------------------ |
| Length              | metre ($\mathrm{m}$)     |
| Mass                | kilogram ($\mathrm{kg}$) |
| Time                | second ($\mathrm{s}$)    |
| Current             | Ampere ($\mathrm{A}$)    |
| Temperature         | Kelvin ($\mathrm{K}$)    |
| Amount of substance | Molar ($\mathrm{mol}$)   |
| Luminous intensity  | Candela ($\mathrm{cd}$)  |

| Derived Quantities | Units                        |
| ------------------ | ---------------------------- |
| Power              | $\mathrm{kgm^2s^{-3}}$       |
| Charge             | $\mathrm{As}$                |
| Voltage            | $\mathrm{kgm^2s^{-3}A^{-1}}$ |


| Factor ($10^x$) | Name  | Symbol |
| --------------- | ----- | ------ |
| 12              | tera  | T      |
| 9               | giga  | G      |
| 6               | mega  | M      |
| 3               | kilo  | k      |
| -1              | deci  | d      |
| -2              | centi | c      |
| -3              | milli | m      |
| -6              | micro | μ      |
| -9              | nano  | n      |
| -12             | pico  | p      |

--- 

**Homogeneity of an equation**

An equation is **homogeneous** if quantities on BOTH sides of the equation has the same unit. 

A homogeneous equation may not be physically correct, but a physically correct equation will always be homogeneous.

:::note[question]
The speed $v$ of a liquid leaving a tube depends on the change in pressure $\Delta P$ and the density $\rho$ of the liquid. The speed is given by the equation

$$
v = k(\frac{\Delta P}{\rho})^n
$$

Where k is a constant that has no units

What is the value of n?
:::

:::tip[Significant figures]
Digits considered *significant*: non-zero digits, zeros who:

- appearing anywhere between two non-zero digits

- trailing zeros in a number containing a decimal point

Digits considered *not significant*: leading zeros, trailing zeros in a number without a decimal point
:::

## 1.3 Errors and uncertainties

**Random errors**:

- values are scattered about the true value

- can be reduced by average

- Examples: Reading scales from different angles

**Systematic errors**:

- the average / peak is not the true value

- the reading is larger or smaller than (or varying from) the true reading by a constant amount 

- can be eliminated by careful calibration

- Examples: Zero Error, Parallax Error

**Precision** := the range of the values / how close the result is to each other / the size of the smallest division

- affected by random error

- improve: repeat and average

**Accuracy** := how close the result is to the true value

- affected by systematic error

- improve: technique, accurate instrument

---

**Uncertainty Calculation**

Absolute Uncertainty (Always 1 s.f. for the final result)

$y = b \pm c \Rightarrow \Delta y = \Delta b + \Delta c$

Percentage Uncertainty (1 / 2 s.f.)

$
\left. 
\begin{aligned}
&y=b\cdot c \\
&y = \frac{b}{c}
\end{aligned}
\right\}
\Rightarrow
\frac{\Delta y}{y} = \frac{\Delta b}{b} + \frac{\Delta c}{c}
$

$y = a^n \Rightarrow \frac{\Delta y}{y} = n \cdot \frac{\Delta a}{a}$

When the value times a *constant*, the *absolute uncertainty* changes but the *percentage uncertainty* doesn't. 

As the s.f. of the absolute uncertainty is always one, the s.f. of the value can therefore be determined. 


## 1.4 Scalars and Vectors*

| Scalar          | Vector                 |
| --------------- | ---------------------- |
| magnitude       | magnitude + direction  |
| Distance, Speed | Displacement, Velocity |

You also need to know: 

- [x] Vector Addition & Subtraction

- [x] Represent a vector as two perpendicular components. 

Direction : N of E 30° / 30° above x-axis (math)

**Remember** always to include a *direction* when the result is a vector. 

## 1.? Measurements

### Length

| Method                 | Min / cm | Max / cm | Smallest Division / mm |
| ---------------------- | ------ | ------ | -------------------- |
| Measuring Tape         | 0      | 150    | 1                    |
| Metre Rule             | 0      | 100    | 1                    |
| Vernier Caliper        | 0      | 15     | 0.02                 |
| Micrometer Screw Gauge | 0      | 2.5    | 0.01                 |

**Vernier Caliper**(游标卡尺): main scale + vernier scale

**Micrometer Screw Gauge**: main scale(0.5) + fractional scale(0.01)

**Remember to:**

- [x] Check zero

- [x] Repeat & Average

- [x] Avoid parallax error

### Mass

balance

## 1.?? Uncertainties

I've taken these notes on class but they are neither in the syllabus nor on past papers. Anyway, I put them here.

**Uncertainty**

Three main types of uncertainty:

- Random Uncertainties

- Systematic Errors

- Reading Uncertainties

**The Limit of Reading** of a measurement is equal to the smallest graduation of the scale of an instrument. 

**The Degree of Uncertainty** of a reading (end reading) is equal to half the smallest graduation of the scale of an instrument. 

Absolute - fractional errors - percentage errors

1 mm - 1/208 - 0.48%
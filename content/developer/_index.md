---
title: "Development"
date: 2024-04-08T22:10:00+02:00
weight: 60
---

This documentation provides some additional background on extending and hacking around the Engelsystem.
It assumes that you have read the [development](https://github.com/engelsystem/engelsystem/blob/main/DEVELOPMENT.md)
and [contributing](https://github.com/engelsystem/engelsystem/blob/main/CONTRIBUTING.md) guidelines
for a basic setup and guidance.

## Design Philosophy

The Engelsystem functionality and code structure after the refactoring is loosely inspired by Laravel
(and uses some Laravel components).
We try to provide a somehow stable version in our `main` branch that "Just Works" and has the newest features.

New Versions are normally released after bigger CCC events like the Chaos Communication Congress and thus
at least once a year.
External dependencies should fulfill their function well and be actively maintained.

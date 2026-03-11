---
title: Getting Started
layout: single
permalink: /getting_started/
---

From install, users should be able to use the network cleanup module.
In order to gain access to the Fluid Modelling Module, users need to ensure that the [pandapipes](https://www.pandapipes.org/) library is installed in their QGIS environment. Users can install the pandapipes library by pasting the following command into the OSGeo4w Shell:-

```bash
pip install pandapipes
```
Depending on their version of QGIS, users may not be able to access the MTO module either. If this is the case, ensure that the [pandas](https://pandas.pydata.org/) library is installed in your QGIS environment. Similarly, users can paste the following command into the OsGeo4W shell to install this library:-
```bash
pip install pandas
```
Sometimes, the [openpyxyl](https://openpyxl.readthedocs.io/en/stable/) library will also be required for the MTO builder. 
```bash
pip install openpyxyl
```
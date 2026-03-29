---
title: Material Takeoff
layout: single
permalink: /mto_builder/
toc: true

---

A collection of tools to quickly abstract network features such as bends, tees and unions to aid procurement.

## 1 Bends

Identifies all bends per corridor in a given network layer.

### 1.1 Parameters

| Label                          | Name                  | Type                                    | Description                                                 |
| -------------------------------|-----------------------|-----------------------------------------|                                                             |
|Input layer                     | `INPUT`               |[vector: line]                           | Line layer user wishes to identify bends on                 |
|ID Field                        | `ID_FIELD`            |[tablefield: any] Default: Not set       | Field containing unique identifiers on line layer           |
|Minimum Bend Angle (degrees)    |`MIN_ANGLE`            |[numeric: double] Default: 30            | Bends with angles smaller than this value will be discarded |
|ID Expression                   |`ID_EXPRESSION`        | 	[expression] Default: "CONCAT('BEND_',LPAD($id,5,'00000'))"| ID expression for each bend.            |

### 1.2 Outputs

| Label                          | Name                  | Type                                    | Description                                                 |
| -------------------------------|-----------------------|-----------------------------------------|                                                             |
|Bends                           | `OUTPUT`              |[vector: point]          | Returns a point layer containing all identified bends along with their angle|

### 1.3 Python Code

```py

import processing

processing.run("algorithm_id", {parameters_dictionary})

```

## 2 Tees
Identifies all tee junctions in a given network layer.

### 2.1 Parameters

| Label                          | Name                  | Type                                    | Description                                                 |
| -------------------------------|-----------------------|-----------------------------------------|                                                             |
|Input layer                     | `INPUT`               |[vector: line]                           | Line layer user wishes to identify junctions on             |
|ID Field                        | `ID_FIELD`            |[tablefield: any] Default: Not set       | Field containing unique identifiers on line layer           |
|ID Expression                   |`ID_EXPRESSION`        | 	[expression] Default: "CONCAT('JUNCTION_',LPAD($id,5,'00000'))"| ID expression for each junction.    |

### 2.2 Outputs

| Label                          | Name                  | Type                                    | Description                                                   |
| -------------------------------|-----------------------|-----------------------------------------|                                                               |
|Tees                            | `OUTPUT`              |[vector: point]                          | Returns a point layer containing all identified Tee Junctions |

### 2.3 Python Code

```py

import processing

processing.run("algorithm_id", {parameters_dictionary})

```

## 3 Four Way Unions
Identifies all four way junctions in a given network layer.

### 3.1 Parameters

| Label                          | Name                  | Type                                    | Description                                                 |
| -------------------------------|-----------------------|-----------------------------------------|                                                             |
|Input layer                     | `INPUT`               |[vector: line]                           | Line layer user wishes to identify junctions on             |
|ID Field                        | `ID_FIELD`            |[tablefield: any] Default: Not set       | Field containing unique identifiers on line layer           |
|ID Expression                   |`ID_EXPRESSION`        | 	[expression] Default: "CONCAT('JUNCTION_',LPAD($id,5,'00000'))"| ID expression for each junction.    |

### 3.2 Outputs


| Label                          | Name                  | Type                                    | Description                                                         |
| -------------------------------|-----------------------|-----------------------------------------|                                                                     |
|Unions                          | `OUTPUT`              |[vector: point]                          | Returns a point layer containing all identified four way junctions  |

### 3.3 Python Code

```py

import processing

processing.run("algorithm_id", {parameters_dictionary})


---
layout: single
title: "First Blog Post: Introduction to Pipeline Engineer"
date: 2025-12-13 10:00:00 +0000
categories: [news, updates]
tags: [QGIS, plugin, tutorial]
author: Pipeline Engineer Team
---

Welcome to the **Pipeline Engineer blog**! In this first post, we’ll introduce the plugin and show you how to get started.

### Features in This Release

- Automated pipeline network modelling  
- Integration with QGIS projects  
- Hydraulic calculations using `pandapipes`

### Quick Python Example

```python
from pipelineengineer import PipelineNetwork

net = PipelineNetwork("my_pipeline.json")
net.calculate()

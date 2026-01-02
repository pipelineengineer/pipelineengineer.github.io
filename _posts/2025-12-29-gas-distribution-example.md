---
title: "Gas Distribution Network Example"
date: 2025-12-29
layout: single
categories: blog
author: Thomas Raj
toc: true
---
Today we will set up a model of a gas distribution network.

![Full Model](/media/2025-12-29-gas-distribution-example/full_model.png)

Modelling a pipe network is generally a considered a worthwhile exercise. Pre-construction, the design of the network can be optimised while during service, models can help operators work proactively rather than reactively.

Today's example is based on a section of a [dataset from Northern Gas Networks](https://northerngasopendataportal.co.uk/dataset/20).

We will abstract a hydraulic model and go through some techniques we can use to validate it.

Resources produced and used in this exercise can be downloaded from [here](https://github.com/pipelineengineer/examples/tree/d797b431f4e41f0187f6847c54470dbad2ed5754/2025-12-29-gas-distribution-example).

As always, the process began by cleaning up the data for the area chosen.

## Network Cleanup

From initially playing around with this dataset, the lines did not split at each intersection.

![Junction Layer Creation](/media/2025-12-29-gas-distribution-example/unsplit_lines.png)

To fix this, the "Split With Lines" Algorithm from the processing toolbox was run.

Following this, the network was iteratively cleaned up using the [disjoint](https://pipelineengineer.github.io/features/#disjoint-check) and [loop](https://pipelineengineer.github.io/features/#loop-checker) checking algorithms from the Pipeline Engineer plugin.

![Disjoint Check](/media/2025-12-29-gas-distribution-example/disjoint_check.gif)

![Loop Check](/media/2025-12-29-gas-distribution-example/loop_check.gif)

Once cleaned up, the model was ready to be built!

## Model Setup

In order to successfully model something, we require three things:- known variables, unknown variables and a relationship between them.

For a typical pipe network problem, the variables we're trying to solve for are pressure and flow rates.

At various nodes throughout the network, either the pressure, flowrate, both or neither will be known.

The relationship between each node is derived from the pipes connecting them.

Using this, we can solve for the unknowns and have a fully capable model.

### Junctions
With our network cleaned up, we can use the model abstract the junctions from the network using Pipeline Engineer's Network Component tool, accesible from the drop down menu. 

![Component Dialog](/media/2025-12-29-gas-distribution-example/component_dialog.png)

We will select our newly cleaned alignments and adapt the layer to create junctions.

The attributes we've included for this model are p_bar, tfluid_k and name. For gases, it's generally okay to neglect height as due to the low density, height differences don't impact the hydraulics. If we were looking at a liquid service however, it's highly reccomended that height is included in the model (an example we may cover in future).

![Junction Layer Creation](/media/2025-12-29-gas-distribution-example/create_junction_layer.png)

p_bar and tfluid_k represent the initial pressure in bar and the initial temperature in Kelvin at each node respectively. Pandapipes relies on the [Newton Raphson Method](https://pandapipes.readthedocs.io/en/latest/pipeflow/pipeflow_procedure.html) to solve for the uknown variables in a pipe network, which essentially means it takes a starting value and keeps trying to find values that meet all the necessary conditions. If we were running a bidirectional or a sequential [pipeflow](https://pandapipes.readthedocs.io/en/latest/pipeflow.html), temperature would be solved for also, however, for this example, we're running a hydraulics pipeflow, which only accounts for pressure.

### Pipes
With our junctions now abstracted, we can now create our pipe layer.

The attributes we'll include on our pipe layer are the following:-
- from_junction - the junction where the pipe starts
- to_junction - the junction where the pipe ends
- length_km - the length of the pipe section in km
- k_mm - the [surface roughness](https://en.wikipedia.org/wiki/Surface_roughness) of the pipe
- in_service - this determines whether the line can be used in the model or not
- name - a unique identifier for each pipe - I added these to this network by pasting the following formula into the field calculator in QGIS:-
```sql
CONCAT('LINE_',LPAD($id+10000,5,'00000'))
```
 I then set the "Layer ID Field" dropdown in the create network component widget to the newly created field.

Additionally, from the layer, I kept the 'MATERIAL', 'DIAMETER' and 'SDR' fields, as these will help us fill out some of the fields required for this model.

We can calculate the internal diameter of each polyethylene or PE pipe using the following formula in the QGIS field calculator:-
```sql
("DIAMETER" - (2*("DIAMETER"/"SDR")))/1000
```
For the one steel line in this network, a quick google search reveals that the internal diameter of a Class 150# with a Nominal Pipe Size of DN150 is 0.1583m.

The pipe roughness for the PE and Steel pipe is set as 0.007mm and 0.045mm respectively.

Before hitting the "Adapt Existing Layer" button, ensure the Junction Layer is selected under the "Select Junction Layer" dropdown.

![Pipe Layer Creation](/media/2025-12-29-gas-distribution-example/create_pipe_layer.png)

Once all is said and done, this layer is good to go!

### Sinks

For a pipe networks, flow rates will typically be known at sources or sinks - places we're we're trying to move fluid from or to.

Sinks for this network are points where connections are made between the network and expected users.

I've chosen these points on our network from a quick visual inspection.

![House Connection](/media/2025-12-29-gas-distribution-example/house_connections.png)

We will represent these in our model using a pandapipes sink.

We will use the following parameters in our sink:-
- junction - the junction in the network that the sink is connected to
- mdot_kg_per_s - the flow rate we're expecting the sink will require (for starters, I'm just going to set this as 0.02kg/s)
- name - the name of the sink

### Grid Connections

Pressures will typically be known at points where our network connects to a larger high pressure grid.

From a quick look at the map, we can immediately see that there seem to be two points where the polyethylene network is connected to a steel line.

![Steel Connection](/media/2025-12-29-gas-distribution-example/steel_connections.png)

From the Google Street View at these locations, we can identify similar infrastructure at these two points.

![Infrastructure at Point A](/media/2025-12-29-gas-distribution-example/stonehouse_pres.png)

![Infrastructure at Point B](/media/2025-12-29-gas-distribution-example/matty_lonning_pres.png)

This infers that the two above points contain some form of pressure regulating equipment.

As such, we will identify these as our pressure boundaries.

These will be represented by an external grid in pandapipes.

We will include the following parameters:-
- junction - the junction the grid is connected to
- p_bar - the pressure at the node
- t_k - the temperature at the node
- name - the name of the grid connection
- in_service - whether the grid is in service or not.

With the grid being the final piece of the puzzle, our network is now fully constructed.

With a full model of the network, we can now solve for the unknowns we're after.

![Full Model](/media/2025-12-29-gas-distribution-example/full_model.png)

## Using the Model

### Pipeflow

With a fully set up model, one of the things we can now do to check that it works is run a pipeflow.

![Full Model](/media/2025-12-29-gas-distribution-example/run_pipeflow.png)

All settings have been kept as default, the fluid has been set as methane and the mode hydraulics.

![Pipe Flow](/media/2025-12-29-gas-distribution-example/pipeflow_results.png)

From a quick visual inpsection of the model, everything seems to be in order, meaning that we can move onto the next step.

### Time Series

One way we can quickly gain confidence that the model that's been built is useful is by comparing the outputs to recorded data. Evidently the more data we can compare the model with, the more confident we can be in the model.

For this network, while we do not have any actual data to benchmark the model with, I will show you how you can return results for a large set of data in order to validate the model created.

Included in the resources for this week's example are two csv files containing varying pressure data and varying flow rate data over a time period.

You can add these to your QGIS session like so:-

![Add Delimited Text](/media/2025-12-29-gas-distribution-example/delimited_text.png)
![Add Delimited Text Interface](/media/2025-12-29-gas-distribution-example/add_csv.png)

Before the next step, it's useful to have an excel writer installed in your QGIS environment. You can install excelwriter using the following command in the OSGeo4W shell to ensure this.

```bash
pip install excelwriter
```

Having the above step complete is not a complete show-stopper, however if done, this will compile all the results from this function into one Excel Workbook.

Once done, you can select the network as before on the pipeflow tab and fill out the time series sheet with the following settings.

![Time Series Settings](/media/2025-12-29-gas-distribution-example/time_series_settings.png)

After this is complete you will have the following:-
- Geopackages of the results of every scenario
- Results for all the components compiled into seperate CSVs
- (If excelwriter is installed) all the CSVs compiled into an Excel workbook

This data can then be easily compared with field recorded data in order to compare the model with field data.

## Conclusion

In today's example, we went from a GIS dataset to a model of a gas distribution network that we can easily benchmark with real data.

If anything was unclear or you wish to learn more, please don't hesitate to send me an email at lemungineer@gmail.com.

Thank you for reading and I'll catch you in the next one!

# Pipeline Engineer

# A Data Driven Approach to Pipeline Projects

### Features



Network Cleanup
    

Network cleanup is where it all starts. Among the everchanging landscape that is a pipeline network project, one factor that often gets left to the wayside is topographical consistency in a network. Pipeline Engineer's solution to this problem is a collection of Network Cleanup Tools.


Disjoint Check


Why not start things off with the Disjoint Check function? Disjoint Check works from a selected line (or a randomly chosen one) and slowly working outwards until every connected line within the network is selected.

Recursive Selection


Similar to Disjoint Check, Recursive Selection also selects lines that are connected to eachother. Unlike Disjoint Check, Recursive Selection will stop when lines that that don't end where the selected line(s) start.


Loop Checker


Even after Disjoint Check and Recursive Selection select every line in a layer, the network topology may not be fully rectified. Loops throw a spanner in the works, as they can be connected from more than one point, making it easy for network holes to go unnoticed.



Pipeline Engineer helps overcome this by extracting the loops identified, dissolving them and splitting them. Each section of this layer should be a continuous and smooth line - if they're not, there may be an unnoticed hole in the network.


...SNEAKY!!!


Hydraulic Modelling


Once network topology has been rectified, the rest is easy (... sort of)!!!



Pipeline Engineer leverages the high automation factor of the pandapipes fluid modelling engine to create highly configurable and visual networks of pipe networks from GIS layers. 

The attribute table of GIS data serves as the primary communicator between GIS Data and the pandapipes engine.

Component information is kept in the attribute table of a GIS layer, allowing for quick adjustments to be made to the model with new results generated rapidly. 


Model Abstraction

In a typical workflow, building a model of a network can be quite a labourious task.



With the Network Component Window, a pandapipes model of an existing network can quickly be abstracted from a network:



Running a Pipeflow

With the model abstracted, Pipeline Engineer can now run a Pipeflow. Several modes can be run, modelling both temperature and pressure. The model pictured below used the 'Hydraulics' mode, which models pressure exclusively.



Material Takeoff


With the network modeled and line sizes and specs locked in, the Material Takeoff suite of tools can be used to get quick summaries of all the Bends, Tees and Four-Way Unions within your pipe network.


### Documentation

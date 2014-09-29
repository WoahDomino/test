Rules for making a munin plugin.
================================

## Basics:
Munin plugins are composed of two major pieces:
### Part 1: Graph configuration
* Munin is going to run your script in two ways.  The presence of that 'config' argument means Munin is looking for all of the information about how you want your graph to look.
      * ./your_plugin
      * ./your_plugin config

* To configure the graph as a whole you'll need to make sure you've accounted for the following parameters:
     * The name of your graph:        print "graph_title NAME"
     * Some basic arguments:          print "graph_args --base 1000 -l 0"
     * The label for your Y axis:     print "graph_vlabel LABEL"
     * Graph category:                print "graph_category CATEGORY" (putting a new string here will cause a new category to be made)
     * A description of your graph:   print "graph_info SENTENCE" 
      
      
      



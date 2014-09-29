Rules for making a munin plugin.
================================

## Basics:
Munin plugins are composed of two major pieces:
### Part 1: Graph configuration
* Munin is going to run your script in two ways:
      * ./your_plugin
      * ./your_plugin config

      The presence of that 'config' argument means Munin is looking for all of the information about how you want your graph to look.

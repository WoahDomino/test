Rules for making a munin plugin.
================================

## Basics:

  Munin plugins are composed of two major pieces:

  ### Part 1: Graph configuration

    * Munin is going to run your script in two ways:

      ./your_plugin
      ./your_plugin config

      The presence of that 'config' argument means Munin is looking for all of the information about how you want your graph to look.

    * To configure the graph as a whole you'll need to make sure you've accounted for the following parameters:

      The name of your graph:        print "graph_title NAME"
      Some basic arguments:          print "graph_args --base 1000 -l 0"
      The label for your Y axis:     print "graph_vlabel LABEL"
      Should your graph scale?       print "graph_scale yes" (or no)
      Graph category:                print "graph_category CATEGORY" (putting a new string here will cause a new category to be made)
      A description of your graph:   print "graph_info SENTENCE"

      For more information on the basic arguments, check out this: (see munin-monitoring.org/wiki/graph_args for more information on these)

    * To configure the specific lines you're plotting:

      The visible label:                print "NAME_OF_LINE.label NAME"
      The type of line:                 print "NAME_OF_LINE.type TYPE" (Can be GAUGE, COUNTER, or DERIVE)
      A maximum size:                   print "NAME_OF_LINE.max NUMBER"
      A minimum size:                   print "NAME_OF_LINE.min NUMBER"
      A description:                    print "NAME_OF_LINE.info SENTENCE"

      Label is the only one that is absolutely required.  These can be assigned dynamically if you need.

  All of these statments are just actually printed out by the script.  Munin does the rest.  Obviously if 'print' is not what you'd use for a print statment in the language of your choice (looking at you, bash) replace it with whatever you'd use to print things.



  ### Part 2: Actual logic

    It's here that you do whatever you need to do to give munin some values to graph.  Query redis keys, read log files, generate random numbers, whatever you feel like doing.  The very last thing your script should do is assign the .value field of each line.

    Some examples might be:
      puts "LINE.value 42"

      print "LINE.value {0}".format(x)

      echo -n "LINE.value "
      echo $total

    It's pretty flexible with how you do it, as long as it looks right in the end.

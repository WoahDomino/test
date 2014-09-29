## Troubleshooting:

  Did you wait for munin to update?  5 minutes feels like much longer than it has any right to be.  Alternately be impatient and follow these steps.

  0. Restart the master and apache (in that order).
      > sudo systemctl restart munin-node
      > sudo systemctl restart httpd

  1. Test that running your script locally works.  This involves both running it just as a file, and munin-run filename.
        If this breaks, your plugin isn't working correctly

  2. Repeat step one but with the config parameter: "munin-run plugin config" and check for extra print statments lingering.  The only thing you should be printing out is graph related stuff.  Extra print statements will break things.

  3. Test to see that the master node can run the plugin:
      > ssh into 10.40.1.30 (the master node)
      > telnet your.ip.address 4949
        >fetch plugin

    If this doesn't return the same thing that munin-run plugin did, your problem is in master's ability to see the plugin.  Problems here could come from a bunch of different places.

  4. If all that worked, things get serious.  You'll need to: , stop munin running, su into munin, and run the update in debug mode, looking for errors.

    On the munin master...

      disable the cron job that updates munin
        comment out the only cron job in /etc/cron.d/munin

      stop munin running:
        run sudo systemctl stop munin-node
        run ps aux | grep "munin-cron" and kill any leftover processes if there are any

      su into munin:
        sudo su -s /bin/bas/ munin

      run the update in debug mode
        clear out your terminal.  You're going to get a *lot* of output
        run: /usr/bin/munin-cron --debug 2> a_file.txt (or be like me and just wait for it to finish and copy paste into sublime)
        give it a minute or two

    Search for the words "FATAL" and "ERROR" in all this output.
    No luck?  Search for the name of your munin node: "domain;subdomain;nodename:plugin_name"
      (Here's an example: "aws;elasticsearch;largecluster;master1:processes")
      If nothing shows up, regardless of what telnet told you, master isn't recognizing the node.

  5. I've never had to go beyond that.  If you do, check out what munin has to say on it first: http://munin-monitoring.org/wiki/PluginDebugging




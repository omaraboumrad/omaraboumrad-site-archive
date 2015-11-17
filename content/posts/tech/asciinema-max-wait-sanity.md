Title: Asciinema --max-wait sanity
Slug: asciinema-max-wait-sanity
Date: 2015-04-21 14:06:18
Tags: tips
Summary: Making Asciinema work with commands that incrementally report progress
Status: Published
Schema: Article
Keywords: asciinema, max, wait, terminal, progress

Asciinema 1.0 added an amazing new switch to the command line interface that allows you to set a terminal's maximum inactivity time.

The switch is: *--max-wait [seconds]*

You can refer to the release post on asciinema blog [here](http://blog.asciinema.org/post/one-point-o/)

Terminal inactivity refers to output and not how long it took a process to complete which makes this switch not function properly for commands that incrementally report progress.

**Example**

    $ asciinema rec local.json --max-wait 2
    $ sudo apt-get update
       ...
       Get:7 http://archive.ubuntu.com trusty/universe Sources [7926 kB]
       10% [5 Packages 300kB/7589 kB 2%] 400kB/s 1s

Even though the above works perfectly fine if your command has progress notification (similar to the *apt-get*), *--max-wait* will be unable to do its job properly.

The obvious solution would be to just redirect the output of a command to */dev/null* though this will completely drop the output which you don't want to lose.

    $ asciinema rec local.json --max-wait 2
    $ sudo apt-get update > /dev/null

A better solution is to find a switch for the command you're trying to run that can suppress the progress only. In the *apt-get* case you can resort to the *-q* switch as seen below.

    $ asciinema rec local.json --max-wait 2
    $ sudo apt-get update -q
    ...
    Get:7 http://archive.ubuntu.com trusty/universe Sources [7926 kB]


The command will no longer report any progress, allowing Asciinema's *--max-wait* to function as intended when playing back a recording.




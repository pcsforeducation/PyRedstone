==========
PyRedstone
==========

Copyright 2012, Josh Gachnang, Josh@ServerCobra.com.

PyRedstone is a Minecraft server wrapper for both vanilla and CraftBukkit servers. It allows you to start/stop the server, manage players, run all in game commands, manage settings and logs, manage CraftBukkit plugins, and a lot more.
PyRedstone also includes a remote API server based on CherryPy that accepts and returns JSON. A reference implementation of a client is included.
The code can be found on `GitHub <https://github.com/pcsforeducation/pyredstone>`_. All issues and pull requests are welcome!

Requirements
============
PyRedstone requires Python 2.7. It has only been tested on Ubuntu, specifically Ubuntu Server 12.04 x64. Additional testing is welcome, and all bugs will be considered. The only Python 2.7 features it relies on are logging.config.dictConfig and OrderedDict.
You must also have Tmux installed. To install it (on Ubuntu), run::
    sudo apt-get update
    sudo apt-get install -y tmux

PyRedstone also relies on the following Python packages:

* cherrypy

* nbt

* MAGPlus

pyredstone.py
=============
PyRedstone can be run from the commandline like so::

    python pyredstone.py --config /path/to/config COMMAND ARG1 ARG2...

I have also included a file called redstone.py, which provides the same functionality, but is installed to the /usr/bin, so you can run it from anywhere like so::

    redstone.py --config /path/to/config COMMAND ARG1 ARG2...

server.py
=========
The server can be started from the commandline or an init script. A sample init script is include in the package and on Github::

    python server.py --config /path/to/config

**Note** The server will attempt to write a PID file to /var/run/pyredstone. The directory must exist and be writable by the user starting the server (root if started by init script).

The server accepts JSON in the following format::

    {"username": "USERNAME", "auth_token": "TOKEN", "action": "ACTION", "args": {"arg1": "ARG", "args": "ARG"}}

**Note** Username and auth_token are not implemented yet. They will likely be saved in the config file.

Config File
===========
The config file is a standard, INI style config file. An example is included called example.cfg. The format should be as follows::

    [ServerName]
    session_name = troydoesntknow
    minecraft_dir = /home/josh/minecraft/
    server_jar = minecraft.jar
    backup_dir = /tmp
    mapper = overviewer
    memory_min = 512
    memory_max = 1024
    java_args = -XX:+AggressiveOpts
    debug = False
    stable_releases = False

The variables are:

* session_name: The name of the Tmux session that will be used.

* minecraft_dir: The path to the directory containing *server_jar* and server.properties.

* server_jar: The name of the actual Minecraft server jar. The vanilla server is usually minecraft.jar, while CraftBukkit is usually craftbukkit.jar.

* backup_dir: Where to put backup files. Not currently used.

* mapper: The mapper software. Not currently used.

* memory_min: Starting memory to be used by the server in megabytes.

* memory_max: Maximum memory to be used by the server in megabytes.

* java_args: Additional java_args to be executed when starting the server.

* debug: Changes the level of output. True will print all debug messages. False will hide info and debug levels from the console, but still log to the files.

Ubuntu Startup Scripts
======

bin/init.d/minecraft
---------
A standard init wrapper around PyRedstone. It gives the standard '/etc/init.d/minecraft start' interface for services. Acceptable commands are start, stop, restart, update, backup, status, and command. You need to customize USERNAME and CONFIG variables.

bin/init.d/redstone_server
---------------
The redstone_server is an init wrapper for server.py. It allows you to start and stop server.py with the server. Acceptable commands are start, stop, restart, and status. You need to customize the USERNAME and CONFIG variables.

Changes
======
* v0.2.0, 08/10/2012 -- Adding support for Minecraft 1.3, added server_stats, added lots of debugging, improved server for batch requests. 
* v0.1.2, 07/15/2012 -- Adding ability to update.
* v0.0.2, 07/10/2012 -- Initial release.



---
layout: post
title: supervisor 
---

http://supervisord.org/running.html

### the simple way to use suepervisor


* Complete Jekyll setup included (layouts, config, [404](/404), [RSS feed](/atom.xml), posts, and [example page](/about))
* Mobile friendly design and development
* Easily scalable text and component sizing with `rem` units in the CSS
* Support for a wide gamut of HTML elements
* Related posts (time-based, because Jekyll) below each post
* Syntax highlighting, courtesy Pygments (the Python-based code snippet highlighter)

### Install 
pip install supervisor

### create default config 
echo_supervisord_conf > supervisord.conf

###supervisord(start)  
supervisord -n -c supervisor.conf 


###supervisorctl (controller)
supervisorctl status/stop all/shutdown 


###add program 
```
[program:foo]
command=/bin/cat              ; the program (relative uses PATH, can take args)

[program:goagent]
command=/usr/local/bin/python proxy.py              ; the program (relative uses PATH, can take args)
directory=/Users/winfan/opt/goagent/local                ; directory to cwd to before exec (def no cwd)

```

supervisorctl reload


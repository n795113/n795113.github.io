---
title: "Maya API: Hello world!"
date: 2022-06-16 23:17:33
categories: [Tech, Maya]
tags:
  - maya
  - python
---
Write a custom Maya command that print "Hello world!" in Maya console.

```python
import sys
import maya.api.OpenMaya as api

# Must include to use Maya API 2.0
def maya_useNewAPI():
    pass

class HelloWorldCmd(api.MPxCommand):
    kCmdName = 'helloWorld'

    def __init__(self):
        api.MPxCommand.__init__(self)

    @staticmethod
    def creator():
        return HelloWorldCmd()

    def doIt(self, arg_list):
        print('Hello world!')

# initialize and uninitialize functions are
# needed to be loaded by Maya Plug-in Manager 
def initializePlugin(mobject):
    fn_plugin = api.MFnPlugin(mobject)
    try:
        fn_plugin.registerCommand(
            HelloWorldCmd.kCmdName,
            HelloWorldCmd.creator
        )
    except:
        sys.stderr.write("Fail to register plugin: " + HelloWorldCmd.kCmdName)

def uninitializePlugin(mobject):
    fn_plugin = api.MFnPlugin(mobject)
    try:
        fn_plugin.deregisterCommand(
            HelloWorldCmd.kCmdName
        )
    except:
        sys.stderr.write("Fail to deregister plugin: " + HelloWorldCmd.kCmdName)
```

Open Maya Plug-in Manager, and hit "Browse" to load our command.
{% asset_img plugin-manager.png 500 %}
Then we can run our command
```mel
helloworld;
```

We can also call it in Python:
```python
from maya import cmds
cmds.helloWorld
```
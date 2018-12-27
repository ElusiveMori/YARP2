# YARP2

YARP was a freeform sandbox roleplay custom map, in the vein of RotRP and SotDRP. It was coded using vJass, an extension of the JASS language used by WC3. YARP2 seeks not only to implement the same features as YARP, but to extend those features and add new ones.

Since vJass is just an extension of the JASS scripting language, it carries many of the same limitations that can make development slow and cubersome. This was one of the main reasons that the development of YARP was ceased. YARP2 seeks to fix this by being written in Wurst, a more recent language that compiles into JASS and makes many more recent programing language constructions avaiable to map coders.


## Setting up

First, download the WurstSetup tool from https://wurstlang.org/.

1. clone into git@github.com:SamuelMoriarty/YARP2.git
2. Import the cloned folder using the WurstSetup tool and click on "Update Project"
3. cd into _build/dependencies/wurstStdlib2
4. git remote add mori-std git@github.com:SamuelMoriarty/WurstStdlib2.git
5. git fetch --all
6. git checkout yarp-fork
7. open vs code, press ctrl-shift-p, type "runmap", the wurst run map command should pop up, try running it

If you correctly completed these steps, the map should compile and open. Don't forget to specify the path to your wc3 installation in the WurstSetup tool.

# YARP2

YARP was a freeform sandbox roleplay custom map, in the vein of RotRP and SotDRP. It was coded using vJass, an extension of the JASS language used by WC3. YARP2 seeks not only to implement the same features as YARP, but to extend those features and add new ones.

Since vJass is just an extension of the JASS scripting language, it carries many of the same limitations that can make development slow and cubersome. This was one of the main reasons that the development of YARP was ceased. YARP2 seeks to fix this by being written in Wurst, a more recent language that compiles into JASS and makes many more recent programing language constructions avaiable to map coders.

The YARP community is a group of like-minded roleplayers who enjoy sandbox-style roleplay. We have our own **Discord** server, and you are [invited!](https://discord.gg/TYRKMNf "Discord Invitation")


## Setting up

First, download the WurstSetup tool from https://wurstlang.org/.

1. clone into git@github.com:SamuelMoriarty/YARP2.git
2. Import the cloned folder using the WurstSetup tool and click on "Update Project"
3. cd into _build/dependencies/wurstStdlib2
4. `git remote add mori-std git@github.com:SamuelMoriarty/WurstStdlib2.git`
5. `git fetch --all`
6. `git checkout yarp-fork`
7. open VS Code, press ctrl-shift-p, type "runmap", the wurst run map command should pop up, try running it

If you correctly completed these steps, the map should compile and open. Don't forget to specify the path to your wc3 installation in the WurstSetup tool.

### Custom Imports
YARP2 uses many custom models and textures. However, they are not included in the GitHub repository, as they are binary files. You can find them [here](https://drive.google.com/uc?export=download&id=1ftuvctVGbd7WEi-hdM5rPlOed0qWFsfU "Yarp Resources").

Simply extract the contents of the zip file to the path `imports/` inside of the repository.

## Contributing

Working with more people is a great way of getting things done faster. If you would like to help in YARP2's development and get it done faster and with more content to boot, don't hesistate to get in contact if you have experience with the area you want to help with. There are two main ways to contribute, outlined below:

### Modding work:
* **Code**: Mentorship and advice are included. Disclaimer: YARP uses Wurst, not GUI triggers or JASS/vJASS. GUI experience is largely inapplicable here.
* **Organizing Object Data**: This has historically been one of the biggest pain points for development. YARP has a LOT of object data - units, buildings, builders, shops. This does not require you to use WorldEdit or it's abhorrent object editor. It uses a custom file format which is easy to read and edit. Specifc areas where help is needed: shop positions, organizing existing objects better, adding new content.
* **Terrains**: Saken and Marnin are already helping me with terraining, but more terrains is always better. YARP needs something original, something quality to ship with. If you'd like to make a new terrain specifically for YARP, it'll gladly be used.

### Community work:
* **Promotion**: This is something that anyone can do - host lobbies during active hours, make campaigns, invite people and friends. Please. This is important.
* **Website:** However, having a proper website for YARP would be really, really nice. If you have experience with web development and would like to help, please get in touch!

There's also a contribution guide, which details requirements for code contribution, as well as some pointers with the project.

[Contribtuion Guide](CONTRIBUTING.md)
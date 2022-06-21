# Fabric-Loader Loading Phases Visualizer for Minecraft

A plain-javascript single-page tool which parses the log4j formatted /log/debug.log file from a Minecraft Modpack. Based on the timestamps of various key log messages, calculates the time between phases and displays it on a bar graph.

![alt](https://i.imgur.com/ynZy5Zg.png)

[Dummy debug.log](https://www.dropbox.com/s/cd8a2ztmp3un0uv/debug.log?dl=1)

# Modifying

A simple string.contains() is performed on every line of the log. Target matches are in `messagesToMatch` in [index.html](https://github.com/Shadowrs/minecraft-fabric-loader-benchmark-visualizer/blob/e6e608932f8889cf888b30a3fe73a600116fadff/index.html#L56) to add a new entry to the bar graph.

Disclaimer: some logs such as `(FabricLoader/Knot) Setting up languages.` are not present in the public Fabric-Loader repo, they are custom added in a fork for testing. To use your own build of fabric-loader: 

- Use MultiMC/PolyMC (not sure if its possible to define your own modloader jar in curseforge/ATLauncher/GDLauncher)
- Install the modpack, for example All Of Fabric 5 (AOF5) from [CurseForge](https://www.curseforge.com/minecraft/modpacks/all-of-fabric-5)
- On the UI, click Customize > Edit. This opens `C:\Users\xx/AppData\Roaming\PolyMC\instances\All of Fabric 5 - AOF5 - 1.18.2\patches/net.fabricmc.fabric-loader.json` in a text editor.
- Change the fabric-loader section to your own maven repo like so:

```
{
            "name": "net.fabricmc:fabric-loader:0.14.8+local",
            "url": "https://repo.repsy.io/mvn/username/reponame/"
        }
```

- Add the Java Arguments to enable log4j logging - see below
- Optionally, if compatible with the mods in the modpack, manually add the [DashLoader](https://github.com/alphaqu/DashLoader) mod to launch x2 faster.

# Prerequisits to generate debug.log

Modify launch options to include Java args: `-Dfabric.log.debug.level=DEBUG -Dfabric.log.level=DEBUG -Dlog4j.configurationFile=B:\log4j.xml`

In the AOF5 1.3.0 modpack, minecraft spams the log with 70k lines of warnings. I reccomend using [this](https://www.dropbox.com/s/2vun33lx27ls79o/log4j.xml?dl=0) log4j.xml format which surpresses the class, or you can add the surpression yourself to your own log4j.xml:

```
<Loggers>
		<Logger level="ERROR" name="net.minecraft.class_3294"/>
	</Loggers>
```


Thanks to:

[![alt-text](https://i.imgur.com/OKTTrCm.png)](https://plotly.com/javascript/) for the graph generation.

[![alt-text](https://i.imgur.com/efkLewS.png)](https://day.js.org/) for date timestamp parsing.


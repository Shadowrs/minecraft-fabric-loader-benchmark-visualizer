# Fabric-Loader Loading Phases Visualizer for Minecraft

A plain-javascript single-page tool which parses the log4j formatted /log/debug.log file from a Minecraft Modpack. Based on the timestamps of various key log messages, calculates the time between phases and displays it on a bar graph.

![alt](https://i.imgur.com/kKUKMKR.png)

[Give it a go](https://shadowrs.github.io/minecraft-fabric-loader-benchmark-visualizer/) using a [dummy debug.log](https://www.dropbox.com/s/cd8a2ztmp3un0uv/debug.log?dl=1)

# Supplying your own debug.log

To run this tool on your own debug.log, you must use PolyMC or the MultiMC Launcher to run Minecraft.
- On the Launcher UI, click Customize > Edit. This opens `C:\Users\xx/AppData\Roaming\PolyMC\instances\All of Fabric 5 - AOF5 - 1.18.2\patches/net.fabricmc.fabric-loader.json` in a text editor. This file allows you to provide overrides for fabricMC's jars with Forks of each library which include extra logging information printed to debug.log required to identify when a phase begins/ends.
- Change the fabric-loader and sponge-mixin sections. 

```
        {
            "name": "net.fabricmc:sponge-mixin:0.11.4+mixin.0.8.5-local",
            "url": "https://repo.repsy.io/mvn/tardisfan/uno/"
        },
        {
            "name": "net.fabricmc:fabric-loader:0.14.8+local",
            "url": "https://repo.repsy.io/mvn/tardisfan/uno/"
        }
```

- Modify launch options to include Java args: `-Dfabric.log.debug.level=DEBUG -Dfabric.log.level=DEBUG`

- Optionally, if compatible with the mods in the modpack you're going to run, manually add the [DashLoader](https://github.com/alphaqu/DashLoader) mod to launch x2 faster. DashLoader replaces some of the last Minecraft phases of loading (related to models) with heavily optimized caching. 



# How it works

A simple string.contains() is performed on every line of the log. Patterns of unique log messages to matches are in `messagesToMatch` in [index.html](https://github.com/Shadowrs/minecraft-fabric-loader-benchmark-visualizer/blob/e6e608932f8889cf888b30a3fe73a600116fadff/index.html#L56). 
The timestamp of each log message is parsed. All matches are sorted by timestamp, and the duration of each phase is `nextPhase.start - thisPhase.start`



# Thanks to

[![alt-text](https://i.imgur.com/OKTTrCm.png)](https://plotly.com/javascript/) for the graph generation.

[![alt-text](https://i.imgur.com/efkLewS.png)](https://day.js.org/) for date timestamp parsing.


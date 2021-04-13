# Mixin-Vanilla-Client
Using Mixins to Mod Minecraft Vanilla.

# Setup
Download the build.gradle in this repository and open it in IntelliJ IDEA.

Before you do anything, make sure that you run the `wrapper` task, and go into gradle-wrapper.properties and change the number to 4.7. ForgeGradle doesn't work with newer versions of Gradle.

Now, edit the following:

```gradle

version = "0.1"
group = "com.exampleClient"
archivesBaseName = "ExampleClient"

```

`version` is the version of your client, pretty self explanatory.
`group` is your base package.
`archivesBaseName` should be the name of your Client.

```gradle
minecraft {
    version = "1.8.9"
    tweakClass = "cf.duhc.client.launch.DuckTweaker"
    mappings = "stable_22"
    runDir = 'run'
    clientJvmArgs = ["-XX:-DisableExplicitGC"] // fast world loading
    makeObfSourceJar = false
}
```

You can leave everything the same except for `tweakClass`.

```gradle
mixin {
    defaultObfuscationEnv notch
    add sourceSets.main, "mixin.exampleClient.refmap.json"
}
```

change `exampleClient` to your client name.

```gradle
manifest.attributes(
		"MixinConfigs": 'mixins.exampleClient.json',
		"TweakClass": 'com.exampleClient.launch.ExampleClientTweaker',
		"TweakOrder": 0,
		"Manifest-Version": 1.0
)
```

Change the MixinConfigs to your .json file and change your TweakClass to your Tweaker.
You're all set!

# How to Use Mixins

COMING SOON
 

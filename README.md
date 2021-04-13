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

# How to Create a Tweaker

(NOTE: This is taken from Hyperium.)

Create a class (whatever name you want) and implement ITweaker. Look at the code below.

```java
public class ClientTweaker implements ITweaker {

    private static final Logger logger = LogManager.getLogger();

    private ArrayList<String> args = new ArrayList<>();

    @Override
    public void acceptOptions(List<String> args, File gameDir, final File assetsDir, String profile) {
        this.args.addAll(args);

        addArg("gameDir", gameDir);
        addArg("assetsDir", assetsDir);
        addArg("version", profile);
    }

    @Override
    public String getLaunchTarget() {
        return "net.minecraft.client.main.Main";
    }

    @Override
    public void injectIntoClassLoader(LaunchClassLoader classLoader) {
        logger.info("Initializing Bootstraps...");
        MixinBootstrap.init();
        logger.info("Adding mixin configuration...");
        MixinEnvironment environment = MixinEnvironment.getDefaultEnvironment();
        Mixins.addConfiguration("mixins.example.json");

        if (environment.getObfuscationContext() == null) {
            environment.setObfuscationContext("notch"); // Switch's to notch mappings
        }

        environment.setSide(MixinEnvironment.Side.CLIENT);
    }

    @Override
    public String[] getLaunchArguments() {
        return args.toArray(new String[]{});
    }

    private void addArg(String label, Object value) {
        args.add("--" + label);
        args.add(value instanceof String ? (String) value : value instanceof File ? ((File) value).getAbsolutePath() : ".");
    }
}
```
 
The main part we want to focus on is the function `injectIntoClassLoader`.

We are first initializing the MixinBootstrap. Then, we are adding our Mixin configuration. We are also switching the obfuscation context to notch's mappings.

# Resources
More info on Mixins can be found with these links:
Javadocs: https://jenkins.liteloader.com/view/Other/job/Mixin/javadoc/ (The javadocs, pretty self explanatory)
Wiki: https://github.com/SpongePowered/Mixin/wiki (great for learning what Mixins even are)
Discord: https://discord.gg/sponge (Get help with Mixins here)
Cheatsheet: https://github.com/2xsaiko/mixin-cheatsheet (Nice place that can explain things a bit easier with better examples)

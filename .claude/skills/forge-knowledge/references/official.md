### Run Minecraft Server with ForgeGradle

Source: https://docs.minecraftforge.net/en/1.20.1/gettingstarted

Invokes the runServer task using gradlew to start a dedicated Minecraft server with Forge loaded. This task is useful for server-side mod testing and development. It can be used with various IDEs or directly from the terminal.

```bash
gradlew runServer
```

--------------------------------

### Run Minecraft Client with ForgeGradle

Source: https://docs.minecraftforge.net/en/1.20.1/gettingstarted

Invokes the runClient task using gradlew to start the Minecraft client with Forge loaded. This is a common task for testing mods during development. It can be used with various IDEs or directly from the terminal.

```bash
gradlew runClient
```

--------------------------------

### Create Custom ModelProvider Example

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Provides an example of a custom `ModelProvider` subclass, `ExampleModelProvider`. It shows how to initialize the provider with output, modid, subdirectory, and the `ModelBuilder` factory method, enabling the generation of models in a specified location.

```java
public class ExampleModelProvider extends ModelProvider<ExampleModelBuilder> {

  public ExampleModelProvider(PackOutput output, String modid, ExistingFileHelper existingFileHelper) {
    // Models will be generated to 'assets/<modid>/models/example' if no 'modid' is specified in '#getBuilder'
    super(output, modid, "example", ExampleModelBuilder::new, existingFileHelper);
  }
}
```

--------------------------------

### Create Custom Loader Builder Example

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Provides an example of a custom loader builder class, `ExampleLoaderBuilder`, which extends `CustomLoaderBuilder`. It includes a static factory method `begin` and a protected constructor for initialization. This class is designed to be extended for specific loader configurations.

```java
public class ExampleLoaderBuilder<T extends ModelBuilder<T>> extends CustomLoaderBuilder<T> {
  public static <T extends ModelBuilder<T>> ExampleLoaderBuilder<T> begin(T parent, ExistingFileHelper existingFileHelper) {
    return new ExampleLoaderBuilder<>(parent, existingFileHelper);
  }

  protected ExampleLoaderBuilder(T parent, ExistingFileHelper existingFileHelper) {
    super(new ResourceLocation(MOD_ID, "example_loader"), parent, existingFileHelper);
  }
}
```

--------------------------------

### Setup Eclipse Development Environment for Forge

Source: https://docs.minecraftforge.net/en/1.20.1/forgedev

These commands and steps are used to set up a Forge development environment within Eclipse. They involve running Gradle tasks to prepare the project and then importing it into your Eclipse workspace.

```shell
./gradlew setup
./gradlew genEclipseRuns
```

--------------------------------

### DifferenceIngredient Example (JSON)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/ingredients

Provides an example of the DifferenceIngredient, which implements set subtraction. The input stack must match the 'base' ingredient but must NOT match the 'subtracted' ingredient.

```json
// For some input
{
  "type": "forge:difference",
  "base": {
    // Ingredient the stack is in
  },
  "subtracted": {
    // Ingredient the stack is NOT in
  }
}
```

--------------------------------

### Access Transformer Syntax Examples

Source: https://docs.minecraftforge.net/en/1.20.1/advanced/accesstransformers

These examples illustrate the syntax for applying Access Transformers to classes, fields, and methods. They cover changing access modifiers, removing the 'final' modifier, and specifying method parameters and return types using JVM descriptor notation.

```accesstransformer
# Makes public the ByteArrayToKeyFunction interface in Crypt
public net.minecraft.util.Crypt$ByteArrayToKeyFunction
```

```accesstransformer
# Makes protected and removes the final modifier from 'random' in MinecraftServer
protected-f net.minecraft.server.MinecraftServer f_129758_ #random
```

```accesstransformer
# Makes public the 'makeExecutor' method in Util,
# accepting a String and returns an ExecutorService
public net.minecraft.Util m_137477_(Ljava/lang/String;)Ljava/util/concurrent/ExecutorService; #makeExecutor
```

```accesstransformer
# Makes public the 'leastMostToIntArray' method in UUIDUtil,
# accepting two longs and returning an int[]
public net.minecraft.core.UUIDUtil m_235872_(JJ)[I #leastMostToIntArray
```

--------------------------------

### EntityLootSubProvider Constructor Example (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/loottables

This example demonstrates initializing an `EntityLootSubProvider` subclass. It configures the provider with all available feature flags, controlling whether loot tables are generated for enabled entity types.

```java
// In some EntityLootSubProvider subclass
public MyEntityLootSubProvider() {
  super(FeatureFlags.REGISTRY.allFlags());
}
```

--------------------------------

### Reading Profiler Results (Example)

Source: https://docs.minecraftforge.net/en/1.20.1/misc/debugprofiler

This snippet demonstrates the typical output format of a Minecraft Forge profiling result file ('profiling.txt'). It shows hierarchical data representing time spent in various game systems and entities, with percentages indicating time relative to the parent section and the total tick duration.

```text
[00] levels - 96.70%/96.70%
[01] |   Level Name - 99.76%/96.47%
[02] |   |   tick - 99.31%/95.81%
[03] |   |   |   entities - 47.72%/45.72%
[04] |   |   |   |   regular - 98.32%/44.95%
[04] |   |   |   |   blockEntities - 0.90%/0.41%
[05] |   |   |   |   |   unspecified - 64.26%/0.26%
[05] |   |   |   |   |   minecraft:furnace - 33.35%/0.14%
[05] |   |   |   |   |   minecraft:chest - 2.39%/0.01%
```

--------------------------------

### Profiling Custom Code with ProfilerFiller

Source: https://docs.minecraftforge.net/en/1.20.1/misc/debugprofiler

This Java code example shows how to manually profile custom code sections using the ProfilerFiller class. By pushing and popping sections, you can measure the execution time of specific operations and identify them in the profiler results. The ProfilerFiller instance can be obtained from Level, MinecraftServer, or Minecraft.

```java
ProfilerFiller#push("yourSectionName : String");
//The code you want to profile
ProfilerFiller#pop();
```

--------------------------------

### PartialNBTIngredient Example (JSON)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/ingredients

Shows how to use PartialNBTIngredient for a more flexible NBT comparison. It allows matching against a single item or a list of items and checks only specified keys within the NBT share tag, ignoring others.

```json
// For some input
{
  "type": "forge:partial_nbt",

  // Either 'item' or 'items' must be specified
  // If both are specified, only 'item' will be read
  "item": "examplemod:example_item",
  "items": [
    "examplemod:example_item",
    "examplemod:example_item2"
    // ...
  ],

  "nbt": {
    // Checks only for equivalency on 'key1' and 'key2'
    // All other keys in the stack will not be checked
    "key1": "data1",
    "key2": {
      // Data 2
    }
  }
}
```

--------------------------------

### Minecraft Forge Screen Rendering

Source: https://docs.minecraftforge.net/en/1.20.1/gui/screens

Provides an example of rendering a Minecraft Forge screen. The #render method handles drawing the background, widgets, and tooltips. It's essential to call super.render() to ensure widgets are drawn correctly.

```java
// In some Screen subclass

// mouseX and mouseY indicate the scaled coordinates of where the cursor is in on the screen
@Override
public void render(GuiGraphics graphics, int mouseX, int mouseY, float partialTick) {
    // Background is typically rendered first
    this.renderBackground(graphics);

    // Render things here before widgets (background textures)

    // Then the widgets if this is a direct child of the Screen
    super.render(graphics, mouseX, mouseY, partialTick);

    // Render things after widgets (tooltips)
}
```

--------------------------------

### Example Custom Model Consumer Provider (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Demonstrates the creation of a custom `IDataProvider` that utilizes a `ModelProvider` subclass to generate custom models. It requires `PackOutput`, `modid`, and `ExistingFileHelper` for initialization. The output is the populated model provider.

```java
public class ExampleModelConsumerProvider implements IDataProvider {

  public ExampleModelConsumerProvider(PackOutput output, String modid, ExistingFileHelper existingFileHelper) {
    this.example = new ExampleModelProvider(output, modid, existingFileHelper);
  }
}
```

--------------------------------

### Generate Visual Studio Code Run Configurations with ForgeGradle

Source: https://docs.minecraftforge.net/en/1.20.1/gettingstarted

Executes the getVSCodeRuns task to generate run configurations for Visual Studio Code. This task is essential for using VS Code with ForgeGradle projects.

```bash
gradlew getVSCodeRuns
```

--------------------------------

### Game Test Batching Setup and Teardown (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Demonstrates how to use `@BeforeBatch` and `@AfterBatch` annotations to set up and tear down test environments for a batch of Game Tests. Batch methods accept a ServerLevel object.

```java
public class ExampleGameTests {
  @BeforeBatch(batch = "firstBatch")
  public static void beforeTest(ServerLevel level) {
    // Perform setup
  }

  @GameTest(batch = "firstBatch")
  public static void exampleTest2(GameTestHelper helper) {
    // Do stuff
  }
}
```

--------------------------------

### Clone Minecraft Forge Repository

Source: https://docs.minecraftforge.net/en/1.20.1/forgedev

This command clones your forked Minecraft Forge repository to your local machine. Ensure you replace '<User>' with your GitHub username. This is a fundamental step for obtaining a local copy of the codebase.

```shell
git clone https://github.com/<User>/MinecraftForge
```

--------------------------------

### Generate Eclipse Run Configurations with ForgeGradle

Source: https://docs.minecraftforge.net/en/1.20.1/gettingstarted

Executes the genEclipseRuns task to generate necessary run configurations for developing mods in Eclipse. This task is part of the ForgeGradle build system.

```bash
gradlew genEclipseRuns
```

--------------------------------

### Game Test Template Location Examples (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Provides examples of how class and method annotations like `@GameTest`, `@GameTestHolder`, and `@PrefixGameTestTemplate` influence the structure template location for Game Tests. This includes specifying namespaces, class name prepending, and template names.

```java
// Modid for all structures will be MODID
@GameTestHolder(MODID)
public class ExampleGameTests {

  // Class name is prepended, template name is not specified
  // Template Location at 'modid:examplegametests.exampletest'
  @GameTest
  public static void exampleTest(GameTestHelper helper) { /*...*/ }

  // Class name is not prepended, template name is not specified
  // Template Location at 'modid:exampletest2'
  @PrefixGameTestTemplate(false)
  @GameTest
  public static void exampleTest2(GameTestHelper helper) { /*...*/ }

  // Class name is prepended, template name is specified
  // Template Location at 'modid:examplegametests.test_template'
  @GameTest(template = "test_template")
  public static void exampleTest3(GameTestHelper helper) { /*...*/ }

  // Class name is not prepended, template name is specified
  // Template Location at 'modid:test_template2'
  @PrefixGameTestTemplate(false)
  @GameTest(template = "test_template2")
  public static void exampleTest4(GameTestHelper helper) { /*...*/ }
}
```

--------------------------------

### Defining Sounds with SoundDefinitionsProvider

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/sounds

Example of adding sound definitions using `SoundDefinitionsProvider`. It shows how to use the `add`, `definition`, and `sound` methods to specify sound names, subtitles, and individual sound properties like weight, volume, pitch, and streaming.

```java
// In some SoundDefinitionsProvider#registerSounds
this.add(EXAMPLE_SOUND_EVENT, definition()
  .subtitle("sound.examplemod.example_sound") // Set translation key
  .with(
    sound(new ResourceLocation(MODID, "example_sound_1")) // Set first sound
      .weight(4) // Has a 4 / 5 = 80% chance of playing
      .volume(0.5), // Scales all volumes called on this sound by half
    sound(new ResourceLocation(MODID, "example_sound_2")) // Set second sound
      .stream() // Streams the sound
  )
);

this.add(EXAMPLE_SOUND_EVENT_2, definition()
  .subtitle("sound.examplemod.example_sound") // Set translation key
  .with(
    sound(EXAMPLE_SOUND_EVENT.getLocation(), SoundType.EVENT) // Adds sounds from 'EXAMPLE_SOUND_EVENT'
      .pitch(0.5) // Scales all pitches called on this sound by half
  )
);
```

--------------------------------

### Example mods.toml Configuration for Forge

Source: https://docs.minecraftforge.net/en/1.20.1/gettingstarted/modfiles

This TOML file configures a Minecraft Forge mod, specifying loader details, license, and mod-specific information like ID, version, display name, and dependencies on Forge and Minecraft.

```toml
modLoader="javafml"
loaderVersion="[46,)"

license="All Rights Reserved"
issueTrackerURL="https://github.com/MinecraftForge/MinecraftForge/issues"
showAsResourcePack=false

[[mods]]
  modId="examplemod"
  version="1.0.0.0"
  displayName="Example Mod"
  updateJSONURL="https://files.minecraftforge.net/net/minecraftforge/forge/promotions_slim.json"
  displayURL="https://minecraftforge.net"
  logoFile="logo.png"
  credits="I'd like to thank my mother and father."
  authors="Author"
  description='''
  Lets you craft dirt into diamonds. This is a traditional mod that has existed for eons. It is ancient. The holy Notch created it. Jeb rainbowfied it. Dinnerbone made it upside down. Etc.
  '''
  displayTest="MATCH_VERSION"

[[dependencies.examplemod]]
  modId="forge"
  mandatory=true
  versionRange="[46,)"
  ordering="NONE"
  side="BOTH"

[[dependencies.examplemod]]
  modId="minecraft"
  mandatory=true
  versionRange="[1.20]"
  ordering="NONE"
  side="BOTH"

```

--------------------------------

### Set Mod Version in build.gradle

Source: https://docs.minecraftforge.net/en/1.20.1/gettingstarted

Defines the version of the mod in the build.gradle file. It is recommended to use a variation of Maven versioning for consistent version management.

```gradle
version = '1.20-1.0.0.0'
```

--------------------------------

### Register Objects with BootstapContext

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/datapackregistries

This example shows how to register specific objects, like `ConfiguredFeature`, using the `#register` method within the `BootstapContext`. It defines a `ResourceKey` for the object and then creates an instance of the object, such as an `OreConfiguration`, to be registered.

```java
public static final ResourceKey<ConfiguredFeature<?, ?>> EXAMPLE_CONFIGURED_FEATURE = ResourceKey.create(
  Registries.CONFIGURED_FEATURE,
  new ResourceLocation(MOD_ID, "example_configured_feature")
);

// In some constant location or argument
new RegistrySetBuilder()
  // Create configured features
  .add(Registries.CONFIGURED_FEATURE, bootstrap -> {
    // Register configured features here
    bootstrap.register(
      // The resource key for the configured feature
      EXAMPLE_CONFIGURED_FEATURE,
      new ConfiguredFeature<>(
        Feature.ORE, // Create an ore feature
        new OreConfiguration(
          List.of(), // Does nothing
          8 // in veins of at most 8
        )
      )
    );
  })
  // Create placed features
  .add(Registries.PLACED_FEATURE, bootstrap -> {
    // Register placed features here
  });
```

--------------------------------

### Generate IntelliJ IDEA Run Configurations with ForgeGradle

Source: https://docs.minecraftforge.net/en/1.20.1/gettingstarted

Executes the genIntellijRuns task for IntelliJ IDEA, which generates run configurations. It may require setting the 'ideaModule' property if a 'module not specified' error occurs, pointing to the main module of the project.

```bash
gradlew genIntellijRuns
```

--------------------------------

### BlockLootSubProvider Constructor Example (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/loottables

This snippet shows how to initialize a `BlockLootSubProvider` subclass. It configures the provider with a set of explosion-resistant items and all available feature flags, determining loot table generation eligibility for blocks.

```java
// In some BlockLootSubProvider subclass
public MyBlockLootSubProvider() {
  super(Collections.emptySet(), FeatureFlags.REGISTRY.allFlags());
}
```

--------------------------------

### CompoundIngredient Example (JSON)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/ingredients

Demonstrates the structure of a CompoundIngredient, which functions as an 'OR' condition. The input stack must match at least one of the provided ingredients within the 'children' array. This is useful for defining recipes with alternative input items or custom ingredient checks.

```json
// For some input
[
  // At least one of these ingredients must match to succeed
  {
    // Ingredient
  },
  {
    // Custom ingredient
    "type": "examplemod:example_ingredient"
  }
]
```

--------------------------------

### Example JSON Language File

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/internationalization

Demonstrates the structure of a JSON language file used for localization. It maps translation keys to their corresponding displayable text in a specific locale. The file must be UTF-8 encoded.

```json
{
  "item.examplemod.example_item": "Example Item Name",
  "block.examplemod.example_block": "Example Block Name",
  "commands.examplemod.examplecommand.error": "Example Command Errored!"
}
```

--------------------------------

### Define Custom ModelBuilder Example

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Presents a basic structure for a custom `ModelBuilder` subclass, `ExampleModelBuilder`. This class can be extended to include specific properties relevant to a particular type of model generation.

```java
public class ExampleModelBuilder extends ModelBuilder<ExampleModelBuilder> {
  // ...
}
```

--------------------------------

### Encoded Pair<Integer, String> Example

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Illustrates the expected JSON structure for an encoded Pair<Integer, String> object when using fieldOf to specify keys for each element of the pair.

```json
// Encoded Pair<Integer, String>
{
  "left": 5,       // fieldOf looks up 'left' key for left object
  "right": "value" // fieldOf looks up 'right' key for right object
}
```

--------------------------------

### Get Relative and Absolute Block Positions

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Provides examples of using `GameTestHelper#absolutePos` and `GameTestHelper#relativePos` to convert between relative and absolute coordinates within a structure template scene.

```java
// Example usage within a GameTestHelper context:
BlockPos absolutePosition = helper.absolutePos(new BlockPos(x, y, z));
BlockPos relativePosition = helper.relativePos(absolutePosition);
```

--------------------------------

### sounds.json Structure Example

Source: https://docs.minecraftforge.net/en/1.20.1/gameeffects/sounds

Defines sound events, their subtitles, and the associated sound files. Supports random selection from multiple files and streaming for longer audio like music. Sound file paths are relative to the resource namespace.

```json
{
  "open_chest": {
    "subtitle": "mymod.subtitle.open_chest",
    "sounds": [ "mymod:open_chest_sound_file" ]
  },
  "epic_music": {
    "sounds": [
      {
        "name": "mymod:music/epic_music",
        "stream": true
      }
    ]
  }
}
```

--------------------------------

### IntersectionIngredient Example (JSON)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/ingredients

Demonstrates the IntersectionIngredient, which functions as an 'AND' condition. The input stack must satisfy all ingredients listed within the 'children' array. Requires at least two child ingredients.

```json
// For some input
{
  "type": "forge:intersection",

  // All of these ingredients must return true to succeed
  "children": [
    {
      // Ingredient 1
    },
    {
      // Ingredient 2
    }
    // ...
  ]
}
```

--------------------------------

### Set Mod Archives Name in build.gradle

Source: https://docs.minecraftforge.net/en/1.20.1/gettingstarted

Configures the base archives name for the mod artifact in the build.gradle file. This determines the name of the generated .jar file for the mod.

```gradle
base.archivesName = 'mymod'
```

--------------------------------

### Set Group ID in build.gradle

Source: https://docs.minecraftforge.net/en/1.20.1/gettingstarted

Sets the group ID for the mod in the build.gradle file. This typically corresponds to the top-level Java package structure and should be a domain you own or your email address.

```gradle
group = 'com.example'
```

--------------------------------

### Creating a Custom Creative Tab (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/items

This example shows how to register a custom creative tab using `CreativeModeTab.builder`. It allows you to set the tab's title, icon, and default items. The code assumes you have a `DeferredRegister<CreativeModeTab>` and `RegistryObject<Item>` and `RegistryObject<Block` instances.

```java
// Assume we have a DeferredRegister<CreativeModeTab> called REGISTRAR
// Assume we have RegistryObject<Item> and RegistryObject<Block> called ITEM and BLOCK
public static final RegistryObject<CreativeModeTab> EXAMPLE_TAB = REGISTRAR.register("example", () -> CreativeModeTab.builder()
  // Set name of tab to display
  .title(Component.translatable("item_group." + MOD_ID + ".example"))
  // Set icon of creative tab
  .icon(() -> new ItemStack(ITEM.get()))
  // Add default items to tab
  .displayItems((params, output) -> {
    output.accept(ITEM.get());
    output.accept(BLOCK.get());
  })
  .build()
);
```

--------------------------------

### Create Key Mapping for GUI - Java

Source: https://docs.minecraftforge.net/en/1.20.1/misc/keymappings

Creates a KeyMapping that is only active when a GUI screen is open. This example uses the KeyConflictContext.GUI to restrict the mapping's availability and assigns a default mouse button input.

```java
new KeyMapping(
  "key.examplemod.example2",
  KeyConflictContext.GUI, // Mapping can only be used when a screen is open
  InputConstants.Type.MOUSE, // Default mapping is on the mouse
  GLFW.GLFW_MOUSE_BUTTON_LEFT, // Default mouse input is the left mouse button
  "key.categories.examplemod.examplecategory" // Mapping will be in the new example category
)
```

--------------------------------

### Implement a Custom LootTableSubProvider (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/loottables

This example shows the basic structure for creating a custom `LootTableSubProvider`. The `generate` method is where the loot table generation logic resides, using a `BiConsumer` to write the loot table resource location and its builder.

```java
public class ExampleSubProvider implements LootTableSubProvider {

  // Used to create a factory method for the wrapping Supplier
  public ExampleSubProvider() {}

  // The method used to generate the loot tables
  @Override
  public void generate(BiConsumer<ResourceLocation, LootTable.Builder> writer) {
    // Generate loot tables here by calling writer#accept
  }
}
```

--------------------------------

### Create an Advancement using Advancement$Builder

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/advancements

This example illustrates how to use `Advancement$Builder` to construct an advancement. It chains methods to define criteria, which are essential for unlocking the advancement. The `#save` method is then called to finalize the advancement, providing the writer, registry name, and file helper.

```java
// In some ForgeAdvancementProvider$AdvancementGenerator#generate(registries, writer, existingFileHelper)
Advancement example = Advancement.Builder.advancement()
  .addCriterion("example_criterion", triggerInstance) // How the advancement is unlocked
  .save(writer, name, existingFileHelper); // Add data to builder
```

--------------------------------

### Generate Shaped Recipe with ShapedRecipeBuilder

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/recipes

This example shows how to create a shaped recipe using `ShapedRecipeBuilder`. It defines the recipe pattern, maps symbols to ingredients, specifies unlocking criteria, and saves the recipe.

```java
// In RecipeProvider#buildRecipes(writer)
ShapedRecipeBuilder builder = ShapedRecipeBuilder.shaped(RecipeCategory.MISC, result)
  .pattern("a a") // Create recipe pattern
  .define('a', item) // Define what the symbol represents
  .unlockedBy("criteria", criteria) // How the recipe is unlocked
  .save(writer); // Add data to builder
```

--------------------------------

### Encoded Dispatch Object Examples

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Shows the JSON output for dispatched objects. If the codec is not augmented from MapCodec, the object data is placed under a 'value' key. Otherwise, fields can be inlined directly.

```json
// Simple object
{
  "type": "string", // For StringObject
  "value": "value" // Codec type is not augmented from MapCodec, needs field
}

// Complex object
{
  "type": "complex", // For ComplexObject

  // Codec type is augmented from MapCodec, can be inlined
  "s": "value",
  "i": 0
}
```

--------------------------------

### Registering a BlockEntityType with DeferredRegister

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/registries

Provides an example of registering a BlockEntityType using DeferredRegister. BlockEntityType classes act as factories for creating BlockEntity instances. This example demonstrates using the Builder pattern to create and register the BlockEntityType.

```java
public static final RegistryObject<BlockEntityType<ExampleBlockEntity>> EXAMPLE_BLOCK_ENTITY = REGISTER.register(
  "example_block_entity", () -> BlockEntityType.Builder.of(ExampleBlockEntity::new, EXAMPLE_BLOCK.get()).build(null)
);
```

--------------------------------

### Encoded Either<Integer, String> Examples

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Demonstrates the encoded format for Either<Integer, String>. An Either$Left will be encoded directly as its value, while an Either$Right will be encoded as a JSON string.

```json
// Encoded Either$Left<Integer, String>
5

// Encoded Either$Right<Integer, String>
"value"
```

--------------------------------

### Generate Stonecutting Recipe with SingleItemRecipeBuilder

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/recipes

This example shows how to generate a stonecutting recipe using `SingleItemRecipeBuilder`. It defines the input and output items, recipe category, unlocking criteria, and saves the recipe.

```java
// In RecipeProvider#buildRecipes(writer)
SingleItemRecipeBuilder builder = SingleItemRecipeBuilder.stonecutting(input, RecipeCategory.MISC, result)
  .unlockedBy("criteria", criteria) // How the recipe is unlocked
  .save(writer); // Add data to builder
```

--------------------------------

### Accessing Loot Tables with ResourceLocation

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/loottables

Demonstrates how to reference and retrieve a loot table using its `ResourceLocation`. This involves obtaining the `LootDataResolver` from the `MinecraftServer` to get the specific `LootTable`.

```java
String lootTableId = "data/<namespace>/loot_tables/<path>.json";
LootTable lootTable = minecraftServer.getLootData().getLootTable(new ResourceLocation(lootTableId));
```

--------------------------------

### Generating Items from Loot Tables

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/loottables

Provides examples of methods available on the `LootTable` interface for generating `ItemStack`s. These methods vary in how they consume or return the generated items and accept either `LootParams` or `LootContext`.

```java
// Consumes generated items
lootTable.getRandomItemsRaw(lootParams, new ArrayList<>());

// Returns generated items
lootTable.getRandomItems(lootParams);

// Fills a container
lootTable.fill(container, lootParams);
```

--------------------------------

### Root Transform Element-wise Specification Example

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelextensions/transforms

Demonstrates applying root transforms using individual properties like origin, translation, and rotation. The transformations are applied in a specific order: translation, left rotation, scale, and finally, the transformation origin. Defaults are applied if properties are not explicitly defined.

```json
{
    "transform": {
        "origin": "center",
        "translation": [ 0, 0.5, 0 ],
        "rotation": { "y": 45 }
    }
    // ...
}
```

--------------------------------

### Creating DirectionProperty for Block States

Source: https://docs.minecraftforge.net/en/1.20.1/blocks/states

Provides examples of creating DirectionProperties, which are convenience implementations of EnumProperty for Direction. This allows specifying horizontal directions or directions along a specific axis.

```java
DirectionProperty.create("property_name", Direction.Plane.HORIZONTAL);
```

```java
DirectionProperty.create("property_name", Direction.Axis.X);
```

--------------------------------

### Automatically Registering Static Client-Only Event Handler (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/events

Demonstrates automatic registration of static event handlers using the `@Mod.EventBusSubscriber` annotation. This example specifies the mod ID, event bus, and client-side distribution.

```java
@Mod.EventBusSubscriber(modid = "mymod", bus = Bus.FORGE, value = Dist.CLIENT)
public class MyStaticClientOnlyEventHandler {
    @SubscribeEvent
    public static void drawLast(RenderLevelStageEvent event) {
        System.out.println("Drawing!");
    }
}
```

--------------------------------

### Creating a Forge Configuration with Builder

Source: https://docs.minecraftforge.net/en/1.20.1/misc/config

Demonstrates how to create a Forge configuration specification using `ForgeConfigSpec$Builder`. This includes pushing and popping sections to organize configuration values and using the `configure` method with a constructor reference to attach config values to a class. The `configure` method returns a pair of the configuration holder class and the spec.

```java
static {
  Pair<ExampleConfig, ForgeConfigSpec> pair = new ForgeConfigSpec.Builder()
    .configure(ExampleConfig::new);
  // Store pair values in some constant field
}
```

--------------------------------

### Minecraft Forge Screen Initialization with Widgets

Source: https://docs.minecraftforge.net/en/1.20.1/gui/screens

Illustrates the initialization process for a Minecraft Forge screen. The #init method is called upon screen creation or resize, and it's used to set up initial settings and add various types of widgets, such as interactable and renderable EditBox components.

```java
// In some Screen subclass
@Override
protected void init() {
    super.init();

    // Add widgets and precomputed values
    this.addRenderableWidget(new EditBox(/* ... */));
}
```

--------------------------------

### Adding Translations in LanguageProvider

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/localization

This example shows how to add translations within the `LanguageProvider#addTranslations` method. It includes adding translations for a block using `addBlock` and for a generic object using the `add` method with a custom translation key.

```java
// In LanguageProvider#addTranslations
this.addBlock(EXAMPLE_BLOCK, "Example Block");
this.add("object.examplemod.example_object", "Example Object");
```

--------------------------------

### DistExecutor: Call Method and Get Result on Specific Side (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/sides

Illustrates using DistExecutor to call a method on a specific physical side and retrieve its result. `safeCallWhenOn` is demonstrated, which includes validation in development environments. Note the warning regarding Java 9+ and `BootstrapMethodError`.

```java
public static Object safeCallMethodExample() {
  // ... client-side logic returning an object
  return null;
}

// In some common class
DistExecutor.safeCallWhenOn(Dist.CLIENT, () -> ExampleClass::safeCallMethodExample);
```

--------------------------------

### StrictNBTIngredient Example (JSON)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/ingredients

Illustrates the usage of StrictNBTIngredient for exact item, damage, and NBT tag matching. The 'type' is set to 'forge:nbt', and the 'nbt' field must contain an exact match of the relevant NBT data on the ItemStack.

```json
// For some input
{
  "type": "forge:nbt",
  "item": "examplemod:example_item",
  "nbt": {
    // Add nbt data (must match exactly what is on the stack)
  }
}
```

--------------------------------

### Advancement Rewards JSON Structure

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/advancements

An example of the JSON structure for advancement rewards in Minecraft. This can include experience points, loot tables, recipes for the recipe book, and functions executed as a creative player.

```json
"rewards": {
  "experience": 10,
  "loot": [
    "minecraft:example_loot_table",
    "minecraft:example_loot_table2"
    // ...
  ],
  "recipes": [
    "minecraft:example_recipe",
    "minecraft:example_recipe2"
    // ...
  ],
  "function": "minecraft:example_function"
}
```

--------------------------------

### AbstractContainerScreen Constructor and Positioning Fields (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/gui/screens

Demonstrates the constructor of an AbstractContainerScreen subclass, setting initial values for title and inventory label X coordinates. It also highlights the relationship between imageHeight and inventoryLabelY.

```java
public MyContainerScreen(MyMenu menu, Inventory playerInventory, Component title) {
    super(menu, playerInventory, title);

    this.titleLabelX = 10;
    this.inventoryLabelX = 10;

    /*
     * If the 'imageHeight' is changed, 'inventoryLabelY' must also be
     * changed as the value depends on the 'imageHeight' value.
     */
}
```

--------------------------------

### Configure Game Test Method with Annotations

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Shows how to configure a Game Test's execution using members of the `@GameTest` annotation. This includes setting setup ticks and defining whether the test is required for batch execution.

```java
// In some class
@GameTest(
  setupTicks = 20L, // The test spends 20 ticks to set up for execution
  required = false // The failure is logged but does not affect the execution of the batch
)
public static void exampleConfiguredTest(GameTestHelper helper) {
  // Do stuff
}
```

--------------------------------

### Opening a Screen with SimpleMenuProvider (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/gui/menus

Demonstrates how to open a custom menu screen for a player using `NetworkHooks.openScreen` and `SimpleMenuProvider`. This requires a server player instance and a `MenuProvider` implementation.

```java
NetworkHooks.openScreen(serverPlayer, new SimpleMenuProvider(
  (containerId, playerInventory, player) -> new MyMenu(containerId, playerInventory),
  Component.translatable("menu.title.examplemod.mymenu")
));
```

--------------------------------

### Add Chainable Methods to Custom Loader Builder

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Illustrates how to add custom chainable configuration methods, such as `exampleInt` and `exampleString`, to a custom loader builder class. These methods allow for setting specific properties and return the builder instance for fluent configuration.

```java
// In ExampleLoaderBuilder
public ExampleLoaderBuilder<T> exampleInt(int example) {
  // Set int
  return this;
}

public ExampleLoaderBuilder<T> exampleString(String example) {
  // Set string
  return this;
}
```

--------------------------------

### Minecraft Data-Driven Recipe Result with NBT

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes

This JSON example illustrates how to define the result of a data-driven Minecraft recipe. It expands on the vanilla system by allowing a full ItemStack object, including 'item', 'count', and 'nbt' data, to be specified as the recipe's output. This provides greater control over the resulting items.

```json
{
  "result": {
    "item": "examplemod:example_item",
    "count": 4,
    "nbt": {
        // Add tag data here
    }
  }
}
```

--------------------------------

### Get Known Blocks for BlockLootSubProvider (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/loottables

This method overrides `getKnownBlocks` in a `BlockLootSubProvider` subclass to provide all registered blocks for loot table generation. It utilizes `DeferredRegister` and streams entries from `RegistryObject`.

```java
// In some BlockLootSubProvider subclass for some DeferredRegister BLOCK_REGISTRAR
@Override
protected Iterable<Block> getKnownBlocks() {
  return BLOCK_REGISTRAR.getEntries() // Get all registered entries
    .stream() // Stream the wrapped objects
    .flatMap(RegistryObject::stream) // Get the object if available
    ::iterator; // Create the iterable
}
```

--------------------------------

### Copying Block Tags to Item Tags

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/tags

This example illustrates the utility of the `copy` method in `ItemTagsProvider`, which allows for the efficient generation of item tags by copying entries from corresponding block tags. This is useful when block tags have direct item representations.

```java
//In ItemTagsProvider#addTags
this.copy(EXAMPLE_BLOCK_TAG, EXAMPLE_ITEM_TAG);
```

--------------------------------

### Retrieve Item Handler Capability Instance (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/capabilities

This snippet shows how to obtain the specific instance for the ITEM_HANDLER capability. It uses CapabilityManager and an anonymous CapabilityToken to get a generic capability instance. Note that even if the capability is available, it may not be registered.

```java
public static final Capability<IItemHandler> ITEM_HANDLER = CapabilityManager.get(new CapabilityToken<>(){});
```

--------------------------------

### Saved Data Declaration and Usage (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/saveddata

Demonstrates the core methods for implementing a SavedData class, including saving NBT data and marking it as dirty. It also shows how to use `DimensionDataStorage#computeIfAbsent` to get or create a SavedData instance.

```Java
// In some class
public ExampleSavedData create() {
  return new ExampleSavedData();
}

public ExampleSavedData load(CompoundTag tag) {
  ExampleSavedData data = this.create();
  // Load saved data
  return data;
}

// In some method within the class
netherDataStorage.computeIfAbsent(this::load, this::create, "example");
```

--------------------------------

### Generate Loot Tables in BlockLootSubProvider (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/loottables

This is an example of overriding the `generate` method within a `BlockLootSubProvider` subclass. This is where the specific logic for creating and adding block loot tables to the writer would be implemented.

```java
// In some BlockLootSubProvider subclass
@Override
public void generate() {
  // Add loot tables here
}
```

--------------------------------

### Getting and Setting BlockState Property Values

Source: https://docs.minecraftforge.net/en/1.20.1/blocks/states

Explains how to retrieve the current value of a property from a BlockState using getValue and how to obtain a new BlockState with a modified property value using setValue. Note that BlockStates are immutable.

```java
BlockState currentState = level.getBlockState(pos);
Direction facing = currentState.getValue(FACING);
BlockState newState = currentState.setValue(OPEN, true);
```

--------------------------------

### Placing and Getting BlockStates in the World

Source: https://docs.minecraftforge.net/en/1.20.1/blocks/states

Shows how to interact with the game world to place blocks and query existing block states. Level#setBlockAndUpdate is used to change a block at a specific position, and Level#getBlockState retrieves the current state.

```java
level.setBlockAndUpdate(blockPos, block.defaultBlockState().setValue(PROPERTY, value));
BlockState state = level.getBlockState(blockPos);
```

--------------------------------

### Register Container Screen with Menu

Source: https://docs.minecraftforge.net/en/1.20.1/gui/screens

Details how to register an AbstractContainerScreen with its corresponding menu type using MenuScreens.register. This must be done within the FMLClientSetupEvent, specifically using enqueueWork to ensure thread safety.

```java
// Event is listened to on the mod event bus
private void clientSetup(FMLClientSetupEvent event) {
    event.enqueueWork(
        // Assume RegistryObject<MenuType<MyMenu>> MY_MENU
        // Assume MyContainerScreen<MyMenu> which takes in three parameters
        () -> MenuScreens.register(MY_MENU.get(), MyContainerScreen::new)
    );
}
```

--------------------------------

### Child Model Visibility Override (JSON)

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelextensions/visibility

Demonstrates a child model inheriting from a composite model and overriding visibility. This example makes 'part_one' invisible and 'part_two' visible, overriding the parent's visibility settings.

```json
{
  "parent": "mymod:mycompositemodel",
  "visibility": {
    "part_one": false,
    "part_two": true
  }
}
```

--------------------------------

### Automatic Translation Key Generation for Blocks and Items

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/internationalization

Explains how Minecraft automatically generates translation keys for blocks and items based on their registry names. This involves prepending 'block.' or 'item.' and replacing colons with dots. The example shows the expected entry in a language file for an item.

```java
// By default, #getDescriptionId will return `block.` or `item.` prepended to the
// registry name of the block or item, with the colon replaced by a dot.
// For example, an item with ID `examplemod:example_item` effectively requires
// the following line in a language file:
// {
//   "item.examplemod.example_item": "Example Item Name"
// }
```

--------------------------------

### Defining a Basic Config Value with Context

Source: https://docs.minecraftforge.net/en/1.20.1/misc/config

Shows how to define a simple configuration value using `ForgeConfigSpec$Builder`. It includes adding a comment for description and specifying the configuration value's name and its default value. The `define` method is used to create the `ConfigValue`.

```java
// For some ForgeConfigSpec$Builder builder
ConfigValue<T> value = builder.comment("Comment")
  .define("config_value_name", defaultValue);
```

--------------------------------

### Getting ModContainer in Forge

Source: https://docs.minecraftforge.net/en/1.20.1/misc/updatechecker

Illustrates various ways to obtain a ModContainer object within the Forge modding environment. This object is crucial for retrieving mod information and performing update checks. Methods include using the active container, ID, or instance.

```java
// Inside your constructor or initialization method:
ModContainer activeContainer = ModLoadingContext.get().getActiveContainer();

// Get by Mod ID:
ModContainer modContainerById = ModList.get().getModContainerById("<your modId>");

// Get by Mod instance:
ModContainer modContainerByObject = ModList.get().getModContainerByObject(<your mod instance>);

// Get another mod's container by ID:
ModContainer otherModContainer = ModList.get().getModContainerById("<other modId>");
```

--------------------------------

### JSON Model with Cutout Render Type

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelextensions/rendertypes

Example JSON structure for a block model that uses the 'minecraft:cutout' render type, typically for blocks like glass. This specifies the render type and the parent model with its textures.

```json
{
  "render_type": "minecraft:cutout",
  "parent": "block/cube_all",
  "textures": {
    "all": "block/glass"
  }
}
```

--------------------------------

### Registering Default Block State

Source: https://docs.minecraftforge.net/en/1.20.1/blocks/states

Override the registerDefaultState method in your Block class to define the initial state of your block when it's placed in the world. This example from DoorBlock sets initial values for FACING, OPEN, HINGE, POWERED, and HALF properties.

```java
this.registerDefaultState(
  this.stateDefinition.any()
    .setValue(FACING, Direction.NORTH)
    .setValue(OPEN, false)
    .setValue(HINGE, DoorHingeSide.LEFT)
    .setValue(POWERED, false)
    .setValue(HALF, DoubleBlockHalf.LOWER)
);
```

--------------------------------

### Legacy Block State Handling with Metadata (Pre-1.8)

Source: https://docs.minecraftforge.net/en/1.20.1/blocks/states

Illustrates how block states were managed using numerical metadata in Minecraft versions prior to 1.8. This approach was limited by the lack of semantic meaning in the metadata values, requiring developers to rely on code comments for interpretation. The example shows a switch statement based on metadata to determine block behavior.

```java
switch (meta) {
  case 0: { ... } // south and on the lower half of the block
  case 1: { ... } // south on the upper side of the block
  case 2: { ... } // north and on the lower half of the block
  case 3: { ... } // north and on the upper half of the block
  // ... etc. ...
}
```

--------------------------------

### Handling Diacritics in Language Translations

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/localization

This code snippet illustrates how to include localized names with diacritical marks in language files. The `LanguageProvider` automatically handles the conversion of these characters into their Unicode equivalents for in-game display. The example shows adding an item with a diacritic character in its name.

```java
// Encdoded as 'Example with a d\u00EDacritic'
this.addItem("example.diacritic", "Example with a díacritic");
```

--------------------------------

### Retrieving Update Check Results in Forge

Source: https://docs.minecraftforge.net/en/1.20.1/misc/updatechecker

This Java code demonstrates how to retrieve the status of an update check for a mod using Forge's API. It involves obtaining the ModContainer and then using VersionChecker to get the result, which includes the status, target version, and changelog.

```java
IModInfo modInfo = ModContainer.getModInfo();
VersionChecker.Result result = VersionChecker.getResult(modInfo);

// You can then access the status, target version, and changelog lines from the 'result' object.
// Example: result.status()
// Example: result.targetVersion()
// Example: result.changelogLines()
```

--------------------------------

### Forge Recipe Manager Usage with IItemHandler

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes

This Java code snippet shows how to use Forge's RecipeManager to get a crafting recipe. It utilizes the RecipeWrapper utility class to adapt an IItemHandler (commonly used for inventories) into a Container, which is then passed to the getRecipeFor method along with the RecipeType and the current level.

```java
recipeManger.getRecipeFor(RecipeType.CRAFTING, new RecipeWrapper(handler), level);
```

--------------------------------

### Build Block State Variants with Partial States

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Demonstrates how to use `partialState` with `VariantBlockStateBuilder` to define specific models for blocks based on individual property values like `AXIS`. Includes setting model files, weights, and rotations.

```java
this.getVariantBuilder(EXAMPLE_BLOCK_1) // Get variant builder
  .partialState() // Construct partial state
  .with(AXIS, Axis.Y) // When BlockState AXIS = Y
    .modelForState() // Set models when AXIS = Y
    .modelFile(yModelFile1) // Can show 'yModelFile1'
    .nextModel() // Adds another model when AXIS = Y
    .modelFile(yModelFile2) // Can show 'yModelFile2'
    .weight(2) // Will show 'yModelFile2' 2/3 of the time
    .addModel() // Finalizes models when AXIS = Y
  .with(AXIS, Axis.Z) // When BlockState AXIS = Z
    .modelForState() // Set models when AXIS = Z
    .modelFile(hModelFile) // Can show 'hModelFile'
    .addModel() // Finalizes models when AXIS = Z
  .with(AXIS, Axis.X)  // When BlockState AXIS = X
    .modelForState() // Set models when AXIS = X
    .modelFile(hModelFile) // Can show 'hModelFile'
    .rotationY(90) // Rotates 'hModelFile' 90 degrees on the Y axis
    .addModel(); // Finalizes models when AXIS = X
```

--------------------------------

### Create SimpleChannel Instance

Source: https://docs.minecraftforge.net/en/1.20.1/networking/simpleimpl

Initializes a SimpleChannel for custom packet communication. Requires a unique channel name, protocol version supplier, and version compatibility checkers. The version checkers ensure that both client and server agree on the protocol version for the channel to be compatible.

```java
private static final String PROTOCOL_VERSION = "1";
public static final SimpleChannel INSTANCE = NetworkRegistry.newSimpleChannel(
  new ResourceLocation("mymodid", "main"),
  () -> PROTOCOL_VERSION,
  PROTOCOL_VERSION::equals,
  PROTOCOL_VERSION::equals
);
```

--------------------------------

### Generating All Models with ModelProvider (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Shows how to call the `generateAll` method on a `ModelProvider` instance within the `run` method of a custom `IDataProvider`. This method generates all associated models and returns a `CompletableFuture`. It integrates this future with other futures for file writing.

```java
// In ExampleModelConsumerProvider
@Override
public CompletableFuture<?> run(CachedOutput cache) {
  // Populate the model provider
  CompletableFuture<?> exampleFutures = this.example.generateAll(cache); // Generate the models

  // Run logic and create CompletableFuture(s) for writing files
  // ...

  // Assume we have a new CompletableFuture providerFuture
  return CompletableFuture.allOf(exampleFutures, providerFuture);
}
```

--------------------------------

### Generated Item Model Face Data in Minecraft Forge

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelextensions/facedata

Shows how to apply face data to generated item models using the 'forge:item_layers' model loader. The 'forge_data' is placed at the top level and applies to specified texture layers. This example configures layer 1 to be tinted red and glow at full brightness, illustrating custom layer properties.

```json
{
  "textures": {
    "layer0": "minecraft:item/stick",
    "layer1": "minecraft:item/glowstone_dust"
  },
  "forge_data": {
    "1": {
      "color": "0xFFFF0000",
      "block_light": 15,
      "sky_light": 15,
      "ambient_occlusion": false
    }
  }
}
```

--------------------------------

### Implementing containerTick in AbstractContainerScreen (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/gui/screens

Shows how to override the containerTick method in an AbstractContainerScreen subclass to perform actions on each tick while the screen is active and the player is looking at it. This is commonly used for ticking the recipe book.

```java
@Override
protected void containerTick() {
    super.containerTick();

    // Tick things here
}
```

--------------------------------

### Ambient Occlusion Override in Minecraft Forge Models

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelextensions/facedata

Illustrates how the 'ambient_occlusion' flag can be set at the element or face level in Minecraft Forge models. This example shows that if the top-level 'ambientocclusion' is false, setting 'forge_data.ambient_occlusion' to true on an element will not override the global setting. This highlights the hierarchy of AO settings.

```json
{
  "ambientocclusion": false,
  "elements": [
    {
      "forge_data": {
        "ambient_occlusion": true // Has no effect
      }
    }
  ]
}
```

--------------------------------

### Registering Mod Event Listeners in Forge

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/lifecycle

Demonstrates two primary ways to register event listeners for mod lifecycle events in Minecraft Forge: using the @EventBusSubscriber annotation and registering listeners in the mod constructor. Ensure correct modid and bus are specified. The examples show a static listener registration and an instance-based listener registration.

```java
@Mod.EventBusSubscriber(modid = "mymod", bus = Mod.EventBusSubscriber.Bus.MOD)
public class MyModEventSubscriber {
  @SubscribeEvent
  static void onCommonSetup(FMLCommonSetupEvent event) { ... }
}

@Mod("mymod")
public class MyMod {
  public MyMod() {
    FMLModLoadingContext.get().getModEventBus().addListener(this::onCommonSetup);
  }

  private void onCommonSetup(FMLCommonSetupEvent event) { ... }
}
```

--------------------------------

### Simplified IItemHandler Capability Exposure with Capability#orEmpty

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/capabilities

Provides a more concise way to expose a single capability, like IItemHandler, using Capability#orEmpty. This method simplifies the getCapability implementation by avoiding explicit if/else checks, making the code cleaner when only one capability is exposed.

```java
@Override
public <T> LazyOptional<T> getCapability(Capability<T> cap, Direction side) {
  return ForgeCapabilities.ITEM_HANDLER.orEmpty(cap, inventoryHandlerLazyOptional);
}
```

--------------------------------

### Exposing IItemHandler Capability in BlockEntity

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/capabilities

Demonstrates how to expose an IItemHandler capability from a BlockEntity using LazyOptional. It includes the initialization of the LazyOptional, overriding getCapability to return the capability, and invalidating it in invalidateCaps. This pattern ensures capabilities are initialized lazily and properly managed.

```java
// Somewhere in your BlockEntity subclass
LazyOptional<IItemHandler> inventoryHandlerLazyOptional;

// Supplied instance (e.g. () -> inventoryHandler)
// Ensure laziness as initialization should only happen when needed
inventoryHandlerLazyOptional = LazyOptional.of(inventoryHandlerSupplier);

@Override
public <T> LazyOptional<T> getCapability(Capability<T> cap, Direction side) {
  if (cap == ForgeCapabilities.ITEM_HANDLER) {
    return inventoryHandlerLazyOptional.cast();
  }
  return super.getCapability(cap, side);
}

@Override
public void invalidateCaps() {
  super.invalidateCaps();
  inventoryHandlerLazyOptional.invalidate();
}
```

--------------------------------

### Defining a List Config Value

Source: https://docs.minecraftforge.net/en/1.20.1/misc/config

Shows how to define a configuration value that is a list of elements. The `#defineList` or `#defineListAllowEmpty` methods are used. This requires the default value, a validator for list elements, and the type class. It allows for lists of any type, including empty lists if `defineListAllowEmpty` is used.

```java
// Assuming builder is an instance of ForgeConfigSpec.Builder
// and T is the type of elements in the list
// A validator could be a Predicate<T>
ConfigValue<List<T>> listValue = builder.comment("A list of configuration items")
  .defineList("list_config", defaultValue, validator, type);

// To allow an empty list:
ConfigValue<List<T>> emptyListValue = builder.comment("An optional list of configuration items")
  .defineListAllowEmpty("optional_list_config", Collections.emptyList(), validator, type);

```

--------------------------------

### Render Container Screen in Minecraft Forge

Source: https://docs.minecraftforge.net/en/1.20.1/gui/screens

Demonstrates the render method for a container screen, which typically involves rendering the background, calling the super method, and then rendering tooltips. This is the most common method to override when customizing container screen appearance.

```java
// In some AbstractContainerScreen subclass
@Override
public void render(GuiGraphics graphics, int mouseX, int mouseY, float partialTick) {
    this.renderBackground(graphics);
    super.render(graphics, mouseX, mouseY, partialTick);

    /*
     * This method is added by the container screen to render
     * the tooltip of the hovered slot.
     */
    this.renderTooltip(graphics, mouseX, mouseY);
}
```

--------------------------------

### Send Packet to Server (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/networking/simpleimpl

Demonstrates the single method for sending a packet from the client to the server using a `SimpleChannel` instance.

```java
INSTANCE.sendToServer(new MyMessage());
```

--------------------------------

### Implementing stillValid with ContainerLevelAccess

Source: https://docs.minecraftforge.net/en/1.20.1/gui/menus

Demonstrates how to implement the `#stillValid` method in an `AbstractContainerMenu` subclass, utilizing `ContainerLevelAccess`. The server constructor creates a `ContainerLevelAccess` instance, while the client constructor uses `ContainerLevelAccess.NULL`. The `#stillValid` method then calls the static utility method to check player proximity to the attached block.

```java
// Client menu constructor
public MyMenuAccess(int containerId, Inventory playerInventory) {
  this(containerId, playerInventory, ContainerLevelAccess.NULL);
}

// Server menu constructor
public MyMenuAccess(int containerId, Inventory playerInventory, ContainerLevelAccess access) {
  // ...
}

// Assume this menu is attached to RegistryObject<Block> MY_BLOCK
@Override
public boolean stillValid(Player player) {
  return AbstractContainerMenu.stillValid(this.access, player, MY_BLOCK.get());
}
```

--------------------------------

### Creating a Record Codec with RecordCodecBuilder in Java

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Demonstrates how to create a codec for a custom object using `RecordCodecBuilder.create`. This method allows defining explicit named fields for encoding and decoding. It shows how to group fields, specify their types and names, handle optional fields with default values, and apply these definitions to construct the object.

```java
public static final Codec<SomeObject> RECORD_CODEC = RecordCodecBuilder.create(instance -> // Given an instance
  instance.group( // Define the fields within the instance
    Codec.STRING.fieldOf("s").forGetter(SomeObject::s),
    Codec.INT.optionalFieldOf("i", 0).forGetter(SomeObject::i), // Integer, defaults to 0 if field not present
    Codec.BOOL.fieldOf("b").forGetter(SomeObject::b) // Boolean
  ).apply(instance, SomeObject::new) // Define how to create the object
);
```

--------------------------------

### Open Minecraft Menu Screen with NetworkHooks

Source: https://docs.minecraftforge.net/en/1.20.1/gui/menus

This Java code snippet shows how to open a menu screen for a player using `NetworkHooks#openScreen`. It requires the player initiating the menu, a `MenuProvider` object from the server side, and optionally a `FriendlyByteBuf` for syncing additional data to the client if the menu was created with an `IContainerFactory`.

```java
// On the logical server:
NetworkHooks.openScreen(player, menuProvider);
// Or with extra data:
NetworkHooks.openScreen(player, menuProvider, buffer);
```

--------------------------------

### Render Background Texture for Container Screen

Source: https://docs.minecraftforge.net/en/1.20.1/gui/screens

Illustrates the renderBg method used to draw the background texture of a container screen. It involves defining the texture's location and using the blit method to draw it at the correct position with specified dimensions.

```java
// In some AbstractContainerScreen subclass

// The location of the background texture (assets/<namespace>/<path>)
private static final ResourceLocation BACKGROUND_LOCATION = new ResourceLocation(MOD_ID, "textures/gui/container/my_container_screen.png");

@Override
protected void renderBg(GuiGraphics graphics, float partialTick, int mouseX, int mouseY) {
    /*
     * Renders the background texture to the screen. 'leftPos' and
     * 'topPos' should already represent the top left corner of where
     * the texture should be rendered as it was precomputed from the
     * 'imageWidth' and 'imageHeight'. The two zeros represent the
     * integer u/v coordinates inside the 256 x 256 PNG file.
     */
    graphics.blit(BACKGROUND_LOCATION, this.leftPos, this.topPos, 0, 0, this.imageWidth, this.imageHeight);
}
```

--------------------------------

### Implement quickMoveStack for Item Transfer in Minecraft Forge

Source: https://docs.minecraftforge.net/en/1.20.1/gui/menus

This Java code snippet demonstrates the implementation of the `#quickMoveStack` method, crucial for handling shift-clicks and quick item movements within menus. It manages item stacks between various inventory slots, including player inventory, hotbar, and custom data inventories. The method relies on `#moveItemStackTo` for actual item transfer and includes logic for updating slots and handling item counts.

```java
@Override
public ItemStack quickMoveStack(Player player, int quickMovedSlotIndex) {
  // The quick moved slot stack
  ItemStack quickMovedStack = ItemStack.EMPTY;
  // The quick moved slot
  Slot quickMovedSlot = this.slots.get(quickMovedSlotIndex)


   // If the slot is in the valid range and the slot is not empty
  if (quickMovedSlot != null && quickMovedSlot.hasItem()) {
    // Get the raw stack to move
    ItemStack rawStack = quickMovedSlot.getItem(); 
    // Set the slot stack to a copy of the raw stack
    quickMovedStack = rawStack.copy();

    /*
    The following quick move logic can be simplified to if in data inventory,
    try to move to player inventory/hotbar and vice versa for containers
    that cannot transform data (e.g. chests).
    */

    // If the quick move was performed on the data inventory result slot
    if (quickMovedSlotIndex == 0) {
      // Try to move the result slot into the player inventory/hotbar
      if (!this.moveItemStackTo(rawStack, 5, 41, true)) {
        // If cannot move, no longer quick move
        return ItemStack.EMPTY;
      }

      // Perform logic on result slot quick move
      slot.onQuickCraft(rawStack, quickMovedStack);
    }
    // Else if the quick move was performed on the player inventory or hotbar slot
    else if (quickMovedSlotIndex >= 5 && quickMovedSlotIndex < 41) {
      // Try to move the inventory/hotbar slot into the data inventory input slots
      if (!this.moveItemStackTo(rawStack, 1, 5, false)) {
        // If cannot move and in player inventory slot, try to move to hotbar
        if (quickMovedSlotIndex < 32) {
          if (!this.moveItemStackTo(rawStack, 32, 41, false)) {
            // If cannot move, no longer quick move
            return ItemStack.EMPTY;
          }
        }
        // Else try to move hotbar into player inventory slot
        else if (!this.moveItemStackTo(rawStack, 5, 32, false)) {
          // If cannot move, no longer quick move
          return ItemStack.EMPTY;
        }
      }
    }
    // Else if the quick move was performed on the data inventory input slots, try to move to player inventory/hotbar
    else if (!this.moveItemStackTo(rawStack, 5, 41, false)) {
      // If cannot move, no longer quick move
      return ItemStack.EMPTY;
    }

    if (rawStack.isEmpty()) {
      // If the raw stack has completely moved out of the slot, set the slot to the empty stack
      quickMovedSlot.set(ItemStack.EMPTY);
    } else {
      // Otherwise, notify the slot that that the stack count has changed
      quickMovedSlot.setChanged();
    }

    /*
    The following if statement and Slot#onTake call can be removed if the
    menu does not represent a container that can transform stacks (e.g.
    chests).
    */
    if (rawStack.getCount() == quickMovedStack.getCount()) {
      // If the raw stack was not able to be moved to another slot, no longer quick move
      return ItemStack.EMPTY;
    }
    // Execute logic on what to do post move with the remaining stack
    quickMovedSlot.onTake(player, rawStack);
  }

  return quickMovedStack; // Return the slot stack
}
```

--------------------------------

### Synchronize Items and Integers using SlotItemHandler and DataSlot (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/gui/menus

Demonstrates the server and client constructors for a menu synchronizing item stacks via SlotItemHandler and a single integer via DataSlot. It shows how to add slots and data slots to the menu.

```java
public MyMenuAccess(int containerId, Inventory playerInventory) {
  this(containerId, playerInventory, new ItemStackHandler(5), DataSlot.standalone());
}

public MyMenuAccess(int containerId, Inventory playerInventory, IItemHandler dataInventory, DataSlot dataSingle) {
  // Check if the data inventory size is some fixed value
  // Then, add slots for data inventory
  this.addSlot(new SlotItemHandler(dataInventory, /*...*/));

  // Add slots for player inventory
  this.addSlot(new Slot(playerInventory, /*...*/));

  // Add data slots for handled integers
  this.addDataSlot(dataSingle);

  // ...
}
```

--------------------------------

### Generate MultiPartBlockStates with Conditions using Java

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Demonstrates how to use `MultiPartBlockStateBuilder` to define complex block states. It involves creating multiple 'parts', each with specific model files and conditions. Conditions can be grouped using AND logic, or OR logic can be applied to a group. This is typically used within a `BlockStateProvider#registerStatesAndModels` method.

```java
// In some BlockStateProvider#registerStatesAndModels

// Redstone Wire
this.getMultipartBuilder(REDSTONE) // Get multipart builder
  .part() // Create part
    .modelFile(redstoneDot) // Can show 'redstoneDot'
    .addModel() // 'redstoneDot' is displayed when...
    .useOr() // At least one of these conditions are true
    .nestedGroup() // true when all grouped conditions are true
      .condition(WEST_REDSTONE, NONE) // true when WEST_REDSTONE is NONE
      .condition(EAST_REDSTONE, NONE) // true when EAST_REDSTONE is NONE
      .condition(SOUTH_REDSTONE, NONE) // true when SOUTH_REDSTONE is NONE
      .condition(NORTH_REDSTONE, NONE) // true when NORTH_REDSTONE is NONE
    .endNestedGroup() // End group
    .nestedGroup() // true when all grouped conditions are true
      .condition(EAST_REDSTONE, SIDE, UP) // true when EAST_REDSTONE is SIDE or UP
      .condition(NORTH_REDSTONE, SIDE, UP) // true when NORTH_REDSTONE is SIDE or UP
    .endNestedGroup() // End group
    .nestedGroup() // true when all grouped conditions are true
      .condition(EAST_REDSTONE, SIDE, UP) // true when EAST_REDSTONE is SIDE or UP
      .condition(SOUTH_REDSTONE, SIDE, UP) // true when SOUTH_REDSTONE is SIDE or UP
    .endNestedGroup() // End group
    .nestedGroup() // true when all grouped conditions are true
      .condition(WEST_REDSTONE, SIDE, UP) // true when WEST_REDSTONE is SIDE or UP
      .condition(SOUTH_REDSTONE, SIDE, UP) // true when SOUTH_REDSTONE is SIDE or UP
    .endNestedGroup() // End group
    .nestedGroup() // true when all grouped conditions are true
      .condition(WEST_REDSTONE, SIDE, UP) // true when WEST_REDSTONE is SIDE or UP
      .condition(NORTH_REDSTONE, SIDE, UP) // true when NORTH_REDSTONE is SIDE or UP
    .endNestedGroup() // End group
    .end() // Finish part
  .part() // Create part
    .modelFile(redstoneSide0) // Can show 'redstoneSide0'
    .addModel() // 'redstoneSide0' is displayed when...
    .condition(NORTH_REDSTONE, SIDE, UP) // NORTH_REDSTONE is SIDE or UP
    .end() // Finish part
  .part() // Create part
    .modelFile(redstoneSideAlt0) // Can show 'redstoneSideAlt0'
    .addModel() // 'redstoneSideAlt0' is displayed when...
    .condition(SOUTH_REDSTONE, SIDE, UP) // SOUTH_REDSTONE is SIDE or UP
    .end() // Finish part
  .part() // Create part
    .modelFile(redstoneSideAlt1) // Can show 'redstoneSideAlt1'
    .rotationY(270) // Rotates 'redstoneSideAlt1' 270 degrees on the Y axis
    .addModel() // 'redstoneSideAlt1' is displayed when...
    .condition(EAST_REDSTONE, SIDE, UP) // EAST_REDSTONE is SIDE or UP
    .end() // Finish part
  .part() // Create part
    .modelFile(redstoneSide1) // Can show 'redstoneSide1'
    .rotationY(270) // Rotates 'redstoneSide1' 270 degrees on the Y axis
    .addModel() // 'redstoneSide1' is displayed when...
    .condition(WEST_REDSTONE, SIDE, UP) // WEST_REDSTONE is SIDE or UP
    .end() // Finish part
  .part() // Create part
    .modelFile(redstoneUp) // Can show 'redstoneUp'
    .addModel() // 'redstoneUp' is displayed when...
    .condition(NORTH_REDSTONE, UP) // NORTH_REDSTONE is UP
    .end() // Finish part
  .part() // Create part
    .modelFile(redstoneUp) // Can show 'redstoneUp'
    .rotationY(90) // Rotates 'redstoneUp' 90 degrees on the Y axis
    .addModel() // 'redstoneUp' is displayed when...
    .condition(EAST_REDSTONE, UP) // EAST_REDSTONE is UP
    .end() // Finish part
  .part() // Create part
    .modelFile(redstoneUp) // Can show 'redstoneUp'
    .rotationY(180) // Rotates 'redstoneUp' 180 degrees on the Y axis
    .addModel() // 'redstoneUp' is displayed when...
    .condition(SOUTH_REDSTONE, UP) // SOUTH_REDSTONE is UP
    .end() // Finish part
  .part() // Create part
    .modelFile(redstoneUp) // Can show 'redstoneUp'
    .rotationY(270) // Rotates 'redstoneUp' 270 degrees on the Y axis
    .addModel() // 'redstoneUp' is displayed when...
    .condition(WEST_REDSTONE, UP) // WEST_REDSTONE is UP
    .end(); // Finish part

```

--------------------------------

### Mob Implementation of MenuProvider (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/gui/menus

Illustrates how a mob can implement `MenuProvider` and handle player interaction via `Mob#mobInteract` to open a menu. This approach is similar to block implementation but the mob itself holds the `MenuProvider`.

```java
public class MyMob extends Mob implements MenuProvider {
  // ...

  @Override
  public InteractionResult mobInteract(Player player, InteractionHand hand) {
    if (!this.level.isClientSide && player instanceof ServerPlayer serverPlayer) {
      NetworkHooks.openScreen(serverPlayer, this);
    }
    return InteractionResult.sidedSuccess(this.level.isClientSide);
  }
}
```

--------------------------------

### Synchronize Multiple Integers using ContainerData (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/gui/menus

Illustrates server and client constructors for a menu synchronizing multiple integers using ContainerData. It shows how to check the data count and add data slots using the ContainerData interface.

```java
public MyMenuAccess(int containerId, Inventory playerInventory) {
  this(containerId, playerInventory, new SimpleContainerData(3));
}

public MyMenuAccess(int containerId, Inventory playerInventory, ContainerData dataMultiple) {
  // Check if the ContainerData size is some fixed value
  checkContainerDataCount(dataMultiple, 3);

  // Add data slots for handled integers
  this.addDataSlots(dataMultiple);

  // ...
}
```

--------------------------------

### Build Block State Variants for All States Except Properties

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Shows how to use `forAllStatesExcept` with `VariantBlockStateBuilder` to generate models for all states except for a specified list of properties. This is useful when certain properties do not affect the model rendering. The lambda function defines the model configuration for the relevant states.

```java
this.getVariantBuilder(EXAMPLE_BLOCK_3) // Get variant builder
  .forAllStatesExcept(state -> // For all HORIZONTAL_FACING states
    ConfiguredModel.builder() // Creates configured model builder
      .modelFile(modelFile) // Can show 'modelFile'
      .rotationY((int) state.getValue(HORIZONTAL_FACING).toYRot()) // Rotates 'modelFile' on the Y axis depending on the property
      .build(), // Creates the array of configured models
  WATERLOGGED); // Ignores WATERLOGGED property
```

--------------------------------

### Configure Enabled Game Test Namespaces (Gradle)

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Sets which namespaces will load Game Tests. If empty or unset, all namespaces are loaded. Namespaces must be comma-separated without spaces.

```gradle
property 'forge.enabledGameTestNamespaces', 'modid1,modid2,modid3'
```

--------------------------------

### Build Block State Variants for All States

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Illustrates using `forAllStates` with `VariantBlockStateBuilder` to generate models for every possible state of a block. This method takes a lambda function that returns a `ConfiguredModel` for each state, allowing dynamic model configuration like rotations based on block properties.

```java
this.getVariantBuilder(EXAMPLE_BLOCK_2) // Get variant builder
  .forAllStates(state -> // For all possible states
    ConfiguredModel.builder() // Creates configured model builder
      .modelFile(modelFile) // Can show 'modelFile'
      .rotationY((int) state.getValue(HORIZONTAL_FACING).toYRot()) // Rotates 'modelFile' on the Y axis depending on the property
      .build() // Creates the array of configured models
  );
```

--------------------------------

### Block Implementation of MenuProvider (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/gui/menus

Shows how a block can implement `MenuProvider` by overriding `BlockBehaviour#getMenuProvider` and handle player interaction in `BlockBehaviour#use` to open a menu. It ensures the menu is opened on the server side and returns the correct interaction result.

```java
@Override
public MenuProvider getMenuProvider(BlockState state, Level level, BlockPos pos) {
  return new SimpleMenuProvider(/* ... */);
}

@Override
public InteractionResult use(BlockState state, Level level, BlockPos pos, Player player, InteractionHand hand, BlockHitResult result) {
  if (!level.isClientSide && player instanceof ServerPlayer serverPlayer) {
    NetworkHooks.openScreen(serverPlayer, state.getMenuProvider(level, pos));
  }
  return InteractionResult.sidedSuccess(level.isClientSide);
}
```

--------------------------------

### Configure Access Transformer in Gradle

Source: https://docs.minecraftforge.net/en/1.20.1/advanced/accesstransformers

This snippet shows how to specify the path to your Access Transformer configuration file within your project's `build.gradle` file. This is essential for Forge to locate and apply your ATs during the build process. Ensure the path is correct relative to your project structure.

```gradle
minecraft {
  accessTransformer = file('src/main/resources/META-INF/accesstransformer.cfg')
}
```

--------------------------------

### Create Basic Key Mapping - Java

Source: https://docs.minecraftforge.net/en/1.20.1/misc/keymappings

Creates a new KeyMapping with a translation key for its name, a default input, and a translation key for its category. The default input specifies the device type (keyboard or mouse) and its identifier. The category determines where the mapping appears in the Controls menu.

```java
new KeyMapping(
  "key.examplemod.example1", // Will be localized using this translation key
  InputConstants.Type.KEYSYM, // Default mapping is on the keyboard
  GLFW.GLFW_KEY_P, // Default key is P
  "key.categories.misc" // Mapping will be in the misc category
)
```

--------------------------------

### DistExecutor: Run Method on Specific Side (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/sides

Demonstrates how to use DistExecutor to run a method on a specific physical side (client or server). `unsafeRunWhenOn` is used for executing a side-specific method without strict validation, suitable for client-side operations.

```java
public static void unsafeRunMethodExample(Object param1, Object param2) {
  // ... client-side logic
}

// In some common class
DistExecutor.unsafeRunWhenOn(Dist.CLIENT, () -> ExampleClass.unsafeRunMethodExample(var1, var2));
```

--------------------------------

### Send Packets to Clients (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/networking/simpleimpl

Shows various methods for sending packets from the server to clients using `SimpleChannel` and `PacketDistributor`. Includes sending to a specific player, players tracking a chunk, and all connected players.

```java
// Send to one player
INSTANCE.send(PacketDistributor.PLAYER.with(serverPlayer), new MyMessage());

// Send to all players tracking this level chunk
INSTANCE.send(PacketDistributor.TRACKING_CHUNK.with(levelChunk), new MyMessage());

// Send to all connected players
INSTANCE.send(PacketDistributor.ALL.noArg(), new MyMessage());
```

--------------------------------

### Minecraft Forge Screen Constructor

Source: https://docs.minecraftforge.net/en/1.20.1/gui/screens

Demonstrates the basic constructor for a Minecraft Forge screen subclass. It accepts a Component to set the screen's title, which is primarily used for narration.

```java
// In some Screen subclass
public MyScreen(Component title) {
    super(title);
}
```

--------------------------------

### Transform Codecs with Partial Equivalence (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Illustrates using comapFlatMap for transformations where the backward conversion (new type to current type) is fully supported, but the forward conversion (current type to new type) might fail. This is useful for parsing strings into integers, where not all strings are valid integers.

```java
// Given an string codec to convert to a integer
// Not all strings can become integers (A is not fully equivalent to B)
// All integers can become strings (B is fully equivalent to A)
public static final Codec<Integer> INT_CODEC = Codec.STRING.comapFlatMap(
  s -> { // Return data result containing error on failure
    try {
      return DataResult.success(Integer.valueOf(s));
    } catch (NumberFormatException e) {
      return DataResult.error(s + " is not an integer.");
    }
  },
  Integer::toString // Regular function
);
```

--------------------------------

### Define Mod Entrypoint with @Mod in Java (Forge)

Source: https://docs.minecraftforge.net/en/1.20.1/gettingstarted/modfiles

This snippet demonstrates how to define the entrypoint for a Forge mod using the @Mod annotation in Java. The annotation's value must match a mod ID in mods.toml. Initialization logic, such as registering events and DeferredRegisters, can be placed within the class constructor. The mod event bus is accessible via FMLJavaModLoadingContext.

```java
@Mod("examplemod") // Must match mods.toml
public class Example {

  public Example() {
    // Initialize logic here
    var modBus = FMLJavaModLoadingContext.get().getModEventBus();

    // ...
  }
}
```

--------------------------------

### Building LootParams for Loot Table Generation

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/loottables

Illustrates the construction of `LootParams` using a builder pattern. `LootParams` encapsulate the context for loot table generation, including level, luck, and specific parameters.

```java
LootParams params = new LootParams.Builder(lootContextParamSet)
    .setLuck(luckValue)
    .create(lootContextParamSet);
```

--------------------------------

### Defining a Whitelisted Config Value

Source: https://docs.minecraftforge.net/en/1.20.1/misc/config

Demonstrates defining a configuration value that must be one of the values present in a provided collection. The `#defineInList` method is used, requiring the default value, the list of allowed values, and the type class. This ensures the configuration value is restricted to a predefined set.

```java
// Assuming builder is an instance of ForgeConfigSpec.Builder
// and T is the type of elements in the collection
Collection<T> allowedValues = List.of(value1, value2, value3);
ConfigValue<T> whitelistedValue = builder.comment("Value must be one of the allowed options")
  .defineInList("whitelisted_config", defaultValue, allowedValues, type);

```

--------------------------------

### Implement Custom Recipe Interface (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/custom

This Java code snippet demonstrates the basic structure of a custom recipe implementation using a record. It defines input, data, and output fields and implements the Recipe interface for a given container type. The Recipe interface requires methods for matching inputs, assembling the output, and providing the recipe's type and serializer.

```java
public record ExampleRecipe(Ingredient input, int data, ItemStack output) implements Recipe<Container> {
  // Implement methods here
}
```

--------------------------------

### Registering a Common Configuration in Mod Constructor

Source: https://docs.minecraftforge.net/en/1.20.1/misc/config

This snippet demonstrates how to register a common configuration file within the mod's constructor using ModLoadingContext. It specifies the configuration type and the ForgeConfigSpec object. Ensure the ForgeConfigSpec is properly built before registration.

```java
ModLoadingContext.get().registerConfig(Type.COMMON, CONFIG);
```

--------------------------------

### Minecraft Forge: Configure Display Test in mods.toml

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/sides

This configuration in `mods.toml` determines how Minecraft Forge handles version compatibility checks between the client and server for your mod. It's crucial for defining whether your mod is client-only, server-only, or requires matching versions on both sides.

```toml
[[mods]]
  # ...

  # MATCH_VERSION means that your mod will cause a red X if the versions on client and server differ. This is the default behaviour and should be what you choose if you have server and client elements to your mod.
  # IGNORE_SERVER_VERSION means that your mod will not cause a red X if it's present on the server but not on the client. This is what you should use if you're a server only mod.
  # IGNORE_ALL_VERSION means that your mod will not cause a red X if it's present on the client or the server. This is a special case and should only be used if your mod has no server component.
  # NONE means that no display test is set on your mod. You need to do this yourself, see IExtensionPoint.DisplayTest for more information. You can define any scheme you wish with this value.
  # IMPORTANT NOTE: this is NOT an instruction as to which environments (CLIENT or DEDICATED SERVER) your mod loads on. Your mod should load (and maybe do nothing!) whereever it finds itself.
  displayTest="IGNORE_ALL_VERSION" # MATCH_VERSION is the default if nothing is specified (#optional)
```

--------------------------------

### Defining a Boolean Config Value

Source: https://docs.minecraftforge.net/en/1.20.1/misc/config

Illustrates the definition of a simple boolean configuration value. The `#define` method is used with a comment, the configuration key name, and the default boolean value. This is a straightforward way to include toggleable settings in your mod's configuration.

```java
// Assuming builder is an instance of ForgeConfigSpec.Builder
ConfigValue<Boolean> booleanValue = builder.comment("A simple true/false setting")
  .define("boolean_config", true);

```

--------------------------------

### Minecraft Forge: Registering and Referencing Features in Datapacks

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/datapackregistries

This Java code demonstrates how to define `ResourceKey`s for `ConfiguredFeature` and `PlacedFeature`, and then use `RegistrySetBuilder` to register them. It specifically shows how to look up a `ConfiguredFeature` using `bootstrap.lookup` and reference it when registering a `PlacedFeature`, ensuring proper dependency management within datapacks.

```java
public static final ResourceKey<ConfiguredFeature<?, ?>> EXAMPLE_CONFIGURED_FEATURE = ResourceKey.create(
  Registries.CONFIGURED_FEATURE,
  new ResourceLocation(MOD_ID, "example_configured_feature")
);

public static final ResourceKey<PlacedFeature> EXAMPLE_PLACED_FEATURE = ResourceKey.create(
  Registries.PLACED_FEATURE,
  new ResourceLocation(MOD_ID, "example_placed_feature")
);

// In some constant location or argument
new RegistrySetBuilder()
  // Create configured features
  .add(Registries.CONFIGURED_FEATURE, bootstrap -> {
    // Register configured features here
    bootstrap.register(
      // The resource key for the configured feature
      EXAMPLE_CONFIGURED_FEATURE,
      new ConfiguredFeature(/* ... */)
    );
  })
  // Create placed features
  .add(Registries.PLACED_FEATURE, bootstrap -> {
    // Register placed features here

    // Get configured feature registry
    HolderGetter<ConfiguredFeature<?, ?>> configured = bootstrap.lookup(Registries.CONFIGURED_FEATURE);

    bootstrap.register(
      // The resource key for the placed feature
      EXAMPLE_PLACED_FEATURE,
      new PlacedFeature(
        configured.getOrThrow(EXAMPLE_CONFIGURED_FEATURE), // Get the configured feature
        List.of() // and do nothing to the placement location
      )
    )
  });
```

--------------------------------

### Build Complex Conditions with IConditionBuilder (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/recipes

Demonstrates how to construct complex conditions for conditional recipes using methods provided by the `IConditionBuilder` interface. This allows for logical operations like AND, OR, and NOT on various item and condition checks.

```java
// In ConditionalRecipe$Builder#addCondition
(
  // If either 'examplemod:example_item'
  // OR 'examplemod:example_item2' exists
  // AND
  // NOT FALSE

  // Methods are defined by IConditionBuilder
  and(
    or(
      itemExists("examplemod", "example_item"),
      itemExists("examplemod", "example_item2")
    ),
    not(
      FALSE()
    )
  )
)
```

--------------------------------

### Render Labels on Container Screen

Source: https://docs.minecraftforge.net/en/1.20.1/gui/screens

Shows the renderLabels method for drawing text elements on top of the container screen's background but below tooltips. It utilizes the font object to draw strings at specified coordinates, relative to the screen's offset.

```java
// In some AbstractContainerScreen subclass
@Override
protected void renderLabels(GuiGraphics graphics, int mouseX, int mouseY) {
    super.renderLabels(graphics, mouseX, mouseY);

    // Assume we have some Component 'label'
    // 'label' is drawn at 'labelX' and 'labelY'
    graphics.drawString(this.font, this.label, this.labelX, this.labelY, 0x404040);
}
```

--------------------------------

### Create Item, Potion, and VillagerType Tags in Java

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/tags

Demonstrates how to create tag wrappers for Items, Potions, and VillagerTypes using different Forge and Vanilla registry methods. It shows how to define tags for items, potions, and villager types and then how to check if a registry object belongs to a specific tag.

```java
public static final TagKey<Item> myItemTag = ItemTags.create(new ResourceLocation("mymod", "myitemgroup"));

public static final TagKey<Potion> myPotionTag = ForgeRegistries.POTIONS.tags().createTagKey(new ResourceLocation("mymod", "mypotiongroup"));

public static final TagKey<VillagerType> myVillagerTypeTag = TagKey.create(Registries.VILLAGER_TYPE, new ResourceLocation("mymod", "myvillagertypegroup"));

// In some method:

ItemStack stack = /*...*/;
boolean isInItemGroup = stack.is(myItemTag);

Potion potion = /*...*/;
boolean isInPotionGroup  = ForgeRegistries.POTIONS.tags().getTag(myPotionTag).contains(potion);

ResourceKey<VillagerType> villagerTypeKey = /*...*/;
boolean isInVillagerTypeGroup = BuiltInRegistries.VILLAGER_TYPE.getHolder(villagerTypeKey).map(holder -> holder.is(myVillagerTypeTag)).orElse(false);
```

--------------------------------

### Registering MenuType with MenuSupplier

Source: https://docs.minecraftforge.net/en/1.20.1/gui/menus

Demonstrates how to register a custom `MenuType` using a `MenuSupplier` for a new menu. The `MenuSupplier` provides the `MenuType` constructor with a lambda function that creates an instance of the `AbstractContainerMenu` subclass. This is the standard way to register menus that do not require additional data beyond the player's inventory and container ID.

```java
// For some DeferredRegister<MenuType<?>> REGISTER
public static final RegistryObject<MenuType<MyMenu>> MY_MENU = REGISTER.register("my_menu", () -> new MenuType(MyMenu::new, FeatureFlags.DEFAULT_FLAGS));

// In MyMenu, an AbstractContainerMenu subclass
public MyMenu(int containerId, Inventory playerInv) {
  super(MY_MENU.get(), containerId);
  // ...
}
```

--------------------------------

### Registering MenuType with IContainerFactory

Source: https://docs.minecraftforge.net/en/1.20.1/gui/menus

Illustrates registering a `MenuType` that requires additional data on the client using `IContainerFactory`. The `IForgeMenuType.create` method is used, allowing the menu constructor to accept a `FriendlyByteBuf` for extra information sent from the server. This is useful when the menu needs context like the data holder's world position.

```java
// For some DeferredRegister<MenuType<?>> REGISTER
public static final RegistryObject<MenuType<MyMenuExtra>> MY_MENU_EXTRA = REGISTER.register("my_menu_extra", () -> IForgeMenuType.create(MyMenu::new));

// In MyMenuExtra, an AbstractContainerMenu subclass
public MyMenuExtra(int containerId, Inventory playerInv, FriendlyByteBuf extraData) {
  super(MY_MENU_EXTRA.get(), containerId);
  // Store extra data from buffer
  // ...
}
```

--------------------------------

### Configure ObjModelBuilder Custom Loader

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Demonstrates how to use a custom loader, specifically for OBJ models, with a ModelBuilder. It shows setting the model location, flipping the V coordinate, and configuring textures. This requires the ObjModelBuilder and relevant Forge APIs.

```java
builder.customLoader(ObjModelBuilder::begin) // Custom loader 'forge:obj'
  .modelLocation(modLoc("models/block/model.obj")) // Set the OBJ model location
  .flipV(true) // Flips the V coordinate in the supplied .mtl texture
  .end() // Finish custom loader configuration
.texture("particle", mcLoc("block/dirt")) // Set particle texture to dirt
.texture("texture0", mcLoc("block/dirt")); // Set 'texture0' texture to dirt
```

--------------------------------

### Create and Check Trigger Instance in Java

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/advancements

Demonstrates how to create a trigger instance from JSON, read conditions, and check trigger instances against a player and item stack. The trigger method internally calls `SimpleCriterionTrigger#trigger` to manage listener checks.

```java
@Override
public ExampleTriggerInstance createInstance(JsonObject json, ContextAwarePredicate player, DeserializationContext context) {
  // Read conditions from JSON: item
  return new ExampleTriggerInstance(player, item);
}

// This method is unique for each trigger and is as such not overridden
public void trigger(ServerPlayer player, ItemStack stack) {
  this.trigger(player,
    // The condition checker method within the AbstractCriterionTriggerInstance subclass
    triggerInstance -> triggerInstance.matches(stack)
  );
}
```

--------------------------------

### Registering a Block with DeferredRegister

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/registries

Demonstrates the recommended approach using DeferredRegister to register a custom block. It involves creating a DeferredRegister instance, defining a RegistryObject for the block, and registering the DeferredRegister with the mod event bus. This method simplifies static initialization.

```java
private static final DeferredRegister<Block> BLOCKS = DeferredRegister.create(ForgeRegistries.BLOCKS, MODID);

public static final RegistryObject<Block> ROCK_BLOCK = BLOCKS.register("rock", () -> new Block(BlockBehaviour.Properties.of().mapColor(MapColor.STONE)));

public ExampleMod() {
  BLOCKS.register(FMLJavaModLoadingContext.get().getModEventBus());
}
```

--------------------------------

### Encode and Decode Java Objects with JsonOps using Codecs

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Demonstrates encoding Java objects to JSON elements (regular and compressed) and decoding JSON elements back to Java objects using Codecs and JsonOps. Requires Gson's JsonElement and DataFixerUpper's Codec, JsonOps, and DynamicOps.

```java
import com.mojang.serialization.JsonOps;
import com.mojang.serialization.Codec;
import com.google.gson.JsonElement;

// Assume ExampleJavaObject is a defined class
// Assume exampleCodec is an instance of Codec<ExampleJavaObject>
// Assume exampleObject is an instance of ExampleJavaObject
// Assume exampleJson is an instance of JsonElement

// Encode Java object to regular JsonElement
JsonOps.INSTANCE.encodeStart(exampleCodec, exampleObject);

// Encode Java object to compressed JsonElement
JsonOps.COMPRESSED.encodeStart(exampleCodec, exampleObject);

// Decode JsonElement into Java object
// Assume JsonElement was parsed normally
exampleCodec.parse(JsonOps.INSTANCE, exampleJson);
```

--------------------------------

### Creating Localized Components with TextComponentHelper

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/internationalization

Illustrates how to use `TextComponentHelper.createComponentTranslation` to create a localized and formatted `MutableComponent`. This method handles localization eagerly for vanilla clients and lazily for others, making it suitable for servers that need to support vanilla clients.

```java
// `createComponentTranslation(CommandSource, String, Object...)` creates a localized and formatted `MutableComponent`
// depending on a receiver. The localization and formatting is done eagerly if the receiver is a vanilla client.
// If not, the localization and formatting is done lazily with a `Component` containing `TranslatableContents`.
// This is only useful if the server should allow vanilla clients to connect.
```

--------------------------------

### Implementing Ingredient Serializer Accessor

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/ingredients

Shows how an Ingredient subclass should implement the getSerializer method to return its static serializer instance. This is crucial for the ingredient to be correctly serialized and deserialized.

```java
// In some ingredient subclass
@Override
public IIngredientSerializer<? extends Ingredient> getSerializer() {
  return INSTANCE;
}
```

--------------------------------

### Creating LootContext with Random Instance

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/loottables

Shows how to build a `LootContext` which holds the `LootParams` and an optional random seeded instance. This context is used when generating items from a loot table.

```java
LootContext context = new LootContext.Builder(lootParams)
    .withRandom(randomInstance)
    .create();
```

--------------------------------

### Apply Default Value to Codec (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Demonstrates providing a default value when encoding or decoding fails using orElse or orElseGet. This ensures a fallback value is used instead of an error.

```java
public static final Codec<Integer> DEFAULT_CODEC = Codec.INT.orElse(0); // Can also be a supplied value via #orElseGet
```

--------------------------------

### Transform Codecs between Equivalent Types (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Demonstrates transforming between equivalent Java classes using a codec's xmap function. It requires mapping functions for both forward and backward conversion.

```java
public class ClassA {

  public ClassB toB() { /* ... */ }
}

public class ClassB {

  public ClassA toA() { /* ... */ }
}

// Assume there is some codec A_CODEC
public static final Codec<ClassB> B_CODEC = A_CODEC.xmap(ClassA::toB, ClassB::toA);
```

--------------------------------

### Instance Annotated Event Handler for Item Pickup (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/events

Shows how to create an instance-based event handler for the `EntityItemPickupEvent`. This handler is registered by passing an instance of the class to the event bus.

```java
public class MyForgeEventHandler {
    @SubscribeEvent
    public void pickupItem(EntityItemPickupEvent event) {
        System.out.println("Item picked up!");
    }
}

// Registration examples:
// MinecraftForge.EVENT_BUS.register(new MyForgeEventHandler());
// FMLJavaModLoadingContext.get().getModEventBus().register(new MyForgeEventHandler());
```

--------------------------------

### Static Factory Method for Trigger Instance

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/advancements

Provides a static factory method `instance` to easily create instances of a custom trigger. This is useful for data generation and allows for static imports, simplifying code.

```java
public static ExampleTriggerInstance instance(ContextAwarePredicate player, ItemPredicate item) {
  return new ExampleTriggerInstance(player, item);
}
```

--------------------------------

### Encode and Decode Java Objects with NbtOps using Codecs

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Illustrates encoding Java objects to NBT Tags and decoding NBT Tags back to Java objects using Codecs and NbtOps. This involves using DataFixerUpper's Codec, NbtOps, and DynamicOps with Mojang's Tag class.

```java
import com.mojang.serialization.Codec;
import com.mojang.serialization.DynamicOps;
import net.minecraft.nbt.Tag;

// Assume ExampleJavaObject is a defined class
// Assume exampleCodec is an instance of Codec<ExampleJavaObject>
// Assume exampleObject is an instance of ExampleJavaObject
// Assume exampleNbt is an instance of Tag

// Encode Java object to Tag (Note: This example uses JsonOps for encoding to Tag, which is incorrect. NbtOps should be used here for NBT encoding.)
// Correct usage would involve NbtOps.INSTANCE.encodeStart(exampleCodec, exampleObject);
exampleCodec.encodeStart(JsonOps.INSTANCE, exampleObject); // This line is likely incorrect for NBT encoding

// Decode Tag into Java object
// Correct usage with NbtOps:
exampleCodec.parse(NbtOps.INSTANCE, exampleNbt);
```

--------------------------------

### Using Test Command for Relative Position

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Demonstrates how to use the in-game `/test pos` command to obtain the relative coordinates of a structure block. The command can optionally export the result as a local variable initialization.

```text
# Get relative position and store in a variable named 'anchor'
/test pos anchor
```

--------------------------------

### Register LootTableSubProvider Entry (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/loottables

This code illustrates how to create a `LootTableProvider.SubProviderEntry` to include a custom `LootTableSubProvider` in the loot table generation process. It associates the sub-provider with a specific `LootContextParamSet`.

```java
// In the list passed into the LootTableProvider constructor
new LootTableProvider.SubProviderEntry(
  ExampleSubProvider::new,
  // Loot table generator for the 'empty' param set
  LootContextParamSets.EMPTY
)
```

--------------------------------

### Custom Tag Provider Constructor

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/tags

Shows the constructor for a custom `TagsProvider` subclass. It takes essential parameters like `PackOutput`, a future provider for `HolderLookup`, and an `ExistingFileHelper`, and specifies the registry to generate tags for.

```java
public RecipeTypeTagsProvider(PackOutput output, CompletableFuture<HolderLookup.Provider> registries, ExistingFileHelper fileHelper) {
  super(output, Registries.RECIPE_TYPE, registries, MOD_ID, fileHelper);
}
```

--------------------------------

### Registering Ingredient Serializers in Forge

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/ingredients

Demonstrates how to register an ingredient serializer using CraftingHelper within a RegisterEvent. This involves declaring a static instance of the serializer and then registering it with a given registry name.

```java
// In some serializer class
public static final ExampleIngredientSerializer INSTANCE = new ExampleIngredientSerializer();

// In some handler class
public void registerSerializers(RegisterEvent event) {
  event.register(ForgeRegistries.Keys.RECIPE_SERIALIZERS,
    helper -> CraftingHelper.register(registryName, INSTANCE)
  );
}
```

--------------------------------

### Defining a Range Config Value

Source: https://docs.minecraftforge.net/en/1.20.1/misc/config

Illustrates how to define a configuration value that must fall within a specified range. The `#defineInRange` method is used, requiring the minimum and maximum bounds, the default value, and the data type class. This is applicable for `Comparable` types like Integers, Doubles, and Longs.

```java
// Assuming builder is an instance of ForgeConfigSpec.Builder
// and T is a Comparable type like Integer, Double, or Long
ConfigValue<T> rangedValue = builder.comment("Value must be between min and max")
  .defineInRange("ranged_config", defaultValue, minValue, maxValue, type);

```

--------------------------------

### Generate Cooking Recipe with SimpleCookingRecipeBuilder

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/recipes

This snippet demonstrates creating various cooking recipes (smelting, blasting, smoking, campfire) using `SimpleCookingRecipeBuilder`. It specifies input, output, experience, cooking time, unlocking criteria, and saves the recipe.

```java
// In RecipeProvider#buildRecipes(writer)
SimpleCookingRecipeBuilder builder = SimpleCookingRecipeBuilder.smelting(input, RecipeCategory.MISC, result, experience, cookingTime)
  .unlockedBy("criteria", criteria) // How the recipe is unlocked 
  .save(writer); // Add data to builder
```

--------------------------------

### AbstractContainerMenu Constructors

Source: https://docs.minecraftforge.net/en/1.20.1/gui/menus

Shows the required constructor pattern for `AbstractContainerMenu` subclasses. It includes a server-side constructor that takes necessary parameters and a client-side constructor that must also be supplied to the `MenuType`. The client constructor should provide default values for any fields initialized in the server constructor.

```java
// Client menu constructor
public MyMenu(int containerId, Inventory playerInventory) {
  this(containerId, playerInventory);
}

// Server menu constructor
public MyMenu(int containerId, Inventory playerInventory) {
  // ...
}
```

--------------------------------

### Registering Capabilities with @AutoRegisterCapability Annotation

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/capabilities

This snippet shows how to automatically register a capability by annotating the capability interface with `@AutoRegisterCapability`. This simplifies the registration process.

```java
import net.minecraftforge.common.capabilities.AutoRegisterCapability;

@AutoRegisterCapability
public interface IExampleCapability {
  // ... capability methods ...
}
```

--------------------------------

### Implement Recipe Serializer for Custom Recipe (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/custom

This Java code illustrates how to specify the RecipeSerializer for a custom recipe. The `getSerializer()` method returns the registered RecipeSerializer instance responsible for decoding JSONs and handling network communication for the recipe. The RecipeSerializer must implement `fromJson`, `toNetwork`, and `fromNetwork` methods.

```java
// For some RegistryObject<RecipeSerializer> EXAMPLE_SERIALIZER
// In ExampleRecipe
@Override
public RecipeSerializer<?> getSerializer() {
  return EXAMPLE_SERIALIZER.get();
}
```

--------------------------------

### Minecraft Forge Screen Closing Handlers

Source: https://docs.minecraftforge.net/en/1.20.1/gui/screens

Demonstrates the methods used for cleaning up resources when a Minecraft Forge screen is closed. #onClose handles user-initiated closing actions, like saving data, while #removed is for final cleanup before the screen is garbage collected.

```java
// In some Screen subclass

@Override
public void onClose() {
    // Stop any handlers here

    // Call last in case it interferes with the override
    super.onClose();
}

@Override
public void removed() {
    // Reset initial states here

    // Call last in case it interferes with the override
    super.removed()
;}
```

--------------------------------

### Handle Codec Parsing Results with DataResult

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Demonstrates how to handle the DataResult returned by Codec#parse operations, including retrieving the successful result or partially converted object on failure. It shows the use of resultOrPartial and ifPresent methods.

```java
import com.mojang.serialization.DataResult;
import com.mojang.serialization.Codec;
import com.google.gson.JsonElement;

// Assume ExampleJavaObject is a defined class
// Assume exampleCodec is an instance of Codec<ExampleJavaObject>
// Assume exampleJson is an instance of JsonElement

// Decode JsonElement into Java object
DataResult<ExampleJavaObject> result = exampleCodec.parse(JsonOps.INSTANCE, exampleJson);

result
  // Get result or partial on error, report error message
  .resultOrPartial(errorMessage -> { /* Do something with error message */ })
  // If result or partial is present, do something
  .ifPresent(decodedObject -> { /* Do something with decoded object */ });
```

--------------------------------

### Create Integer Range Codec (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Shows how to create a codec that enforces an inclusive integer range using intRange. If the value is outside the specified bounds, it returns an error DataResult, but the value is still provided as a partial result.

```java
public static final Codec<Integer> RANGE_CODEC = Codec.intRange(0, 4);
```

--------------------------------

### WaveFront OBJ Model Loading with Forge

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelloaders

Demonstrates how to configure a model JSON to use Forge's WaveFront OBJ loader. This loader accepts .obj files and can automatically use corresponding .mtl files. The 'flip_v' option can be used to correct texture V-axis orientation.

```json
{
  "loader": "forge:obj",
  "flip_v": true,
  "model": "examplemod:models/block/model.obj",
  "textures": {
    "texture0": "minecraft:block/dirt",
    "particle": "minecraft:block/dirt"
  }
}
```

--------------------------------

### Generate List Codec (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Shows how to create a codec for a list of objects from an existing object codec using Codec.listOf(). Decoded lists are immutable by default; use transformers for mutable lists.

```java
// BlockPos#CODEC is a Codec<BlockPos>
public static final Codec<List<BlockPos>> LIST_CODEC = BlockPos.CODEC.listOf();
```

--------------------------------

### Generate Map Codec (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Demonstrates creating a codec for a map using Codec.unboundedMap(), which accepts codecs for keys and values. Keys must be transformable to/from strings. Decoded maps are immutable by default; use transformers for mutable maps.

```java
// BlockPos#CODEC is a Codec<BlockPos>
public static final Codec<Map<String, BlockPos>> MAP_CODEC = Codec.unboundedMap(Codec.STRING, BlockPos.CODEC);
```

--------------------------------

### Generate Either Codec using Codec#either (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Creates a codec that attempts to decode an object using one of two provided codecs. It first tries the primary codec; if that fails, it attempts decoding with the secondary codec. If both fail, the error from the second codec is reported.

```java
public static final Codec<Either<Integer, String>> EITHER_CODEC = Codec.either(
  Codec.INT,
  Codec.STRING
);
```

--------------------------------

### Implement ForgeAdvancementProvider AdvancementGenerator

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/advancements

This snippet shows the basic structure for implementing the `ForgeAdvancementProvider$AdvancementGenerator` interface. The `generate` method is where the logic for creating and defining advancements resides. It receives registry lookups, a writer for advancements, and a helper for existing files.

```java
// In some subclass of ForgeAdvancementProvider$AdvancementGenerator or as a lambda reference

@Override
public void generate(HolderLookup.Provider registries, Consumer<Advancement> writer, ExistingFileHelper existingFileHelper) {
  // Build advancements here
}
```

--------------------------------

### Scheduling Actions within Game Tests

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Shows how to schedule actions to occur at specific times or intervals during a Game Test's execution using methods like `#runAtTickTime`, `#runAfterDelay`, and `#onEachTick`.

```java
// Run an action on a specific tick
helper.runAtTickTime(15L, () -> {
  // Action to perform at tick 15
});

// Run an action after a delay
helper.runAfterDelay(10L, () -> {
  // Action to perform 10 ticks from now
});

// Run an action every tick
helper.onEachTick(() -> {
  // Action to perform every tick
});
```

--------------------------------

### Minecraft Forge: Register Custom Display Test Extension

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/sides

This Java code snippet demonstrates how to register a custom display test extension point in Minecraft Forge. It allows you to define specific logic for how the client and server determine mod compatibility, useful for one-sided mods.

```java
//Make sure the mod being absent on the other network side does not cause the client to display the server as incompatible
ModLoadingContext.get().registerExtensionPoint(IExtensionPoint.DisplayTest.class, () -> new IExtensionPoint.DisplayTest(() -> NetworkConstants.IGNORESERVERONLY, (a, b) -> true));
```

--------------------------------

### Add Brewing Recipes with BrewingRecipeRegistry - Java

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/incode

Adds custom brewing recipes by calling `BrewingRecipeRegistry#addRecipe` within the `FMLCommonSetupEvent`. This method handles input ingredients, catalysts, and output stacks. Note that this must be called within the synchronous work queue via `#enqueueWork` as it is not thread-safe.

```Java
public void onCommonSetup(@Observes FMLCommonSetupEvent event) {
    event.enqueueWork(() -> {
        // Example: Add a simple brewing recipe
        BrewingRecipeRegistry.addRecipe(new ItemStack(Items.POTION), new ItemStack(Items.GOLDEN_CARROT), new ItemStack(Items.AWKWARD_POTION));

        // Example: Add a brewing recipe with a custom IBrewingRecipe implementation
        BrewingRecipeRegistry.addRecipe(new MyCustomBrewingRecipe());
    });
}
```

--------------------------------

### Basic Item Model Generation in Forge

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Demonstrates how to generate a basic item model using the `basicItem` method within an `ItemModelProvider`. This method automatically sets the parent to 'minecraft:item/generated' and uses the provided item's registry object to define the texture path for 'layer0'.

```java
// In some ItemModelProvider#registerModels

// Will generate 'assets/<modid>/models/item/example_item.json'
// Parent will be 'minecraft:item/generated'
// For the texture key 'layer0'
//  It will be at 'assets/<modid>/textures/item/example_item.png'
this.basicItem(EXAMPLE_ITEM.get());
```

--------------------------------

### Matching Logic for Trigger Instance

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/advancements

Shows the `matches` method within a custom trigger instance, which is responsible for checking if the provided game state (e.g., an ItemStack) meets the defined conditions. This method is specific to the trigger's purpose.

```java
public boolean matches(ItemStack stack) {
  // Since ItemPredicate matches a stack, a stack is the input
  return this.item.matches(stack);
}
```

--------------------------------

### Play Sound from Player (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/gameeffects/sounds

This overridden method in the Player class forwards sound playback to the Level class, including 'this' player. On the server, it plays the sound to everyone except the player. On the client, it does nothing (see LocalPlayer override). Useful for sounds directed at or around a player.

```java
public void playSound(SoundEvent soundIn, float volume, float pitch) {
    this.level.playSound(this, soundIn, this.getSoundCategory(), volume, pitch);
}
```

--------------------------------

### Registering Capabilities with RegisterCapabilitiesEvent

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/capabilities

This snippet demonstrates how to register a custom capability by supplying its class to the `register` method of `RegisterCapabilitiesEvent`. This event is handled on the mod event bus.

```java
import net.minecraftforge.eventbus.api.SubscribeEvent;
import net.minecraftforge.fml.common.Mod;
import net.minecraftforge.common.capabilities.RegisterCapabilitiesEvent;

@Mod.EventBusSubscriber(modid = "your_mod_id")
public class CapabilityEvents {

  @SubscribeEvent
  public static void registerCaps(RegisterCapabilitiesEvent event) {
    event.register(IExampleCapability.class);
  }

  // Define your capability interface here
  public interface IExampleCapability {
    // Capability methods
  }
}
```

--------------------------------

### Registering Model and Block State Providers for Data Generation

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

This code snippet demonstrates how to register custom `ItemModelProvider` and `BlockStateProvider` classes with the `DataGenerator` on the MOD event bus. It utilizes the `GatherDataEvent` to conditionally include client-side assets.

```java
import net.minecraftforge.data.event.GatherDataEvent;
import net.minecraftforge.eventbus.api.SubscribeEvent;
import net.minecraft.data.DataGenerator;
import net.minecraftforge.fml.common.Mod;

// Assuming MOD_ID is defined elsewhere
// Assuming MyItemModelProvider and MyBlockStateProvider are custom provider classes

@Mod.EventBusSubscriber(modid = MOD_ID, bus = Mod.EventBusSubscriber.Bus.MOD)
public class DataGenerators {

    @SubscribeEvent
    public static void gatherData(GatherDataEvent event) {
        DataGenerator gen = event.getGenerator();
        ExistingFileHelper efh = event.getExistingFileHelper();

        gen.addProvider(
            // Tell generator to run only when client assets are generating
            event.includeClient(),
            output -> new MyItemModelProvider(output, MOD_ID, efh)
        );
        gen.addProvider(
            event.includeClient(),
            output -> new MyBlockStateProvider(output, MOD_ID, efh)
        );
    }
}
```

--------------------------------

### Create Basic Game Test Method

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Demonstrates the basic structure of a Game Test method in Java. It requires a `@GameTest` annotation and accepts a `GameTestHelper` object to perform test logic.

```java
public class ExampleGameTests {
  @GameTest
  public static void exampleTest(GameTestHelper helper) {
    // Do stuff
  }
}
```

--------------------------------

### Registering Event Handlers with Mod and Forge Buses (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/events

Demonstrates how to register event handlers for both mod-specific events and general Forge events using their respective event buses. Event handlers can be instance or static methods.

```java
// In the main mod class ExampleMod

// This event is on the mod bus
private void modEventHandler(RegisterEvent event) {
    // Do things here
}

// This event is on the forge bus
private static void forgeEventHandler(AttachCapabilitiesEvent<Entity> event) {
    // ...
}

// In the mod constructor
modEventBus.addListener(this::modEventHandler);
forgeEventBus.addGenericListener(Entity.class, ExampleMod::forgeEventHandler);
```

--------------------------------

### Gather Data for Loot Table Generation (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/loottables

This snippet demonstrates how to register a `LootTableProvider` with the `DataGenerator` on the mod event bus. It specifies when the provider should run (server data generation) and configures the loot table generation process with sub-providers.

```java
// On the MOD event bus
@SubscribeEvent
public void gatherData(GatherDataEvent event) {
    event.getGenerator().addProvider(
        // Tell generator to run only when server data are generating
        event.includeServer(),
        output -> new MyLootTableProvider(
          output,
          // Specify registry names of tables that are required to generate, or can leave empty
          Collections.emptySet(),
          // Sub providers which generate the loot
          List.of(subProvider1, subProvider2, /*...*/)
        )
    );
}
```

--------------------------------

### Custom Criterion Trigger Instance Creation

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/advancements

Demonstrates the creation of a custom criterion trigger instance by subclassing `AbstractCriterionTriggerInstance`. It includes setting up conditions via the constructor and a static factory method for data generation.

```java
public ExampleTriggerInstance(ContextAwarePredicate player, ItemPredicate item) {
  super(ID, player);
  // Store the item condition that must be met
}
```

--------------------------------

### Register Key Mapping - Java

Source: https://docs.minecraftforge.net/en/1.20.1/misc/keymappings

Registers a KeyMapping instance on the physical client using the RegisterKeyMappingsEvent. This ensures the key binding is recognized by the game and can be configured by the player. The KeyMapping is lazily initialized.

```java
// In some physical client only class

// Key mapping is lazily initialized so it doesn't exist until it is registered
public static final Lazy<KeyMapping> EXAMPLE_MAPPING = Lazy.of(() -> /*...*/);

// Event is on the mod event bus only on the physical client
@SubscribeEvent
public void registerBindings(RegisterKeyMappingsEvent event) {
  event.register(EXAMPLE_MAPPING.get());
}
```

--------------------------------

### Handle Client-bound Packets (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/networking/simpleimpl

Handles packets sent from the server to the client. It uses `DistExecutor.unsafeRunWhenOn` to ensure execution only on the client side and `enqueueWork` for thread safety.

```java
// In Packet class
public static void handle(MyClientMessage msg, Supplier<NetworkEvent.Context> ctx) {
  ctx.get().enqueueWork(() ->
    // Make sure it's only executed on the physical client
    DistExecutor.unsafeRunWhenOn(Dist.CLIENT, () -> () -> ClientPacketHandlerClass.handlePacket(msg, ctx))
  );
  ctx.get().setPacketHandled(true);
}

// In ClientPacketHandlerClass
public static void handlePacket(MyClientMessage msg, Supplier<NetworkEvent.Context> ctx) {
  // Do stuff
}
```

--------------------------------

### Registering an IConditionSerializer in Forge 1.20.1

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/conditional

This snippet demonstrates how to declare a static instance of a condition serializer and register it using CraftingHelper. Registration can occur during the RegisterEvent for RecipeSerializers or during FMLCommonSetupEvent.

```java
// In some serializer class
public static final ExampleConditionSerializer INSTANCE = new ExampleConditionSerializer();

// In some handler class
public void registerSerializers(RegisterEvent event) {
  event.register(ForgeRegistries.Keys.RECIPE_SERIALIZERS,
    helper -> CraftingHelper.register(INSTANCE)
  );
}
```

--------------------------------

### Defining an Enum Config Value

Source: https://docs.minecraftforge.net/en/1.20.1/misc/config

Explains how to define a configuration value that must be an enum. The `#defineEnum` method is used, requiring a getter to convert string or integer representations to the enum type, a collection of allowed enum values, and the default enum value. This enforces that the configuration value is one of the specified enum constants.

```java
// Assuming builder is an instance of ForgeConfigSpec.Builder
// and MyEnum is your enum type
Supplier<MyEnum> getter = (str) -> MyEnum.valueOf(str); // Example getter
Collection<MyEnum> enumValues = Arrays.asList(MyEnum.values());
ConfigValue<MyEnum> enumValue = builder.comment("Select an option from the enum")
  .defineEnum("enum_config", defaultValue, getter, enumValues);

```

--------------------------------

### Play Local Sound in ClientLevel (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/gameeffects/sounds

This method in ClientLevel forwards sound playback to the Level class. It modifies the BlockPos by adding 0.5 to each coordinate before passing it to the Level's overload. This is typically used for sounds originating from a specific block's position on the client side.

```java
public void playLocalSound(BlockPos pos, SoundEvent soundEvent, SoundSource source, float volume, float pitch, float distanceDelay) {
    this.level.playSeededSound(pos.getX() + 0.5, pos.getY() + 0.5, pos.getZ() + 0.5, soundEvent, source, volume, pitch, distanceDelay, (long)this.random.nextInt());
}
```

--------------------------------

### Client-Side Localization with I18n

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/internationalization

Demonstrates the usage of the `net.minecraft.client.resources.language.I18n` class for localizing text on the Minecraft client. This method is strictly for client-side code and will crash if used on a server. It takes a translation key and formatting arguments.

```java
// `get(String, Object...)` localizes in the client’s locale with formatting.
// The first parameter is a translation key, and the rest are formatting arguments
// for `String.format(String, Object...)`.
```

--------------------------------

### Registering Blocks with RegisterEvent

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/registries

Illustrates registering blocks using the RegisterEvent lifecycle event. This method involves subscribing to the RegisterEvent and using the provided helper to register blocks by their ResourceLocation. This approach is an alternative to DeferredRegister.

```java
@SubscribeEvent
public void register(RegisterEvent event) {
  event.register(ForgeRegistries.Keys.BLOCKS,
    helper -> {
      helper.register(new ResourceLocation(MODID, "example_block_1"), new Block(...));
      helper.register(new ResourceLocation(MODID, "example_block_2"), new Block(...));
      helper.register(new ResourceLocation(MODID, "example_block_3"), new Block(...));
      // ...
    }
  );
}
```

--------------------------------

### Build Datapack Registries with RegistrySetBuilder

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/datapackregistries

This code illustrates the usage of `RegistrySetBuilder` to define and register datapack registry objects. You can add entries for specific registries, such as `Registries.CONFIGURED_FEATURE` and `Registries.PLACED_FEATURE`, and provide bootstrap consumers to handle the registration logic.

```java
new RegistrySetBuilder()
  // Create configured features
  .add(Registries.CONFIGURED_FEATURE, bootstrap -> {
    // Register configured features here
  })
  // Create placed features
  .add(Registries.PLACED_FEATURE, bootstrap -> {
    // Register placed features here
  });
```

--------------------------------

### Registering Custom Network Packets

Source: https://docs.minecraftforge.net/en/1.20.1/networking/simpleimpl

Registers a custom message type (packet) with the SimpleChannel. This involves providing a unique discriminator ID, the message class, encoding and decoding functions, and a handler for processing the message. These functions can be method references for conciseness.

```java
INSTANCE.registerMessage(id++, MessageClass.class, MessageClass::encode, MessageClass::decode, MessageClass::handle);
```

--------------------------------

### Marking Game Test Success

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Demonstrates methods within `GameTestHelper` to mark a test as successful. `#succeedIf` runs a check immediately, `#succeedWhen` checks every tick until timeout, and `#succeedOnTickWhen` checks on a specific tick.

```java
// Mark test as successful
helper.succeed();

// Succeed if a condition is met immediately
helper.succeedIf(() -> {
  // Assertion logic here
  if (!conditionMet) {
    throw new GameTestAssertException("Condition not met");
  }
});

// Succeed when a condition is met at any tick before timeout
helper.succeedWhen(() -> {
  // Assertion logic here that might take multiple ticks
});

// Succeed only on a specific tick if a condition is met
helper.succeedOnTickWhen(10L, () -> {
  // Assertion logic for tick 10
});
```

--------------------------------

### Loot Table Construction with Builder (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/loottables

This illustrates the initial step in building a loot table using `LootTable#lootTable`. The builder can then be configured with pools, conditions, and modifiers before being finalized.

```java
LootTable.lootTable()
```

--------------------------------

### Registering SoundDefinitionsProvider in DataGenerator

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/sounds

This code snippet demonstrates how to register a custom `SoundDefinitionsProvider` with the `DataGenerator` during the `GatherDataEvent`. It ensures the provider runs only when client assets are being generated.

```java
// On the MOD event bus
@SubscribeEvent
public void gatherData(GatherDataEvent event) {
    event.getGenerator().addProvider(
        // Tell generator to run only when client assets are generating
        event.includeClient(),
        output -> new MySoundDefinitionsProvider(output, MOD_ID, event.getExistingFileHelper())
    );
}
```

--------------------------------

### Define Recipe Type for Custom Recipe (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/custom

This Java code shows how to specify the RecipeType for a custom recipe implementation. The `getType()` method should return the appropriate RecipeType instance, which defines the category or context for the recipe (e.g., SMELTING, BLASTING). If a suitable type doesn't exist, a new one must be registered.

```java
// For some RegistryObject<RecipeType> EXAMPLE_TYPE
// In ExampleRecipe
@Override
public RecipeType<?> getType() {
  return EXAMPLE_TYPE.get();
}
```

--------------------------------

### ItemOverrides Constructor and Accessor

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelloaders/itemoverrides

The ItemOverrides constructor initializes overrides from a list of ItemOverrides. The getOverrides method returns an immutable list of BakedOverrides.

```java
public ItemOverrides()

public ImmutableList<BakedOverride> getOverrides()
```

--------------------------------

### Create Unit Codec for In-Code Values (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Explains how to use Codec.unit to represent an in-code value that does not encode to anything. This is useful for codecs that are part of a data object but not directly represented in the encoded data.

```java
public static final Codec<IForgeRegistry<Block>> UNIT_CODEC = Codec.unit(
  () -> ForgeRegistries.BLOCKS // Can also be a raw value
);
```

--------------------------------

### Static Annotated Event Handler for Arrow Nock (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/events

Illustrates how to create a static event handler for the `ArrowNockEvent`. Static handlers are registered by passing the class itself to the event bus.

```java
public class MyStaticForgeEventHandler {
    @SubscribeEvent
    public static void arrowNocked(ArrowNockEvent event) {
        System.out.println("Arrow nocked!");
    }
}

// Registration example:
// MinecraftForge.EVENT_BUS.register(MyStaticForgeEventHandler.class);
```

--------------------------------

### Generate Smithing Trim Recipe (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/recipes

Generates a smithing recipe specifically for armor trims. It takes a template, base item, and addition item, and can be associated with a recipe category. Unlock criteria and saving are handled similarly to other recipe builders.

```java
// In RecipeProvider#buildRecipes(writer)
SmithingTrimRecipe builder = SmithingTrimRecipe.smithingTrim(template, base, addition, RecipeCategory.MISC)
  .unlocks("criteria", criteria) // How the recipe is unlocked
  .save(writer, name); // Add data to builder
```

--------------------------------

### Convert Tag to JsonElement using DynamicOps

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Shows how to convert an NBT Tag into a JSON Element using the DynamicOps utility. This is useful for cross-format data manipulation. Requires NbtOps, JsonOps, and Mojang's Tag and JsonElement.

```java
import com.mojang.serialization.DynamicOps;
import net.minecraft.nbt.Tag;
import com.google.gson.JsonElement;

// Assume exampleTag is an instance of Tag

// Convert Tag to JsonElement
JsonElement convertedJson = NbtOps.INSTANCE.convertTo(JsonOps.INSTANCE, exampleTag);
```

--------------------------------

### Root Transform Rotation Formats

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelextensions/transforms

Illustrates various ways to specify rotations for root transforms, including single axis-degree mapping, an array of mappings, an array of Euler angles (degrees), and a direct quaternion representation. Each method defines rotation around specific axes or as a combined quaternion.

```json
{
    "rotation": { "x": 90 }
}
```

```json
{
    "rotation": [ { "x": 90 }, { "y": 45 }, { "x": -22.5 } ]
}
```

```json
{
    "rotation": [ 90, 180, 45 ]
}
```

```json
{
    "rotation": [ 0.38268346, 0, 0, 0.9238795 ]
}
```

--------------------------------

### Using RegistryObject to Reference Items and Custom Types

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/registries

Demonstrates how to create and use RegistryObject instances to reference registered items and custom types. RegistryObject allows retrieving references to registered objects after the RegisterEvent has fired. It requires a ResourceLocation and the IForgeRegistry or its name.

```java
public static final RegistryObject<Item> BOW = RegistryObject.create(new ResourceLocation("minecraft:bow"), ForgeRegistries.ITEMS);

// assume that 'neomagicae:mana_type' is a valid registry, and 'neomagicae:coffeinum' is a valid object within that registry
public static final RegistryObject<ManaType> COFFEINUM = RegistryObject.create(new ResourceLocation("neomagicae", "coffeinum"), new ResourceLocation("neomagicae", "mana_type"), "neomagicae");
```

--------------------------------

### Update JSON Format for Forge Update Checker

Source: https://docs.minecraftforge.net/en/1.20.1/misc/updatechecker

This is the structure for the update.json file used by Forge's update checker. It includes the mod's homepage, version-specific changelogs, and promotional information for different Minecraft versions. Adherence to Maven versioning is recommended for compatibility.

```json
{
  "homepage": "<homepage/download page for your mod>",
  "<mcversion>": {
    "<modversion>": "<changelog for this version>", 
    // List all versions of your mod for the given Minecraft version, along with their changelogs
    // ...
  },
  "promos": {
    "<mcversion>-latest": "<modversion>",
    // Declare the latest "bleeding-edge" version of your mod for the given Minecraft version
    "<mcversion>-recommended": "<modversion>",
    // Declare the latest "stable" version of your mod for the given Minecraft version
    // ...
  }
}
```

--------------------------------

### Register ForgeAdvancementProvider for Data Generation

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/advancements

This code snippet demonstrates how to register the `ForgeAdvancementProvider` with the `DataGenerator` on the mod event bus. It utilizes the `GatherDataEvent` to ensure the provider runs only during server data generation. The provider is configured with sub-providers responsible for generating the actual advancements.

```java
// On the MOD event bus
@SubscribeEvent
public void gatherData(GatherDataEvent event) {
    event.getGenerator().addProvider(
        // Tell generator to run only when server data are generating
        event.includeServer(),
        output -> new ForgeAdvancementProvider(
          output,
          event.getLookupProvider(),
          event.getExistingFileHelper(),
          // Sub providers which generate the advancements
          List.of(subProvider1, subProvider2, /*...*/)
        )
    );
}
```

--------------------------------

### Check KeyMapping in GUI keyPressed (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/misc/keymappings

Checks if a KeyMapping is active and matches the input within a GUI's keyPressed method. It uses InputConstants.getKey to create an input from the key and scancode, and isActiveAndMatches to compare it with the KeyMapping.

```java
// In some Screen subclass
@Override
public boolean keyPressed(int key, int scancode, int mods) {
  if (EXAMPLE_MAPPING.get().isActiveAndMatches(InputConstants.getKey(key, scancode))) {
    // Execute logic to perform on key press here
    return true;
  }
  return super.keyPressed(x, y, button);
}
```

--------------------------------

### Adding Items to Creative Tabs (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/items

This code snippet demonstrates how to add items and blocks to a specific creative tab (INGREDIENTS) using the `BuildCreativeModeTabContentsEvent` on the Forge event bus. It assumes you have `RegistryObject<Item>` and `RegistryObject<Block` instances.

```java
// Registered on the MOD event bus
// Assume we have RegistryObject<Item> and RegistryObject<Block> called ITEM and BLOCK
@SubscribeEvent
public void buildContents(BuildCreativeModeTabContentsEvent event) {
  // Add to ingredients tab
  if (event.getTabKey() == CreativeModeTabs.INGREDIENTS) {
    event.accept(ITEM);
    event.accept(BLOCK); // Takes in an ItemLike, assumes block has registered item
  }
}
```

--------------------------------

### Lazy Localization with TranslatableContents

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/internationalization

Shows how to use `TranslatableContents` to create lazily localized and formatted text components. This is ideal for sending messages to players, as it respects their individual locale. Supported format specifiers are %s and numbered arguments like %1$s.

```java
// The first parameter of the `TranslatableContents(String, Object...)` constructor is a translation key,
// and the rest are used for formatting.
// Formatting arguments may be `Component`s that will be inserted into the resulting formatted text
// with all their attributes preserved.
// A `MutableComponent` can be created using `Component#translatable` by passing in the `TranslatableContents`’s parameters.
// It can also be created using `MutableComponent#create` by passing in the `ComponentContents` itself.
```

--------------------------------

### Performing Assertions in Game Tests

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Illustrates the concept of assertions in Game Tests. When a condition is not met, a `GameTestAssertException` should be thrown to indicate failure.

```java
public static void assertSomething(GameTestHelper helper) {
  if (!someCondition) {
    throw new GameTestAssertException("Expected something, but got something else.");
  }
  // If the condition is met, the test continues.
}
```

--------------------------------

### Enable Game Tests in Run Configurations (Gradle)

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Enables Game Tests for a specific run configuration. By default, only 'client', 'server', and 'gameTestServer' have Game Tests enabled.

```gradle
property 'forge.enableGameTest', 'true'
```

--------------------------------

### Handle Server-bound Packets (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/networking/simpleimpl

Handles incoming packets on the server. It uses `enqueueWork` for thread-safe operations and provides access to the sender's player object. Packets are marked as handled using `setPacketHandled`.

```java
public static void handle(MyMessage msg, Supplier<NetworkEvent.Context> ctx) {
  ctx.get().enqueueWork(() -> {
    // Work that needs to be thread-safe (most work)
    ServerPlayer sender = ctx.get().getSender(); // the client that sent this packet
    // Do stuff
  });
  ctx.get().setPacketHandled(true);
}
```

--------------------------------

### Play Sound from Entity (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/gameeffects/sounds

The Entity class's playSound method forwards the call to the Level class, omitting the player. On the server, this plays a sound event to all nearby players. On the client, this method does nothing. It's used for emitting sounds from any non-player entity on the server.

```java
public void playSound(SoundEvent soundIn, float volume, float pitch) {
    this.level.playSound((Player)null, this.getPosX(), this.getPosY(), this.getPosZ(), soundIn, this.getSoundCategory(), volume, pitch);
}
```

--------------------------------

### Registering Game Tests with @GameTestHolder (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Shows how to register all test methods within a class for Game Testing using the `@GameTestHolder` annotation. The annotation's value must be the mod ID for default configurations.

```java
@GameTestHolder(MODID)
public class ExampleGameTests {
  // ...
}
```

--------------------------------

### Generate Pair Codec using Codec#pair (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Creates a codec that encodes or decodes a pair of objects. The pair codec decodes the left object first, then uses the remaining data to decode the right object. This requires codecs to either describe their encoded data (like records) or be augmented into a MapCodec.

```java
public static final Codec<Pair<Integer, String>> PAIR_CODEC = Codec.pair(
  Codec.INT.fieldOf("left").codec(),
  Codec.STRING.fieldOf("right").codec()
);
```

--------------------------------

### Minecraft Forge Screen Ticking Logic

Source: https://docs.minecraftforge.net/en/1.20.1/gui/screens

Shows how to implement ticking logic within a Minecraft Forge screen. The #tick method is used for client-side operations, such as managing the blinking cursor for an EditBox.

```java
// In some Screen subclass
@Override
public void tick() {
    super.tick();

    // Add ticking logic for EditBox in editBox
    this.editBox.tick();
}
```

--------------------------------

### Create Key Mapping with Modifier - Java

Source: https://docs.minecraftforge.net/en/1.20.1/misc/keymappings

Creates a KeyMapping that requires a modifier key (e.g., Shift) to be held down along with the default input. This allows for more complex input schemes and prevents unintended activation.

```java
new KeyMapping(
  "key.examplemod.example3",
  KeyConflictContext.UNIVERSAL,
  KeyModifier.SHIFT, // Default mapping requires shift to be held down
  InputConstants.Type.KEYSYM, // Default mapping is on the keyboard
  GLFW.GLFW_KEY_G, // Default key is G
  "key.categories.misc"
)
```

--------------------------------

### Generate Shapeless Recipe with ShapelessRecipeBuilder

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/recipes

This code illustrates generating a shapeless recipe using `ShapelessRecipeBuilder`. It adds required items to the recipe, defines unlocking criteria, and saves the generated recipe.

```java
// In RecipeProvider#buildRecipes(writer)
ShapelessRecipeBuilder builder = ShapelessRecipeBuilder.shapeless(RecipeCategory.MISC, result)
  .requires(item) // Add item to the recipe
  .unlockedBy("criteria", criteria) // How the recipe is unlocked
  .save(writer); // Add data to builder
```

--------------------------------

### Creating IntegerProperty for Block States

Source: https://docs.minecraftforge.net/en/1.20.1/blocks/states

Demonstrates how to create an IntegerProperty for a block, which allows the block state to hold an integer value within a specified range. This is useful for properties like levels or counts.

```java
IntegerProperty.create("property_name", minimum_value, maximum_value);
```

--------------------------------

### Registering a BlockEntityType in Java

Source: https://docs.minecraftforge.net/en/1.20.1/blockentities

Demonstrates how to register a custom BlockEntityType using Forge's DeferredRegister. It involves creating a builder for the BlockEntityType, specifying the BlockEntity supplier and associated blocks, and then building it. The `build` method takes a Type for DataFixer, which can be null if not used.

```java
public static final RegistryObject<BlockEntityType<MyBE>> MY_BE = REGISTER.register("mybe", () -> BlockEntityType.Builder.of(MyBE::new, validBlocks).build(null));

// In MyBE, a BlockEntity subclass
public MyBE(BlockPos pos, BlockState state) {
  super(MY_BE.get(), pos, state);
}
```

--------------------------------

### Element Face Data in Minecraft Forge Models

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelextensions/facedata

Demonstrates how to specify additional face data at the element level within a Minecraft Forge model. This data can include color, block light, sky light, and ambient occlusion. If a specific face does not have its own 'forge_data', it will inherit from the element's 'forge_data'. This JSON structure is for defining custom element properties.

```json
{
  "elements": [
    {
      "forge_data": {
        "color": "0xFFFF0000",
        "block_light": 15,
        "sky_light": 15,
        "ambient_occlusion": false
      },
      "faces": {
        "north": {
          "forge_data": {
            "color": "0xFFFF0000",
            "block_light": 15,
            "sky_light": 15,
            "ambient_occlusion": false
          }
          // ...
        },
        // ...
      },
      // ...
    }
  ]
}
```

--------------------------------

### Call Custom Criterion Trigger in Java

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/advancements

Shows how to call the `#trigger` method of a custom criterion trigger whenever the associated action is performed. This is typically done in a method where the action's logic resides.

```java
// In some piece of code where the action is being performed
// Where EXAMPLE_CRITERIA_TRIGGER is the custom criteria trigger
public void performExampleAction(ServerPlayer player, ItemStack stack) {
  // Run code to perform action
  EXAMPLE_CRITERIA_TRIGGER.trigger(player, stack);
}
```

--------------------------------

### Implement Custom BEWLR in Item Class (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/items/bewlr

This code snippet shows how to set a custom BlockEntityWithoutLevelRenderer for an item in Minecraft Forge. It requires implementing the `IClientItemExtensions` interface within the `Item#initializeClient` method and overriding `getCustomRenderer` to return your BEWLR instance. Ensure only one instance of a custom BEWLR is created per mod.

```java
import net.minecraft.world.item.ItemStack;
import net.minecraft.client.renderer.block.model.ItemDisplayContext;
import net.minecraft.client.renderer.entity.ItemRenderer;
import net.minecraft.client.resources.model.BakedModel;
import net.minecraft.client.renderer.RenderBuffers;
import net.minecraft.client.multiplayer.ClientLevel;
import net.minecraft.client.renderer.MultiBufferSource;
import com.mojang.blaze3d.vertex.PoseStack;
import net.minecraftforge.client.extensions.common.IClientItemExtensions;
import net.minecraft.world.item.Item;
import net.minecraft.world.item.ItemDisplayContext;
import java.util.function.Consumer;

// Assume myBEWLRInstance is an instance of your custom BlockEntityWithoutLevelRenderer
// Assume this code is within your custom Item class

@Override
public void initializeClient(Consumer<IClientItemExtensions> consumer) {
  consumer.accept(new IClientItemExtensions() {

    @Override
    public BlockEntityWithoutLevelRenderer getCustomRenderer() {
      // Replace 'myBEWLRInstance' with your actual BEWLR instance
      return myBEWLRInstance;
    }
  });
}

// Placeholder for your custom BEWLR instance
private BlockEntityWithoutLevelRenderer myBEWLRInstance = new BlockEntityWithoutLevelRenderer(null, null) {
    // ... your custom rendering logic ...
    @Override
    public void renderByItem(ItemStack itemStack, ItemDisplayContext ctx, PoseStack poseStack, MultiBufferSource bufferSource, int combinedLight, int combinedOverlay) {
        // Your custom rendering implementation here
        super.renderByItem(itemStack, ctx, poseStack, bufferSource, combinedLight, combinedOverlay);
    }
};

// Example of how BlockEntityWithoutLevelRenderer is used internally (simplified)
public static class BlockEntityWithoutLevelRenderer {
    protected ItemRenderer itemRenderer;
    protected ClientLevel level;

    public BlockEntityWithoutLevelRenderer(ItemRenderer itemRenderer, ClientLevel level) {
        this.itemRenderer = itemRenderer;
        this.level = level;
    }

    public void renderByItem(ItemStack itemStack, ItemDisplayContext ctx, PoseStack poseStack, MultiBufferSource bufferSource, int combinedLight, int combinedOverlay) {
        // Default rendering logic or call to specific renderer
        System.out.println("Rendering item: " + itemStack.getItem().getDescriptionId());
    }
}

```

--------------------------------

### Generate Game Test Methods Dynamically

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Illustrates how to generate Game Tests dynamically using a method annotated with `@GameTestGenerator`. This method should return a collection of `TestFunction` objects, allowing for programmatic creation of tests.

```java
public class ExampleGameTests {
  @GameTestGenerator
  public static Collection<TestFunction> exampleTests() {
    // Return a collection of TestFunctions
  }
}
```

--------------------------------

### Check KeyMapping on Client Tick (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/misc/keymappings

Checks if a KeyMapping has been consumed within the game client. This is typically done by listening to the ClientTickEvent on the Forge event bus. It uses a while loop with consumeClick() to handle multiple presses per tick without infinite loops.

```java
public void onClientTick(ClientTickEvent event) {
  if (event.phase == TickEvent.Phase.END) { // Only call code once as the tick event is called twice every tick
    while (EXAMPLE_MAPPING.get().consumeClick()) {
      // Execute logic to perform on click here
    }
  }
}
```

--------------------------------

### Create Dispatch Codec for Polymorphic Data (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/codecs

Enables decoding of different object types based on a type identifier. A dispatch codec first reads a type string, then uses a specific codec associated with that type to decode the rest of the object. This is useful for registries or polymorphic data structures.

```java
// Define our object
public abstract class ExampleObject {

  // Define the method used to specify the object type for encoding
  public abstract Codec<? extends ExampleObject> type();
}

// Create simple object which stores a string
public class StringObject extends ExampleObject {

  public StringObject(String s) { /* ... */ }

  public String s() { /* ... */ }

  public Codec<? extends ExampleObject> type() {
    // A registered registry object
    // "string":
    //   Codec.STRING.xmap(StringObject::new, StringObject::s)
    return STRING_OBJECT_CODEC.get();
  }
}

// Create complex object which stores a string and integer
public class ComplexObject extends ExampleObject {

  public ComplexObject(String s, int i) { /* ... */ }

  public String s() { /* ... */ }

  public int i() { /* ... */ }

  public Codec<? extends ExampleObject> type() {
    // A registered registry object
    // "complex":
    //   RecordCodecBuilder.create(instance ->
    //     instance.group(
    //       Codec.STRING.fieldOf("s").forGetter(ComplexObject::s),
    //       Codec.INT.fieldOf("i").forGetter(ComplexObject::i)
    //     ).apply(instance, ComplexObject::new)
    //   )
    return COMPLEX_OBJECT_CODEC.get();
  }
}

// Assume there is an IForgeRegistry<Codec<? extends ExampleObject>> DISPATCH
public static final Codec<ExampleObject> = DISPATCH.getCodec() // Gets Codec<Codec<? extends ExampleObject>>
  .dispatch(
    ExampleObject::type, // Get the codec from the specific object
    Function.identity() // Get the codec from the registry
  );
```

--------------------------------

### Override toJson for Custom Loader Properties

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/modelproviders

Shows how to override the `toJson` method in a custom loader builder to include additional custom properties in the JSON output. It demonstrates calling the superclass method first and then encoding custom properties.

```java
// In ExampleLoaderBuilder
@Override
public JsonObject toJson(JsonObject json) {
  json = super.toJson(json); // Handle base loader properties
  // Encode custom loader properties
  return json;
}
```

--------------------------------

### Particle Description JSON Structure

Source: https://docs.minecraftforge.net/en/1.20.1/gameeffects/particles

Defines the textures used by particles of type PARTICLE_SHEET_OPAQUE, PARTICLE_SHEET_TRANSLUCENT, and PARTICLE_SHEET_LIT. The 'textures' array contains ResourceLocations pointing to particle textures within the assets directory.

```json
{
  "textures": [
    "mymod:particle_texture",
    "mymod:particle_texture2"
  ]
}
```

--------------------------------

### Item Model Override for Custom Property (JSON)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/client/models/itemproperties

This JSON snippet defines an item model with an override based on a custom property named 'examplemod:power'. It shows how to specify a predicate that checks if the 'examplemod:power' property is greater than or equal to 0.75, and if so, applies a different model ('examplemod:item/example_powered').

```JSON
{
  "parent": "item/generated",
  "textures": {
    // Default
    "layer0": "examplemod:items/example_partial"
  },
  "overrides": [
    {
      // power >= .75
      "predicate": {
        "examplemod:power": 0.75
      },
      "model": "examplemod:item/example_powered"
    }
  ]
}
```

--------------------------------

### Registering a LootItemConditionType with DeferredRegister

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/registries

Shows how to use DeferredRegister to register objects for registries not directly wrapped by Forge, such as LootItemConditionType. It utilizes an overload of DeferredRegister.create to specify the vanilla registry key. Note that dynamic registry objects can only be registered via data files.

```java
private static final DeferredRegister<LootItemConditionType> REGISTER = DeferredRegister.create(Registries.LOOT_CONDITION_TYPE, "examplemod");

public static final RegistryObject<LootItemConditionType> EXAMPLE_LOOT_ITEM_CONDITION_TYPE = REGISTER.register("example_loot_item_condition_type", () -> new LootItemConditionType(...));
```

--------------------------------

### Creating EnumProperty for Block States

Source: https://docs.minecraftforge.net/en/1.20.1/blocks/states

Illustrates the creation of an EnumProperty, allowing a block state to hold values from a specific Enum class. You can specify the entire Enum or a subset of its values.

```java
EnumProperty.create("property_name", EnumClass.class);
```

--------------------------------

### Adding Elements and Tags to a Tag

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/tags

Demonstrates how to use the `TagAppender` returned by `this.tag()` to add individual objects, optional objects from other mods, and entire tags to a defined tag. It also shows how to remove elements and optionally add tags.

```java
// In some TagProvider#addTags
this.tag(EXAMPLE_TAG)
  .add(EXAMPLE_OBJECT) // Adds an object to the tag
  .addOptional(new ResourceLocation("othermod", "other_object")) // Adds an object from another mod to the tag

this.tag(EXAMPLE_TAG_2)
  .addTag(EXAMPLE_TAG) // Adds a tag to the tag
  .remove(EXAMPLE_OBJECT) // Removes an object from this tag
```

--------------------------------

### Serializing Custom Trigger Instance to JSON

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/advancements

Illustrates how to override the `serializeToJson` method to include custom conditions of a trigger instance in its JSON representation. This is essential for data-driven advancement definitions.

```java
@Override
public JsonObject serializeToJson(SerializationContext context) {
  JsonObject obj = super.serializeToJson(context);
  // Write conditions to json
  return obj;
}
```

--------------------------------

### AttributeTagsProvider Constructor - Minecraft Forge Java

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/tags

This Java code defines the constructor for `AttributeTagsProvider`, a subtype of `IntrinsicHolderTagsProvider`. It takes `PackOutput`, `CompletableFuture<HolderLookup.Provider>`, and `ExistingFileHelper` as arguments. The constructor utilizes a lambda expression to extract the `ResourceKey` from an `attribute` object, enabling the object to be added to its tag.

```java
public AttributeTagsProvider(PackOutput output, CompletableFuture<HolderLookup.Provider> registries, ExistingFileHelper fileHelper) {
  super(
    output,
    ForgeRegistries.Keys.ATTRIBUTES,
    registries,
    attribute -> ForgeRegistries.ATTRIBUTES.getResourceKey(attribute).get(),
    MOD_ID,
    fileHelper
  );
}
```

--------------------------------

### Using @ObjectHolder for Field Injection of Registered Objects

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/registries

Illustrates how to use the @ObjectHolder annotation to inject registered objects into public static fields. This method requires specifying the registry name and resource location (or relying on class/mod annotations for defaults). Compile-time exceptions are thrown for invalid or incomplete configurations.

```java
class Holder {
  @ObjectHolder(registryName = "minecraft:enchantment", value = "minecraft:flame")
  public static final Enchantment flame = null;     // Annotation present. [public static] is required. [final] is optional.
                                                    // Registry name is explicitly defined: "minecraft:enchantment"
                                                    // Resource location is explicitly defined: "minecraft:flame"
                                                    // To inject: "minecraft:flame" from the [Enchantment] registry

  public static final Biome ice_flat = null;        // No annotation on the field.
                                                    // Therefore, the field is ignored.

  @ObjectHolder("minecraft:creeper")
  public static Entity creeper = null;              // Annotation present. [public static] is required.
                                                    // The registry has not been specified on the field.
                                                    // Therefore, THIS WILL PRODUCE A COMPILE-TIME EXCEPTION.

  @ObjectHolder(registryName = "potion")
  public static final Potion levitation = null;     // Annotation present. [public static] is required. [final] is optional.
                                                    // Registry name is explicitly defined: "minecraft:potion"
                                                    // Resource location is not specified on the field
                                                    // Therefore, THIS WILL PRODUCE A COMPILE-TIME EXCEPTION.
}
```

--------------------------------

### Registering a Data Provider with DataGenerator

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/tags

This snippet shows how to register a custom tag provider to the `DataGenerator` during the `GatherDataEvent`. It includes filtering for server data generation and instantiating a custom `BlockTagsProvider`.

```java
// On the MOD event bus
@SubscribeEvent
public void gatherData(GatherDataEvent event) {
    event.getGenerator().addProvider(
        // Tell generator to run only when server data are generating
        event.includeServer(),
        // Extends net.minecraftforge.common.data.BlockTagsProvider
        output -> new MyBlockTagsProvider(
          output,
          event.getLookupProvider(),
          MOD_ID,
          event.getExistingFileHelper()
        )
    );
}
```

--------------------------------

### Can Tool Perform Action Condition

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/loottables

Illustrates a Forge-specific loot condition that verifies if the tool provided in the loot context can perform a designated `ToolAction`. This allows conditional loot generation based on tool capabilities.

```json
{
  "condition": "forge:can_tool_perform_action",
  "action": "axe_strip"
}
```

--------------------------------

### Data Generation for Custom Recipes

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/custom

Utilize the RecipeProvider to create a FinishedRecipe for custom recipes, regardless of their input or output data types. This is essential for data generation in Minecraft Forge.

```java
public class RecipeProvider {
    // Method to create a FinishedRecipe for custom recipes
    public FinishedRecipe createRecipe(CustomRecipe recipe) {
        // ... implementation details ...
        return null; // Placeholder
    }
}
```

--------------------------------

### Register Custom Banner Patterns - Java

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/incode

Registers custom banner patterns for use in the loom. This involves creating a `DeferredRegister` for `BannerPattern` and then registering a new `BannerPattern` instance with a unique name and identifier. Patterns not in the `minecraft:no_item_required` tag require an accompanying `BannerPatternItem`.

```Java
public static final DeferredRegister<BannerPattern> REGISTER = DeferredRegister.create(Registries.BANNER_PATTERN, "examplemod");

// Registers a new banner pattern named 'example_pattern' with the identifier 'examplemod:ep'
public static final RegistryObject<BannerPattern> EXAMPLE_PATTERN = REGISTER.register("example_pattern", () -> new BannerPattern("examplemod:ep"));

// Example of how to add this to the BannerPatternItem if it's not 'no_item_required'
// public static final RegistryObject<Item> EXAMPLE_PATTERN_ITEM = // ... register BannerPatternItem ...

// Ensure the registration is called during mod initialization
// @Mod.EventBusSubscriber(modid = "examplemod", bus = Mod.EventBusSubscriber.Bus.MOD)
// public static class Registration {
//     @SubscribeEvent
//     public static void register(RegisterEvent event) {
//         if (event.getRegistryKey().equals(Registries.BANNER_PATTERN.key())) {
//             REGISTER.generateFieldAdditions(event.getRegistry());
//         }
//     }
// }
```

--------------------------------

### Marking BlockEntity as Dirty with ItemStackHandler

Source: https://docs.minecraftforge.net/en/1.20.1/datastorage/capabilities

This snippet illustrates how to ensure capability data for a `BlockEntity` with persistent state is saved. It overrides the `onContentsChanged` method in `ItemStackHandler` to call `setChanged()` on the `BlockEntity` when the inventory contents change.

```java
import net.minecraft.world.level.block.entity.BlockEntity;
import net.minecraftforge.items.ItemStackHandler;
import net.minecraft.world.item.ItemStack;

public class MyBlockEntity extends BlockEntity {

  private final ItemStackHandler inventory = new ItemStackHandler(9) { // Example with 9 slots
    @Override
    protected void onContentsChanged(int slot) {
      super.onContentsChanged(slot);
      setChanged(); // Marks the BlockEntity as dirty
    }
  };

  // Constructor and other BlockEntity methods would go here
  public MyBlockEntity(/* parameters */) {
    // ...
  }

  // You would also need to handle saving and loading the inventory data
}
```

--------------------------------

### Creating BooleanProperty for Block States

Source: https://docs.minecraftforge.net/en/1.20.1/blocks/states

Shows how to create a BooleanProperty, which defines a block state property that can only be either true or false. This is commonly used for on/off states or activation.

```java
BooleanProperty.create("property_name");
```

--------------------------------

### Custom Recipe Logic for Non-Item Recipes

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/custom

Implement custom logic for recipes that do not use items as input or output. This involves adding 'matches' and 'assemble' methods to your custom recipe class and retrieving recipes via RecipeManager.

```java
public interface ExampleRecipe {
    // Checks the block at the position to see if it matches the stored data
    boolean matches(Level level, BlockPos pos);

    // Creates the block state to set the block at the specified position to
    BlockState assemble(RegistryAccess access);
}

public class RecipeManager {
    public Optional<ExampleRecipe> getRecipeFor(Level level, BlockPos pos) {
      return level.getRecipeManager()
        .getAllRecipesFor(exampleRecipeType) // Gets all recipes
        .stream() // Looks through all recipes for types
        .filter(recipe -> recipe.matches(level, pos)) // Checks if the recipe inputs are valid
        .findFirst(); // Finds the first recipe whose inputs match
    }
}
```

--------------------------------

### Registering Language Provider in Forge DataGenerator

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/client/localization

This snippet demonstrates how to register a custom `LanguageProvider` for a mod within the Forge `DataGenerator`. It specifies that the provider should run only during client asset generation and sets the locale to 'en_us' (American English).

```java
// On the MOD event bus
@SubscribeEvent
public void gatherData(GatherDataEvent event) {
    event.getGenerator().addProvider(
        // Tell generator to run only when client assets are generating
        event.includeClient(),
        // Localizations for American English
        output -> new MyLanguageProvider(output, MOD_ID, "en_us")
    );
}
```

--------------------------------

### Conditional Data Structure - JSON

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/conditional

Demonstrates the JSON structure for conditionally loading recipes or advancements. It specifies conditions that must be met for a particular datum to be used. The 'type' field is required for recipes but not advancements.

```json
{
  "type": "forge:conditional",

  "recipes": [
    {
      "conditions": [
        {
        },
        {
        }
      ],
      "recipe": {
      }
    },
    {
    },
  ]
}
```

--------------------------------

### Check KeyMapping in GUI mouseClicked (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/misc/keymappings

Checks if a KeyMapping is active and matches the input within a GUI's mouseClicked method. It uses InputConstants.TYPE.MOUSE.getOrCreate to create an input from the mouse button, and isActiveAndMatches to compare it with the KeyMapping.

```java
// In some Screen subclass
@Override
public boolean mouseClicked(double x, double y, int button) {
  if (EXAMPLE_MAPPING.get().isActiveAndMatches(InputConstants.TYPE.MOUSE.getOrCreate(button))) {
    // Execute logic to perform on mouse click here
    return true;
  }
  return super.mouseClicked(x, y, button);
}
```

--------------------------------

### Add Recipe Provider to Data Generator

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/recipes

This code snippet demonstrates how to add a custom `RecipeProvider` to the Forge data generator. It uses the `@SubscribeEvent` annotation to hook into the `GatherDataEvent` and ensures the provider runs only during server data generation.

```java
// On the MOD event bus
@SubscribeEvent
public void gatherData(GatherDataEvent event) {
    event.getGenerator().addProvider(
        // Tell generator to run only when server data are generating
        event.includeServer(),
        MyRecipeProvider::new
    );
}
```

--------------------------------

### Generate Smithing Transform Recipe (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/recipes

Generates a smithing recipe that transforms one item into another using a template, base item, and addition item. The recipe can have unlock criteria and is saved with a specified name. This builder is suitable for recipes that transform items, including custom ones with specific serializers.

```java
// In RecipeProvider#buildRecipes(writer)
SmithingTransformRecipeBuilder builder = SmithingTransformRecipeBuilder.smithing(template, base, addition, RecipeCategory.MISC, result)
  .unlocks("criteria", criteria) // How the recipe is unlocked
  .save(writer, name); // Add data to builder
```

--------------------------------

### Generate Datapack Registry Objects with Forge

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/datapackregistries

This snippet demonstrates how to add a `DatapackBuiltinEntriesProvider` to the data generator during the `GatherDataEvent`. This provider is responsible for generating datapack registry objects for your mod, ensuring proper referencing of existing datapack entries.

```java
// On the MOD event bus
@SubscribeEvent
public void gatherData(GatherDataEvent event) {
    event.getGenerator().addProvider(
        // Tell generator to run only when server data are generating
        event.includeServer(),
        output -> new DatapackBuiltinEntriesProvider(
          output,
          event.getLookupProvider(),
          // The builder containing the datapack registry objects to generate
          new RegistrySetBuilder().add(/* ... */),
          // Set of mod ids to generate the datapack registry objects of
          Set.of(MOD_ID)
        )
    );
}
```

--------------------------------

### Registering Game Tests with RegisterGameTestsEvent (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/misc/gametest

Illustrates registering Game Tests via the `RegisterGameTestsEvent`. This event listener requires tests to specify their mod ID using `GameTest#templateNamespace` if not using `@GameTestHolder`.

```java
// In some class
public void registerTests(RegisterGameTestsEvent event) {
  event.register(ExampleGameTests.class);
}

// In ExampleGameTests
@GameTest(templateNamespace = MODID)
public static void exampleTest3(GameTestHelper helper) {
  // Perform setup
}
```

--------------------------------

### Generate Conditional Recipe (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/recipes

Builds recipes that are only valid when certain conditions are met. Multiple condition-recipe pairs can be added. It supports generating advancements based on the conditions or setting a specific advancement.

```java
// In RecipeProvider#buildRecipes(writer)
ConditionalRecipe.builder()
  // Add the conditions for the recipe
  .addCondition(...)
  // Add recipe to return when conditions are true
  .addRecipe(...)

  // Add the next conditions for the next recipe
  .addCondition(...)
  // Add next recipe to return when the next conditions are true
  .addRecipe(...)

  // Create conditional advancement which uses the conditions
  // and unlocking advancement in the recipes above
  .generateAdvancement()
  .build(writer, name);
```

--------------------------------

### Storing and Loading BlockEntity Data in Java

Source: https://docs.minecraftforge.net/en/1.20.1/blockentities

Shows how to save and load additional data for a BlockEntity by overriding the `saveAdditional` and `load` methods. These methods are crucial for persisting block entity data when chunks are saved or loaded. Remember to call `setChanged()` when data is modified and to always call super methods.

```java
BlockEntity#saveAdditional(CompoundTag tag)

BlockEntity#load(CompoundTag tag)
```

--------------------------------

### Custom Recipe JSON Structure (JSON)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/custom

This JSON object represents the structure of a custom recipe file. The 'type' field must match the registry name of the custom recipe serializer. Additional fields like 'input', 'data', and 'output' are defined by the specific RecipeSerializer for data decoding.

```json
{
  "type": "examplemod:example_serializer",
  "input": {
    // Some ingredient input
  },
  "data": 0, // Some data wanted for the recipe
  "output": {
    // Some stack output
  }
}
```

--------------------------------

### Forge Extension for Tag Removal

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/tags

This JSON snippet demonstrates a Forge extension to the vanilla tag declaration syntax. The `remove` array allows for the granular removal of specific entries from a tag, acting as a more refined alternative to the `replace` option.

```json
{
  "replace": false,
  "values": [
    "minecraft:gold_ingot"
  ],
  "remove": [
    "minecraft:iron_ingot"
  ]
}
```

--------------------------------

### Adding Properties to BlockStateDefinition

Source: https://docs.minecraftforge.net/en/1.20.1/blocks/states

Demonstrates how to override the createBlockStateDefinition method in a Block class to add custom Property objects to the block's state definition. The StateDefinition$Builder#add method is used to include each desired property.

```java
@Override
protected void createBlockStateDefinition(StateDefinition$Builder<Block, BlockState> builder) {
    builder.add(FACING, OPEN, HINGE, POWERED, HALF);
}
```

--------------------------------

### Handle Anvil Recipes with AnvilUpdateEvent - Java

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes/incode

Modifies anvil recipes by listening to the `AnvilUpdateEvent`. This event allows developers to define the output item, experience cost, and material cost based on the input items and materials provided. The event can be canceled to prevent any output.

```Java
public void onAnvilUpdate(@Observes AnvilUpdateEvent event) {
    ItemStack left = event.getLeft();
    ItemStack right = event.getRight();

    // Check if the left and right items are the desired inputs
    if (left.getItem() == Items.DIAMOND_SWORD && right.getItem() == Items.NETHERITE_INGOT) {
        ItemStack output = new ItemStack(Items.NETHERITE_SWORD);
        // Optionally, apply enchantments or other NBT data to the output
        // output.setTag(left.getTag().copy());

        event.setOutput(output);
        event.setCost(5); // Experience cost
        event.setMaterialCost(1); // Material cost (e.g., 1 Netherite Ingot)
    }
}
```

--------------------------------

### Root Transform Matrix Format

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelextensions/transforms

Specifies a root transform using a raw 3x4 transformation matrix. The matrix is applied in the order of translation, left rotation, scale, and right rotation, with the transformation origin applied last. Custom model loaders may ignore this field.

```json
{
    "transform": {
        "matrix": [
            [ 0, 0, 0, 0 ],
            [ 0, 0, 0, 0 ],
            [ 0, 0, 0, 0 ]
        ]
    }
}
```

--------------------------------

### Generate Special Recipe (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/recipes

Creates a JSON entry for dynamic recipes that don't fit the standard JSON format, such as dying armor or fireworks. It requires a dynamic recipe serializer and is saved with a given name.

```java
// In RecipeProvider#buildRecipes(writer)
SpecialRecipeBuilder.special(dynamicRecipeSerializer)
  .save(writer, name); // Add data to builder
```

--------------------------------

### Register Item Color Handler in Forge

Source: https://docs.minecraftforge.net/en/1.20.1/resources/client/models/tinting

This Java code snippet shows how to register a custom `ItemColor` handler using the `RegisterColorHandlersEvent.Item` event. The `register` method can take the color handler and the items to apply it to. It also supports registering a handler for a `Block`'s associated `BlockItem`.

```java
@SubscribeEvent
public void registerItemColors(RegisterColorHandlersEvent.Item event){
  event.register(myItemColor, coloredItem1, coloredItem2, ...);
}
```

--------------------------------

### Synchronize BlockEntity Data Using Custom Network Message (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/blockentities

This is the most complex but often most optimized method, allowing precise control over synchronized data. It requires setting up custom network messages and sending them via `SimpleChannel#send()`. Ensure safety checks for BlockEntity existence and chunk loading before processing messages.

```java
SimpleChannel#send(PacketDistributor$PacketTarget, MSG)
```

--------------------------------

### Item Exists Condition - JSON

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/conditional

Verifies if a given item has been registered in the current Minecraft instance. Returns true if the item specified by 'item' (in 'modid:item_name' format) exists.

```json
{
  "type": "forge:item_exists",
  "item": "examplemod:example_item"
}
```

--------------------------------

### Implementing Ticking BlockEntities in Java

Source: https://docs.minecraftforge.net/en/1.20.1/blockentities

Explains how to add ticking functionality to a BlockEntity by implementing the `EntityBlock#getTicker` method. This method returns a `BlockEntityTicker` which can be defined by a simple method reference. It's important to avoid complex calculations within the ticker to prevent lag.

```java
// Inside some Block subclass
@Nullable
@Override
public <T extends BlockEntity> BlockEntityTicker<T> getTicker(Level level, BlockState state, BlockEntityType<T> type) {
  return type == MyBlockEntityTypes.MYBE.get() ? MyBlockEntity::tick : null;
}

// Inside MyBlockEntity
public static void tick(Level level, BlockPos pos, BlockState state, MyBlockEntity blockEntity) {
  // Do stuff
}
```

--------------------------------

### Register Block Color Handler in Forge

Source: https://docs.minecraftforge.net/en/1.20.1/resources/client/models/tinting

This Java code snippet demonstrates how to register a custom `BlockColor` handler with the game's `BlockColors` instance during the `RegisterColorHandlersEvent.Block` event. The `register` method accepts the custom color handler and one or more blocks to apply it to.

```java
@SubscribeEvent
public void registerBlockColors(RegisterColorHandlersEvent.Block event){
  event.register(myBlockColor, coloredBlock1, coloredBlock2, ...);
}
```

--------------------------------

### Registering Custom Render Types in Java

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelextensions/rendertypes

Java code snippet demonstrating how to register custom named render types using the RegisterNamedRenderTypesEvent. It shows registering 'special_cutout' and 'special_translucent' with their respective render types and texture sheets.

```java
public static void onRegisterNamedRenderTypes(RegisterNamedRenderTypesEvent event)
{
  event.register("special_cutout", RenderType.cutout(), Sheets.cutoutBlockSheet());
  event.register("special_translucent", RenderType.translucent(), Sheets.translucentCullBlockSheet(), Sheets.translucentItemSheet());
}
```

--------------------------------

### Synchronize BlockEntity Data on Block Update (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/blockentities

This approach requires overriding `getUpdateTag()`, `getUpdatePacket()`, and optionally `onDataPacket()`. `ClientboundBlockEntityDataPacket.create(this)` is used to create the update packet. The server must then notify of the block update using `Level#sendBlockUpdated()` with the `Block#UPDATE_CLIENTS` flag.

```java
@Override
public CompoundTag getUpdateTag() {
  CompoundTag tag = new CompoundTag();
  //Write your data into the tag
  return tag;
}

@Override
public Packet<ClientGamePacketListener> getUpdatePacket() {
  // Will get tag from #getUpdateTag
  return ClientboundBlockEntityDataPacket.create(this);
}

// Can override IForgeBlockEntity#onDataPacket. By default, this will defer to the #load.
```

```java
Level#sendBlockUpdated(BlockPos pos, BlockState oldState, BlockState newState, int flags)
```

--------------------------------

### Implementing a Custom LootModifier in Java

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/glm

Extends the abstract LootModifier class to create custom loot modifications. The constructor accepts an array of LootItemConditions, and the doApply method contains the logic for modifying the loot. The codec method is essential for serialization and deserialization.

```java
public class ExampleModifier extends LootModifier {

  public ExampleModifier(LootItemCondition[] conditionsIn, String prop1, int prop2, Item prop3) {
    super(conditionsIn);
    // Store the rest of the parameters
  }

  @NotNull
  @Override
  protected ObjectArrayList<ItemStack> doApply(ObjectArrayList<ItemStack> generatedLoot, LootContext context) {
    // Modify the loot and return the new drops
  }

  @Override
  public Codec<? extends IGlobalLootModifier> codec() {
    // Return the codec used to encode and decode this modifier
  }
}
```

--------------------------------

### Named Loot Pool Configuration

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/loottables

Demonstrates how to assign a specific name to a loot pool within a loot table JSON. Unnamed pools are automatically given a name based on their hash code prefixed with `custom#`.

```json
{
  "name": "example_pool",
  "rolls": {
    "min": 1,
    "max": 1
  },
  "entries": [
    {
      "type": "minecraft:item",
      "name": "minecraft:diamond"
    }
  ]
}
```

--------------------------------

### ItemOverrides Resolve Method for Dynamic Models

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelloaders/itemoverrides

The resolve method processes an ItemStack's state to return a new BakedModel for rendering. It takes the current BakedModel, ItemStack, ClientLevel, LivingEntity, and an int. It should not mutate the level.

```java
public BakedModel resolve(BakedModel model, ItemStack stack, ClientLevel level, LivingEntity entity, int packedLight)
```

--------------------------------

### Define and Add a Global Loot Modifier

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/glm

This Java code shows how to define an instance of a custom `ExampleModifier` within a `GlobalLootModifierProvider`. It includes setting loot item conditions, such as weather checks, and specifying items to be added to the loot table.

```java
// In some GlobalLootModifierProvider#start
this.add("example_modifier", new ExampleModifier(
  new LootItemCondition[] {
    WeatherCheck.weather().setRaining(true).build() // Executes when raining
  },
  "val1",
  10,
  Items.DIRT
));
```

--------------------------------

### Synchronize BlockEntity Data on LevelChunk Load

Source: https://docs.minecraftforge.net/en/1.20.1/blockentities

This method involves overriding `getUpdateTag()` to collect data for the client and `handleUpdateTag()` to process this data. It's suitable for BlockEntities with minimal data. Excessive data synchronization can cause network congestion, so send only necessary information.

```java
BlockEntity#getUpdateTag()
IForgeBlockEntity#handleUpdateTag(CompoundTag tag)
```

--------------------------------

### Advancement Requirements Definition

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/advancements

Defines the structure for 'requirements' in advancement JSON. It specifies that an advancement is unlocked if any of the inner arrays of criteria are met. This allows for complex unlocking conditions.

```json
{
  "criteria": {
    "example_criterion1": { /*...*/ },
    "example_criterion2": { /*...*/ },
    "example_criterion3": { /*...*/ },
    "example_criterion4": { /*...*/ }
  },

  "requirements": [
    [
      "example_criterion1",
      "example_criterion2"
    ],
    [
      "example_criterion3",
      "example_criterion4"
    ]
  ]
}
```

--------------------------------

### True Condition - JSON

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/conditional

Represents a boolean condition that always evaluates to true. This is useful as a placeholder or when a condition block is syntactically required but should always pass.

```json
{
  "type": "forge:true"
}
```

--------------------------------

### Defining a LootModifier Codec in Java

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/glm

Creates a Codec for a custom LootModifier, enabling its configuration via JSON. It uses RecordCodecBuilder and LootModifier.codecStart to combine condition codecs with custom property codecs, linking them to the modifier's constructor.

```java
// For some DeferredRegister<Codec<? extends IGlobalLootModifier>> REGISTRAR
public static final RegistryObject<Codec<ExampleModifier>> = REGISTRAR.register("example_codec", () ->
  RecordCodecBuilder.create(
    inst -> LootModifier.codecStart(inst).and(
      inst.group(
        Codec.STRING.fieldOf("prop1").forGetter(m -> m.prop1),
        Codec.INT.fieldOf("prop2").forGetter(m -> m.prop2),
        ForgeRegistries.ITEMS.getCodec().fieldOf("prop3").forGetter(m -> m.prop3)
      )
    ).apply(inst, ExampleModifier::new)
  )
);

```

--------------------------------

### Sending and Receiving InterModComms Messages in Forge

Source: https://docs.minecraftforge.net/en/1.20.1/concepts/lifecycle

Illustrates how to send and receive messages between mods using Minecraft Forge's InterModComms system. Messages are sent during InterModEnqueueEvent using sendTo and received during InterModProcessEvent using getMessages. The system is thread-safe and uses ConcurrentMap for message storage.

```java
// Sending messages during InterModEnqueueEvent
InterModComms.sendTo("othermod", "messagekey", () -> "data");

// Receiving messages during InterModProcessEvent
InterModComms.getMessages("mymod", m -> true)
    .forEach(message -> {
        // Process message.message.getSender(), message.message.getKey(), message.message.getMessageData()
    });
```

--------------------------------

### Register Item Property in Minecraft Forge (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/client/models/itemproperties

This Java code snippet demonstrates how to register a custom item property for a specific item. It uses `ItemProperties#register` within `FMLClientSetupEvent.enqueueWork` to ensure thread-safe registration. The property's value is determined by a lambda function that checks if an entity is using the item.

```Java
private void setup(final FMLClientSetupEvent event)
{
  event.enqueueWork(() ->
  {
    ItemProperties.register(ExampleItems.APPLE, 
      new ResourceLocation(ExampleMod.MODID, "pulling"), (stack, level, living, id) -> {
        return living != null && living.isUsingItem() && living.getUseItem() == stack ? 1.0F : 0.0F;
      });
  });
}
```

--------------------------------

### Tag Empty Condition - JSON

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/conditional

Determines if a specified item tag is empty. Returns true if the tag identified by 'tag' (in 'modid:tag_name' format) contains no items.

```json
{
  "type": "forge:tag_empty",
  "tag": "examplemod:example_tag"
}
```

--------------------------------

### Declaring a Block Tag in JSON

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/tags

This JSON structure defines a tag for blocks. It includes options for replacing existing tags, specifying values, and handling optional or non-existent entries. The `id` field allows referencing entries from other mods, and `required: false` prevents errors if the referenced entry is missing.

```json
{
  "replace": false,
  "values": [
    "minecraft:gold_ingot",
    "mymod:my_ingot",
    {
      "id": "othermod:ingot_other",
      "required": false
    }
  ]
}
```

--------------------------------

### BakedOverride Structure in Vanilla JSON

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelloaders/itemoverrides

BakedOverride objects are defined within the 'overrides' array of a vanilla item JSON model. Each override contains a 'predicate' (a map of property names to minimum values) and a 'model' (the target model location if the predicate matches).

```json
{
  // Inside a vanilla JSON item model
  "overrides": [
    {
      // This is an ItemOverride
      "predicate": {
        // This is the Map<ResourceLocation, Float>, containing the names of properties and their minimum values
        "example1:prop": 0.5
      },
      // This is the 'location', or target model, of the override, which is used if the predicate above matches
      "model": "example1:item/model"
    },
    {
      // This is another ItemOverride
      "predicate": {
        "example2:prop": 1
      },
      "model": "example2:item/model"
    }
  ]
}
```

--------------------------------

### Composite Model Visibility Control (JSON)

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelextensions/visibility

Defines a composite model with two parts ('part_one', 'part_two') using the 'forge:composite' loader. It explicitly hides 'part_two' by setting its visibility to false. Child models can override this to control part visibility.

```json
{
  "loader": "forge:composite",
  "children": {
    "part_one": {
      "parent": "mymod:mypartmodel_one"
    },
    "part_two": {
      "parent": "mymod:mypartmodel_two"
    }
  },
  "visibility": {
    "part_two": false
  }
}
```

--------------------------------

### Another Child Model Visibility Override (JSON)

Source: https://docs.minecraftforge.net/en/1.20.1/rendering/modelextensions/visibility

Shows another child model inheriting from a composite model. This configuration explicitly sets 'part_two' to be visible, while 'part_one' will inherit its visibility from the parent composite model.

```json
{
  "parent": "mymod:mycompositemodel",
  "visibility": {
    "part_two": true
  }
}
```

--------------------------------

### And Condition Operator - JSON

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/conditional

A boolean operator condition that evaluates to true only if all nested conditions within the 'values' list are true. Conditions are ANDed together.

```json
{
  "type": "forge:and",
  "values": [
    {
    },
    {
    }
  ]
}
```

--------------------------------

### False Condition - JSON

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/conditional

Represents a boolean condition that always evaluates to false. This can be used to ensure a specific datum is never loaded.

```json
{
  "type": "forge:false"
}
```

--------------------------------

### Mod Loaded Condition - JSON

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/conditional

Checks if a specific mod is currently loaded in the application. Returns true if the mod with the provided 'modid' is present.

```json
{
  "type": "forge:mod_loaded",
  "modid": "examplemod"
}
```

--------------------------------

### Or Condition Operator - JSON

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/conditional

A boolean operator condition that evaluates to true if at least one of the nested conditions within the 'values' list is true. Conditions are ORed together.

```json
{
  "type": "forge:or",
  "values": [
    {
    },
    {
    }
  ]
}
```

--------------------------------

### Not Condition Operator - JSON

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/conditional

A boolean operator condition that inverts the result of a single nested condition. If the nested condition is true, 'forge:not' returns false, and vice versa.

```json
{
  "type": "forge:not",
  "value": {
  }
}
```

--------------------------------

### Register Global Loot Modifier Provider in Forge

Source: https://docs.minecraftforge.net/en/1.20.1/datagen/server/glm

This Java code snippet demonstrates how to register a custom `GlobalLootModifierProvider` with the `DataGenerator` during the server data generation phase. It uses the `@SubscribeEvent` annotation to hook into the `GatherDataEvent`.

```java
// On the MOD event bus
@SubscribeEvent
public void gatherData(GatherDataEvent event) {
    event.getGenerator().addProvider(
        // Tell generator to run only when server data are generating
        event.includeServer(),
        output -> new MyGlobalLootModifierProvider(output, MOD_ID)
    );
}
```

--------------------------------

### Serialized Modifier Data JSON (Forge)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/glm

This JSON file defines the specific properties and conditions for a single global loot modifier. The 'type' field specifies the codec, and 'conditions' lists the criteria for activation. Additional properties can be defined by the custom modifier class, allowing data packs to adjust balance.

```json
// Within data/examplemod/loot_modifiers/example_glm.json
{
  "type": "examplemod:example_loot_modifier",
  "conditions": [
    // Normal loot table conditions
    // ...
  ],
  "prop1": "val1",
  "prop2": 10,
  "prop3": "minecraft:dirt"
}
```

--------------------------------

### Minecraft Recipe Advancement Trigger

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/recipes

This JSON snippet demonstrates how to define a recipe advancement trigger in Minecraft. It uses the 'minecraft:recipe_unlocked' trigger to check if a specific recipe has been unlocked by the player, often as a condition for gaining a recipe or advancement. The 'recipe' field specifies the custom recipe ID.

```json
{
  "has_the_recipe": { 
    "trigger": "minecraft:recipe_unlocked",
    "conditions": {
      "recipe": "examplemod:example_recipe"
    }
  }
}
//...
"requirements": [
  [
    "has_the_recipe"
  ]
]
```

--------------------------------

### Loot Table ID Condition

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/loottables

Shows a Forge-specific loot condition that checks if the current loot table being processed matches a given `loot_table_id`. This is useful in global loot modifiers for targeted application.

```json
{
  "condition": "forge:loot_table_id",
  "loot_table_id": "minecraft:blocks/dirt"
}
```

--------------------------------

### IGlobalLootModifier Implementation (Java)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/glm

The IGlobalLootModifier interface provides the core logic for a modifier. The '#apply' method processes the loot and returns the modified drops, while the '#codec' method returns the registered codec for JSON serialization/deserialization. Implementations can extend 'LootModifier' for base functionality.

```java
public abstract class IGlobalLootModifier implements IForgeRegistryEntry<IGlobalLootModifier> {
    // ...
    public abstract LootContextParamSet getParamSet();
    public abstract Set<LootContextParam<?>> getRequiredParams();
    public abstract List<ItemStack> apply(List<ItemStack> stacks, LootContext context);
    public abstract Codec<IGlobalLootModifier> codec();
    // ...
}
```

--------------------------------

### Registering Global Loot Modifiers in JSON (Forge)

Source: https://docs.minecraftforge.net/en/1.20.1/resources/server/glm

The global_loot_modifiers.json file defines which loot modifiers are loaded into the game. It resides in the 'forge' namespace and specifies an ordered list of modifiers to be applied. The 'replace' option controls whether to append or entirely replace the global list.

```json
{
  "replace": false, // Must be present
  "entries": [
    // Represents a loot modifier in 'data/examplemod/loot_modifiers/example_glm.json'
    "examplemod:example_glm",
    "examplemod:example_glm2"
    // ...
  ]
}
```

=== COMPLETE CONTENT === This response contains all available snippets from this library. No additional content exists. Do not make further requests.
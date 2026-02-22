### Configure Game Test Execution with Annotations

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Shows how to configure the execution of a Game Test using members of the `@GameTest` annotation. This example sets `setupTicks` to 20 for setup time and `required` to false, meaning test failure won't halt the batch.

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

### Querying Vanilla Registries (Java)

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Demonstrates how to retrieve registered objects by their ResourceLocation or get the ResourceLocation of a registered object using NeoForged's BuiltInRegistries. Also shows how to check for the existence of a registry key.

```java
BuiltInRegistries.BLOCKS.get(new ResourceLocation("minecraft", "dirt")); // returns the dirt block  
BuiltInRegistries.BLOCKS.getKey(Blocks.DIRT); // returns the resource location "minecraft:dirt"  
  
// Assume that ExampleBlocksClass.EXAMPLE_BLOCK.get() is a Supplier<Block> with the id "yourmodid:example_block"  
BuiltInRegistries.BLOCKS.get(new ResourceLocation("yourmodid", "example_block")); // returns the example block  
BuiltInRegistries.BLOCKS.getKey(ExampleBlocksClass.EXAMPLE_BLOCK.get()); // returns the resource location "yourmodid:example_block"

BuiltInRegistries.BLOCKS.containsKey(new ResourceLocation("minecraft", "dirt")); // true  
BuiltInRegistries.BLOCKS.containsKey(new ResourceLocation("create", "brass_ingot")); // true only if Create is installed
```

--------------------------------

### Dispatch Codec Setup in Java

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Demonstrates setting up a dispatch codec using Codec#dispatch, which selects a subcodec based on a specified type. This is useful for registries like rule tests or block placers.

```java
// Define our object
abstract class ExampleObject {
  public abstract Codec<? extends ExampleObject> type();
}

// Create simple object which stores a string
class StringObject extends ExampleObject {
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
class ComplexObject extends ExampleObject {
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

### Example: Query Block Item Handler

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Demonstrates querying an IItemHandler capability for a block from the NORTH side, checking if the handler is available before use.

```java
IItemHandler handler = level.getCapability(Capabilities.ItemHandler.BLOCK, pos, Direction.NORTH);
if (handler != null) {
    // Use the handler for some item-related operation.
}

```

--------------------------------

### Java Package Structure Example

Source: https://docs.neoforged.net/docs/1.20.4/gettingstarted/structuring

Illustrates how distinct JAR files can contain classes with the same name in the same package, highlighting the importance of unique top-level packages to avoid conflicts and game crashes. This example shows that 'com.example.ExampleClass' in 'a.jar' would prevent the same class in 'b.jar' from being loaded.

```text
a.jar  
  - com.example.ExampleClass  
b.jar  
  - com.example.ExampleClass // This class will not normally be loaded  
```

--------------------------------

### CompoundIngredient Example (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/ingredients

Demonstrates the structure of a CompoundIngredient, which acts as an OR condition for a list of ingredients. The input stack must match at least one of the provided ingredients.

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

### Java Class Naming Convention Examples

Source: https://docs.neoforged.net/docs/1.20.4/gettingstarted/structuring

Provides examples of common class naming conventions in Java development, particularly for Minecraft mods. It shows how to suffix class names with their type (e.g., Item, Block, Menu) for better clarity and easier identification of their purpose.

```java
// An Item called PowerRing -> PowerRingItem
public class PowerRingItem { ... }

// A Block called NotDirt -> NotDirtBlock
public class NotDirtBlock { ... }

// A menu for an Oven -> OvenMenu
public class OvenMenu { ... }
```

--------------------------------

### EntityLootSubProvider Constructor Example (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

This example shows the constructor for an EntityLootSubProvider subclass. It initializes the superclass with all available feature flags, determining which entity types are enabled for loot table generation.

```java
// In some EntityLootSubProvider subclass  
public MyEntityLootSubProvider() {  
  super(FeatureFlags.REGISTRY.allFlags());  
}
```

--------------------------------

### DifferenceIngredient Example (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/ingredients

Shows the DifferenceIngredient, which performs set subtraction. The input stack must match the 'base' ingredient but not the 'subtracted' ingredient.

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

### Screen Rendering with Background and Widgets in Java

Source: https://docs.neoforged.net/docs/1.20.4/gui/screens

Provides an example of rendering a screen, including drawing the background and then the screen's widgets. The render method is responsible for drawing all visual elements of the screen each frame, and it's crucial to call super.render for widget rendering.

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

### Generating Cooking Recipes with SimpleCookingRecipeBuilder (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/recipes

This example demonstrates how to generate various cooking recipes (smelting, blasting, etc.) using SimpleCookingRecipeBuilder. It shows how to set input, output, experience, cooking time, unlock criteria, and save the recipe.

```java
// In RecipeProvider#buildRecipes(writer)  
SimpleCookingRecipeBuilder builder = SimpleCookingRecipeBuilder.smelting(input, RecipeCategory.MISC, result, experience, cookingTime)  
  .unlockedBy("criteria", criteria) // How the recipe is unlocked   
  .save(writer); // Add data to builder  

```

--------------------------------

### Getting a Model Builder with ModelProvider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Demonstrates the core functionality of ModelProvider: retrieving a BlockModelBuilder or ItemModelBuilder using the `getBuilder(String path)` method. This is the starting point for generating most models.

```java
BlockModelBuilder blockModel = models.getBuilder("my_block");
ItemModelBuilder itemBuilder = itemModels.getBuilder("my_item");
```

--------------------------------

### PartialNBTIngredient Example (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/ingredients

Shows how to use PartialNBTIngredient for a more flexible NBT comparison. It matches against specified keys in the NBT data, not the entire tag. Either 'item' or 'items' can be specified.

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

### Implementing a Custom LootTableSubProvider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

This example demonstrates the basic structure of a custom LootTableSubProvider. It includes the necessary constructor and the generate method, where the logic for creating loot tables is implemented by calling the provided writer.

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

### Initialize RegistrySetBuilder for Datapack Registries

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Demonstrates the initial setup of a `RegistrySetBuilder` to hold entries for multiple datapack registries. This builder serves as a container for defining registry data.

```java
new RegistrySetBuilder()
    .add(Registries.CONFIGURED_FEATURE, bootstrap -> {
    // Register configured features through the bootstrap context (see below)
    })
    .add(Registries.PLACED_FEATURE, bootstrap -> {
    // Register placed features through the bootstrap context (see below)
    });
```

--------------------------------

### Conditional Recipe Example (Mod Loaded)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/conditional

An example of a shaped crafting recipe that will only be loaded if the 'examplemod' is present in the current application. This utilizes the 'neoforge:mod_loaded' condition.

```json
{
  "neoforge:conditions": [
    {
      "type": "neoforge:mod_loaded",
      "modid": "examplemod"
    }
  ],
  
  "type": "minecraft:crafting_shaped",
  "category": "redstone",
  "key": {
    "#": {
      "item": "examplemod:example_planks"
    }
  },
  "pattern": [
    "##",
    "##",
    "##"
  ],
  "result": {
    "count": 3,
    "item": "mymod:compat_door"
  }
}
```

--------------------------------

### Profiling Result Structure Example

Source: https://docs.neoforged.net/docs/1.20.4/misc/debugprofiler

An example of the hierarchical data structure found in the profiling.txt file, showing time spent in different game sections. This helps in identifying performance-intensive areas.

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

### Batching Game Tests with @BeforeBatch and @AfterBatch

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Explains how to group tests into batches using the 'batch' parameter in @GameTest. It also shows how to define setup (@BeforeBatch) and teardown (@AfterBatch) methods for these batches. Batch methods accept a ServerLevel parameter and return nothing.

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

### ContainerData Initialization and Usage in Menu Constructors (Java)

Source: https://docs.neoforged.net/docs/1.20.4/gui/menus

Demonstrates how to initialize and use ContainerData in both client and server-side menu constructors for synchronizing integers. It shows the creation of a SimpleContainerData instance and the use of addDataSlots. The example assumes a fixed ContainerData size for checking.

```java
public class MyMenuAccess {

  // Client menu constructor
  public MyMenuAccess(int containerId, Inventory playerInventory) {
    this(containerId, playerInventory, new SimpleContainerData(3));
  }

  // Server menu constructor
  public MyMenuAccess(int containerId, Inventory playerInventory, ContainerData dataMultiple) {
    // Check if the ContainerData size is some fixed value
    checkContainerDataCount(dataMultiple, 3);

    // Add data slots for handled integers
    this.addDataSlots(dataMultiple);

    // ...
  }

  // Placeholder for checkContainerDataCount method
  private void checkContainerDataCount(ContainerData data, int expectedCount) {
    // Implementation would check data.getCount() == expectedCount
  }

  // Placeholder for addDataSlots method
  private void addDataSlots(ContainerData data) {
    // Implementation would add data slots based on ContainerData
  }

  // Placeholder for Inventory class
  static class Inventory {}

  // Placeholder for ContainerData interface
  interface ContainerData {
    int getCount();
  }

  // Placeholder for SimpleContainerData class
  static class SimpleContainerData implements ContainerData {
    private final int size;

    public SimpleContainerData(int size) {
      this.size = size;
    }

    @Override
    public int getCount() {
      return size;
    }
  }
}
```

--------------------------------

### Implementing ICustomConfigurationTask Interface (Java)

Source: https://docs.neoforged.net/docs/1.20.4/networking/configuration-tasks

This example shows the implementation of the ICustomConfigurationTask interface, which is required for custom configuration tasks. It includes defining a unique task type and implementing the 'run' method to send custom data payloads.

```java
public record MyConfigurationTask implements ICustomConfigurationTask {  
    public static final ConfigurationTask.Type TYPE = new ConfigurationTask.Type(new ResourceLocation("mymod:my_task"));  
      
    @Override  
    public void run(final Consumer<CustomPacketPayload> sender) {  
        final MyData payload = new MyData();  
        sender.accept(payload);  
    }  
  
    @Override  
    public ConfigurationTask.Type type() {  
        return TYPE;  
    }  
}  

```

--------------------------------

### BlockLootSubProvider Constructor Example (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

This snippet illustrates the constructor for a BlockLootSubProvider subclass. It initializes the superclass with an empty set for explosion-resistant items and all available feature flags.

```java
// In some BlockLootSubProvider subclass  
public MyBlockLootSubProvider() {  
  super(Collections.emptySet(), FeatureFlags.REGISTRY.allFlags());  
}
```

--------------------------------

### StrictNBTIngredient Example (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/ingredients

Illustrates the use of StrictNBTIngredient for exact item, damage, and NBT tag matching. The 'type' must be set to 'forge:nbt'.

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

### Item Model with Custom NeoForge Layers

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models

This example demonstrates how to use the 'neoforge:item_layers' loader to apply custom extra face data to specific layers of an item model. It shows how to tint a layer red, set block and sky light to maximum, and disable ambient occlusion for 'layer 1'.

```json
{
  "loader": "neoforge:item_layers",
  "parent": "minecraft:item/generated",
  "textures": {
    "layer0": "minecraft:item/stick",
    "layer1": "minecraft:item/glowstone_dust"
  },
  "neoforge_data": {
    "layers": {
      "1": {
        "color": "0xFFFF0000",
        "block_light": 15,
        "sky_light": 15,
        "ambient_occlusion": false
      }
    }
  }
}
```

--------------------------------

### Set BlockState in Level with Convenience Method (Java)

Source: https://docs.neoforged.net/docs/1.20.4/blocks/states

Provides an example of using the Level#setBlockAndUpdate convenience method, which internally calls Level#setBlock with the default UPDATE_ALL flag. This is a simpler way to update blocks when default behavior is desired.

```java
level.setBlockAndUpdate(blockPos, blockState);

```

--------------------------------

### Generating Shaped Recipes with ShapedRecipeBuilder (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/recipes

This example shows how to use ShapedRecipeBuilder to create shaped recipes. It covers defining the recipe pattern, ingredients, unlock criteria, and saving the recipe.

```java
// In RecipeProvider#buildRecipes(writer)  
ShapedRecipeBuilder builder = ShapedRecipeBuilder.shaped(RecipeCategory.MISC, result)  
  .pattern("a a") // Create recipe pattern  
  .define('a', item) // Define what the symbol represents  
  .unlockedBy("criteria", criteria) // How the recipe is unlocked  
  .save(writer); // Add data to builder  

```

--------------------------------

### IntersectionIngredient Example (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/ingredients

Demonstrates the IntersectionIngredient, which functions as an AND condition. The input stack must satisfy all provided child ingredients. Requires at least two children.

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

### Create an Advancement using Advancement.Builder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/advancements

This example shows how to use the Advancement.Builder to construct a new advancement. It includes adding a criterion, specifying the writer, registry name, and existing file helper to save the advancement.

```java
// In some ForgeAdvancementProvider$AdvancementGenerator#generate(registries, writer, existingFileHelper)  
Advancement example = Advancement.Builder.advancement()  
  .addCriterion("example_criterion", triggerInstance) // How the advancement is unlocked  
  .save(writer, name, existingFileHelper); // Add data to builder
```

--------------------------------

### Datagen Language Provider Example in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/i18n

This Java code demonstrates how to create a custom LanguageProvider for datagening translation files in NeoForge. It extends the LanguageProvider class, initializes it with pack output, mod ID, and locale, and overrides the addTranslations() method to add various types of translations, including simple key-value pairs and specific object types like blocks, items, and entities.

```java
public class MyLanguageProvider extends LanguageProvider {
    public MyLanguageProvider(PackOutput output) {
        super(
                // Provided by the GatherDataEvent.
                output,
                // Your mod id.
                "examplemod",
                // The locale to use. You may use multiple language providers for different locales.
                "en_us"
        );
    }
      
    @Override
    protected void addTranslations() {
        // Adds a translation with the given key and the given value.
        add("translation.key.1", "Translation 1");
          
        // Helpers are available for various common object types. Every helper has two variants: an add() variant
        // for the object itself, and an addTypeHere() variant that accepts a supplier for the object.
        // The different names for the supplier variants are required due to generic type erasure.
        // All following examples assume the existence of the values as suppliers of the needed type.
  
        // Adds a block translation.
        add(MyBlocks.EXAMPLE_BLOCK.get(), "Example Block");
        addBlock(MyBlocks.EXAMPLE_BLOCK, "Example Block");
        // Adds an item translation.
        add(MyItems.EXAMPLE_ITEM.get(), "Example Item");
        addItem(MyItems.EXAMPLE_ITEM, "Example Item");
        // Adds an item stack translation. This is mainly for items that have NBT-specific names.
        add(MyItems.EXAMPLE_ITEM_STACK.get(), "Example Item");
        addItemStack(MyItems.EXAMPLE_ITEM_STACK, "Example Item");
        // Adds an entity type translation.
        add(MyEntityTypes.EXAMPLE_ENTITY_TYPE.get(), "Example Entity");
        addEntityType(MyEntityTypes.EXAMPLE_ENTITY_TYPE, "Example Entity");
        // Adds an enchantment translation.
        add(MyEnchantments.EXAMPLE_ENCHANTMENT.get(), "Example Enchantment");
        addEnchantment(MyEnchantments.EXAMPLE_ENCHANTMENT, "Example Enchantment");
        // Adds a mob effect translation.
        add(MyMobEffects.EXAMPLE_MOB_EFFECT.get(), "Example Effect");
        addEffect(MyMobEffects.EXAMPLE_MOB_EFFECT, "Example Effect");
    }
}
```

--------------------------------

### Named Loot Pool Example (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/loottables

An example of how to define a named loot pool within a JSON loot table. The 'name' key assigns an identifier to the pool, which can be useful for referencing or debugging. Unnamed pools are automatically assigned a hash code prefixed with 'custom#'.

```json
{
  "name": "example_pool",
  "rolls": {
    "min": 1,
    "max": 3
  },
  "entries": [
    {
      "type": "minecraft:item",
      "name": "minecraft:diamond",
      "weight": 1
    }
  ]
}
```

--------------------------------

### Getting a Recipe via RecipeManager (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes

Demonstrates how to retrieve a specific recipe using the RecipeManager in NeoForge. It utilizes the 'getRecipeFor' method, which requires a RecipeType, a Container (or RecipeWrapper), and the current level. This is useful for programmatic recipe lookups.

```java
recipeManger.getRecipeFor(RecipeType.CRAFTING, new RecipeWrapper(handler), level);
```

--------------------------------

### Get BlockState Property Value (Java)

Source: https://docs.neoforged.net/docs/1.20.4/blocks/states

Demonstrates how to retrieve the value of a specific property from a BlockState. This is useful for checking the current state of a block, such as its orientation or other properties.

```java
Direction direction = endPortalFrameBlockState.getValue(EndPortalFrameBlock.FACING);

```

--------------------------------

### Define a Basic Config Value with NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/misc/config

Shows how to define a basic configuration value using `ModConfigSpec.Builder#define`. This method allows specifying a comment, the config value name, and a default value. The value can be retrieved using `ConfigValue#get`.

```java
// For some ModConfigSpec.Builder builder
ConfigValue<T> value = builder.comment("Comment")
  .define("config_value_name", defaultValue);
```

--------------------------------

### Create Custom MobEffect in Java

Source: https://docs.neoforged.net/docs/1.20.4/items/mobeffects

Demonstrates how to create a custom MobEffect by extending the MobEffect class. Includes methods for applying effects each tick, checking if an effect should apply, and logic for when the effect starts. Requires the LivingEntity and MobEffectCategory classes.

```java
public class MyMobEffect extends MobEffect {
    public MyMobEffect(MobEffectCategory category, int color) {
        super(category, color);
    }

    @Override
    public void applyEffectTick(LivingEntity entity, int amplifier) {
        // Apply your effect logic here.
    }

    // Whether the effect should apply this tick. Used e.g. by the Regeneration effect that only applies
    // once every x ticks, depending on the tick count and amplifier.
    @Override
    public boolean shouldApplyEffectTickThisTick(int tickCount, int amplifier) {
        return tickCount % 2 == 0; // replace this with whatever check you want
    }

    // Utility method that is called when the effect is first added to the entity.
    @Override
    public void onEffectStarted(LivingEntity entity, int amplifier) {
    }
}
```

--------------------------------

### Encoded Either<Integer, String> Examples

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Shows the encoded format for Either$Left<Integer, String> and Either$Right<Integer, String>. A simple integer represents the left type, while a JSON string represents the right type.

```json
// Encoded Either$Left<Integer, String>
5

// Encoded Either$Right<Integer, String>
"value"

```

--------------------------------

### Nesting CompoundTags in Java

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/nbt

Demonstrates how to embed one CompoundTag within another using the generic `get` and `put` methods. This allows for hierarchical data structures within NBT.

```java
tag.put("Tag", new CompoundTag());
tag.get("Tag");

```

--------------------------------

### Create a Basic KeyMapping in Java

Source: https://docs.neoforged.net/docs/1.20.4/misc/keymappings

Shows how to create a KeyMapping instance using its constructor. It takes a translation key for its name, a default input, and a category translation key.

```java
import net.minecraft.client.KeyMapping;
import net.minecraft.client.InputConstants;
import org.lwjgl.glfw.GLFW;

new KeyMapping(
  "key.examplemod.example1", // Will be localized using this translation key
  InputConstants.Type.KEYSYM, // Default mapping is on the keyboard
  GLFW.GLFW_KEY_P, // Default key is P
  "key.categories.misc" // Mapping will be in the misc category
)
```

--------------------------------

### Open Server Menu with SimpleMenuProvider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/gui/menus

Demonstrates opening a server-side menu for a player using `NetworkHooks#openScreen` and `SimpleMenuProvider`. This method is used when a menu type is registered, the menu is finished, and a screen is attached.

```java
NetworkHooks.openScreen(serverPlayer, new SimpleMenuProvider(
  (containerId, playerInventory, player) -> new MyMenu(containerId, playerInventory),
  Component.translatable("menu.title.examplemod.mymenu")
));
```

--------------------------------

### Work with NBT ListTag in Java

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/nbt

Explains how to get and create ListTag objects, emphasizing the requirement to specify the list's element type (e.g., TAG_STRING). This is essential for handling ordered collections of NBT data.

```java
ListTag list = tag.getList("SomeListHere", Tag.TAG_STRING);
ListTag list = new ListTag(List.of("Value1", "Value2"), Tag.TAG_STRING);

```

--------------------------------

### Java Module Package Conflict Example

Source: https://docs.neoforged.net/docs/1.20.4/gettingstarted/structuring

Demonstrates a scenario where two modules export the same package ('package X'), which will cause the mod loader to crash on startup. This emphasizes the need for unique package names across modules to ensure proper loading and prevent conflicts.

```text
module A  
  - package X  
    - class I  
    - class J  
module B  
  - package X // This package will cause the mod loader to crash, as there already is a module with package X being exported  
    - class R  
    - class S  
    - class T  
```

--------------------------------

### Create and Use BlockCapabilityCache (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Demonstrates how to create a BlockCapabilityCache for a specific capability, level, position, and context. It also shows how to retrieve the cached capability and handle cases where it might be null. The cache is automatically managed by the garbage collector.

```Java
private BlockCapabilityCache<IItemHandler, @Nullable Direction> capCache;

// Later, for example in `onLoad` for a block entity:
this.capCache = BlockCapabilityCache.create(
        Capabilities.ItemHandler.BLOCK, // capability to cache
        level, // level
        pos, // target position
        Direction.NORTH // context
);

IItemHandler handler = this.capCache.getCapability();
if (handler != null) {
    // Use the handler for some item-related operation.
}
```

--------------------------------

### Encoded Pair<Integer, String> Example

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Illustrates the JSON structure for an encoded Pair<Integer, String> object. The 'fieldOf' method is used to look up keys for the left and right objects within the JSON.

```json
{
  "left": 5,       // fieldOf looks up 'left' key for left object
  "right": "value" // fieldOf looks up 'right' key for right object
}

```

--------------------------------

### Mark Game Test as Successful

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Provides examples of methods within `GameTestHelper` used to mark a Game Test as successful. `#succeed` marks immediate success, `#succeedIf` succeeds if a `Runnable` throws no exception immediately, `#succeedWhen` succeeds if the `Runnable` succeeds on any tick until timeout, and `#succeedOnTickWhen` succeeds only on a specific tick.

```java
helper.succeed();

helper.succeedIf(() -> {
    // Check condition
});

helper.succeedWhen(() -> {
    // Check condition every tick
});

helper.succeedOnTickWhen(tick, () -> {
    // Check condition on a specific tick
});
```

--------------------------------

### Composite Model JSON Example - NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Defines a composite model structure in JSON for NeoForged, allowing for modular model parts. It specifies child models and their visibility, which can be overridden in parent models.

```json
{
  "loader": "neoforge:composite",
  "children": {
    "part_1": {
      "parent": "examplemod:some_model_1"
    },
    "part_2": {
      "parent": "examplemod:some_model_2"
    }
  },
  "visibility": {
    "part_2": false
  }
}
```

```json
{
  "parent": "examplemod:example_composite_model",
  "visibility": {
    "part_1": false,
    "part_2": true
  }
}
```

--------------------------------

### Iterating Through Vanilla Registries (Java)

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Shows how to iterate over all keys (ResourceLocations) or entries (Map.Entry) within a vanilla registry in NeoForged.

```java
for (ResourceLocation id : BuiltInRegistries.BLOCKS.keySet()) {  
    // ...  
}
for (Map.Entry<ResourceLocation, Block> entry : BuiltInRegistries.BLOCKS.entrySet()) {  
    // ...  
}
```

--------------------------------

### Using Loot Tables in Code (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/loottables

Demonstrates how to retrieve and use loot tables in Java code. It involves obtaining the LootDataResolver from the MinecraftServer to get a LootTable, which can then be used with LootParams to generate items. LootParams are built using LootParams.Builder and LootContextParamSet.

```java
MinecraftServer server = ...;
LootDataResolver resolver = server.getLootData();
ResourceLocation lootTableLocation = new ResourceLocation("my_mod", "loot_tables/my_table");
LootTable lootTable = resolver.getLootTable(lootTableLocation);

LootContextParamSet paramSet = LootContextParamSets.EMPTY; // Or other appropriate set
LootParams params = new LootParams.Builder(paramSet).create(new HashSet<>(), new Random());

// To get items:
List<ItemStack> items = lootTable.getRandomItems(params);

// To fill a container:
lContainer.fill(lootTable, params);
```

--------------------------------

### Screen Initialization and Widget Addition in Java

Source: https://docs.neoforged.net/docs/1.20.4/gui/screens

Illustrates the initialization process for a screen, including calling the superclass's init method and adding various types of widgets. The init method is called upon screen initialization and resizing, and is used for setting up widgets and precomputing coordinates.

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

### Registering a Block Entity Type with NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/blockentities

Demonstrates how to register a custom Block Entity type using NeoForged's DeferredRegister. It shows the creation of a BlockEntityType builder, specifying the BlockEntity supplier and associated Blocks, and building the registry object. The example also includes the constructor for the custom BlockEntity subclass.

```java
public static final Supplier<BlockEntityType<MyBE>> MY_BE = REGISTER.register("mybe", () -> BlockEntityType.Builder.of(MyBE::new, validBlocks).build(null));  

// In MyBE, a BlockEntity subclass  
public MyBE(BlockPos pos, BlockState state) {  
  super(MY_BE.get(), pos, state);
}
```

--------------------------------

### Registering a Custom Registry (Java)

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Demonstrates how to register a custom registry to the game's root registry using the NewRegistryEvent in NeoForged.

```java
@SubscribeEvent // on the mod event bus  
public static void registerRegistries(NewRegistryEvent event) {  
    event.register(SPELL_REGISTRY);
}
```

--------------------------------

### Registering Attachment Types with NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/attachments

This Java code demonstrates how to create and register different types of attachments using NeoForged's DeferredRegister. It shows examples of attachments with INBTSerializable, codec serialization, and no serialization. The DeferredRegister must be registered to the mod bus.

```java
private static final DeferredRegister<AttachmentType<?>> ATTACHMENT_TYPES = DeferredRegister.create(NeoForgeRegistries.ATTACHMENT_TYPES, MOD_ID);
  
private static final Supplier<AttachmentType<ItemStackHandler>> HANDLER = ATTACHMENT_TYPES.register(
        "handler", () -> AttachmentType.serializable(() -> new ItemStackHandler(1)).build());
private static final Supplier<AttachmentType<Integer>> MANA = ATTACHMENT_TYPES.register(
        "mana", () -> AttachmentType.builder(() -> 0).serialize(Codec.INT).build());
private static final Supplier<AttachmentType<SomeCache>> SOME_CACHE = ATTACHMENT_TYPES.register(
        "some_cache", () -> AttachmentType.builder(() -> new SomeCache()).build()
);

ATTACHMENT_TYPES.register(modBus);
```

--------------------------------

### Get Relative and Absolute Block Positions

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Illustrates using `GameTestHelper` methods to convert between relative coordinates within a structure template and absolute world coordinates. `absolutePos` converts relative to absolute, and `relativePos` does the reverse.

```java
BlockPos absolutePosition = helper.absolutePos(relativePos);
BlockPos relativePosition = helper.relativePos(absolutePosition);
```

--------------------------------

### Encoded Dispatch Object Examples

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Illustrates JSON structures for encoded objects using a dispatch codec. The 'type' field specifies the object's type, and 'value' is used when the codec is not augmented from MapCodec. Otherwise, fields can be inlined.

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

### Screen Initialization with Constructor in Java

Source: https://docs.neoforged.net/docs/1.20.4/gui/screens

Demonstrates the basic constructor for a screen subclass, accepting a Component for the screen's title. This component is primarily used for narration messages in the base screen.

```java
// In some Screen subclass  
public MyScreen(Component title) {  
    super(title);  
}  

```

--------------------------------

### Creating and Using ResourceLocations in Java

Source: https://docs.neoforged.net/docs/1.20.4/misc/resourcelocation

Demonstrates how to create new ResourceLocation objects in Java, either by providing namespace and path separately or as a combined string. It also shows how to retrieve the namespace, path, and combined string representation of a ResourceLocation.

```java
import net.minecraft.resources.ResourceLocation;

// Creating a new ResourceLocation
ResourceLocation itemLocation = new ResourceLocation("examplemod", "example_item");
ResourceLocation anotherItemLocation = new ResourceLocation("examplemod:example_item");
ResourceLocation defaultNamespaceLocation = new ResourceLocation("example_item"); // Results in minecraft:example_item

// Retrieving parts of a ResourceLocation
String namespace = itemLocation.getNamespace(); // "examplemod"
String path = itemLocation.getPath(); // "example_item"
String combined = itemLocation.toString(); // "examplemod:example_item"

// Utility methods return new ResourceLocations (immutable)
ResourceLocation prefixedLocation = itemLocation.withPrefix("prefix_");
ResourceLocation suffixedLocation = itemLocation.withSuffix("_suffix");
```

--------------------------------

### List Codec Generation (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

The listOf method generates a codec for a list of objects from an existing object codec. Decoded lists are immutable by default; use a transformer for mutable lists. This example creates a codec for a list of BlockPos objects.

```java
// BlockPos#CODEC is a Codec<BlockPos>
public static final Codec<List<BlockPos>> LIST_CODEC = BlockPos.CODEC.listOf();
```

--------------------------------

### Populate Custom Block Tag in JSON

Source: https://docs.neoforged.net/docs/1.20.4/items/tools

Populates a custom block tag defined in Java by listing the block IDs in a JSON file. This example shows blocks that require a copper tool, such as gold ores and redstone ore. The file path follows the pattern `data/<mod_id>/tags/blocks/<tag_name>.json`.

```json
{
  "values": [
    "minecraft:gold_block",
    "minecraft:raw_gold_block",
    "minecraft:gold_ore",
    "minecraft:deepslate_gold_ore",
    "minecraft:redstone_ore",
    "minecraft:deepslate_redstone_ore"
  ]
}
```

--------------------------------

### Create ModConfigSpec Builder and Configure ExampleConfig

Source: https://docs.neoforged.net/docs/1.20.4/misc/config

Demonstrates how to create a `ModConfigSpec` and a corresponding configuration class using `ModConfigSpec.Builder.configure`. This pattern is typically used with a static block to initialize configuration values.

```java
class ExampleConfig {
  // Define values here in final fields
  ExampleConfig(ModConfigSpec.Builder builder) {
    // ... config value definitions ...
  }
}

static {
  Pair<ExampleConfig, ModConfigSpec> pair = new ModConfigSpec.Builder()
    .configure(ExampleConfig::new);
  // Store pair values in some constant field
}
```

--------------------------------

### Range Codecs for Integer Validation (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Range codecs implement flatXMap to ensure values are inclusively between a set minimum and maximum. If a value is outside the bounds, it returns an error DataResult. This example creates a codec for integers between 0 and 4.

```java
public static final Codec<Integer> RANGE_CODEC = Codec.intRange(0, 4);
```

--------------------------------

### Transform Codecs with comapFlatMap (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

The comapFlatMap function is used when a type is partially equivalent, meaning conversion restrictions exist. It returns a DataResult, allowing for error states when exceptions or invalid states are reached. This example demonstrates converting a String to an Integer, where not all strings are valid integers.

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

### Apply BakedModelWrapper in a Client-Side Event Handler (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/bakedmodel

Shows how to apply a custom BakedModelWrapper within a client-side event handler for ModelEvent.ModifyBakingResult. This example demonstrates modifying both block models and item models by using computeIfPresent with the model's resource location and a BiFunction that returns the new wrapped model.

```java
@SubscribeEvent // on the mod event bus only on the physical client
public static void modifyBakingResult(ModelEvent.ModifyBakingResult event) {
    // For block models
    event.getModels().computeIfPresent(
        // The model resource location of the model to modify. Get it from
        // BlockModelShaper#stateToModelLocation with the blockstate to be affected as a parameter.
        BlockModelShaper.stateToModelLocation(MyBlocksClass.EXAMPLE_BLOCK.defaultBlockState()),
        // A BiFunction with the location and the original models as parameters, returning the new model.
        (location, model) -> new MyBakedModelWrapper(model)
    );
    // For item models
    event.getModels().computeIfPresent(
        // The model resource location of the model to modify.
        new ModelResourceLocation("examplemod", "example_item", "inventory"),
        // A BiFunction with the location and the original models as parameters, returning the new model.
        (location, model) -> new MyBakedModelWrapper(model)
    );
}
```

--------------------------------

### Add Brewing Stand Recipes - Java

Source: https://docs.neoforged.net/docs/1.20.4/items/mobeffects

Registers custom brewing recipes for regular, splash, and lingering potions. This code should be called during the setup phase, preferably within an `event.enqueueWork()` call to ensure thread safety, as the brewing recipe registry is not thread-safe. It requires NeoForged and Minecraft's item and potion utilities.

```java
Ingredient brewingIngredient = Ingredient.of(Items.FEATHER);
BrewingRecipeRegistry.addRecipe(
        PotionUtils.setPotion(new ItemStack(Items.POTION), Potions.AWKWARD),
        brewingIngredient,
        PotionUtils.setPotion(new ItemStack(Items.POTION), MY_POTION)
);
BrewingRecipeRegistry.addRecipe(
        PotionUtils.setPotion(new ItemStack(Items.SPLASH_POTION), Potions.AWKWARD),
        brewingIngredient,
        PotionUtils.setPotion(new ItemStack(Items.SPLASH_POTION), MY_POTION)
);
BrewingRecipeRegistry.addRecipe(
        PotionUtils.setPotion(new ItemStack(Items.LINGERING_POTION), Potions.AWKWARD),
        brewingIngredient,
        PotionUtils.setPotion(new ItemStack(Items.LINGERING_POTION), MY_POTION)
);
```

--------------------------------

### Create a KeyMapping for GUI Context in Java

Source: https://docs.neoforged.net/docs/1.20.4/misc/keymappings

Illustrates creating a KeyMapping that is restricted to GUI contexts. It uses a specific KeyConflictContext and a mouse input as the default.

```java
import net.minecraft.client.KeyMapping;
import net.minecraft.client.InputConstants;
import net.neoforged.neoforge.client.settings.KeyConflictContext;
import org.lwjgl.glfw.GLFW;

new KeyMapping(
  "key.examplemod.example2",
  KeyConflictContext.GUI, // Mapping can only be used when a screen is open
  InputConstants.Type.MOUSE, // Default mapping is on the mouse
  GLFW.GLFW_MOUSE_BUTTON_LEFT, // Default mouse input is the left mouse button
  "key.categories.examplemod.examplecategory" // Mapping will be in the new example category
)
```

--------------------------------

### Default Value for Codec Failures (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

The orElse and orElseGet methods provide a default value when encoding or decoding fails. orElse takes a direct value, while orElseGet takes a supplier function. This example shows a codec that defaults to 0 if decoding fails.

```java
public static final Codec<Integer> DEFAULT_CODEC = Codec.INT.orElse(0); // Can also be a supplied value via #orElseGet
```

--------------------------------

### Unit Codec for Non-Encodable Values (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

The Codec.unit function creates a codec that supplies an in-code value and encodes to nothing. This is useful when a codec needs to represent a value that is not directly encodable in the data object. This example creates a unit codec for the block registry.

```java
public static final Codec<IForgeRegistry<Block>> UNIT_CODEC = Codec.unit(
  () -> ForgeRegistries.BLOCKS // Can also be a raw value
);
```

--------------------------------

### Implement Menu Opening in a Block (Java)

Source: https://docs.neoforged.net/docs/1.20.4/gui/menus

Shows how to implement menu opening for a block by overriding `BlockBehaviour#use` and `BlockBehaviour#getMenuProvider`. The `use` method handles the interaction logic, opening the screen on the server side.

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

### Implementing a Custom Integer Merger for Advanced Data Maps (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datamaps

This example shows how to create a custom `DataMapValueMerger` for an advanced data map. The `IntMerger` class merges integer values by summing them when conflicts arise between data packs. It implements the `DataMapValueMerger` interface and overrides the `merge` method to define the merging logic. This is useful for data maps storing numerical collections.

```java
public class IntMerger implements DataMapValueMerger<Item, Integer> {
    @Override
    public Integer merge(Registry<Item> registry, Either<TagKey<Item>, ResourceKey<Item>> first, Integer firstValue, Either<TagKey<Item>, ResourceKey<Item>> second, Integer secondValue) {
        return firstValue + secondValue;
    }
}
```

--------------------------------

### Map Codec Generation (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

The unboundedMap method generates a codec for a map from key and value codecs. It supports any string-based or string-transformed value as a key. Decoded maps are immutable by default; use a transformer for mutable maps. This example creates a codec for a map with String keys and BlockPos values.

```java
// BlockPos#CODEC is a Codec<BlockPos>
public static final Codec<Map<String, BlockPos>> MAP_CODEC = Codec.unboundedMap(Codec.STRING, BlockPos.CODEC);
```

--------------------------------

### Creating a ResourceKey in Java

Source: https://docs.neoforged.net/docs/1.20.4/misc/resourcelocation

Demonstrates how to create a `ResourceKey` using the static `ResourceKey#create` method. This involves specifying the registry key and the registry name. The registry key itself is created using `ResourceKey#createRegistryKey`.

```java
ResourceKey<Registry<Item>> ITEM_REGISTRY_KEY = ResourceKey.createRegistryKey(new ResourceLocation("minecraft", "item"));
ResourceKey<Item> DIAMOND_SWORD_KEY = ResourceKey.create(ITEM_REGISTRY_KEY, new ResourceLocation("minecraft", "diamond_sword"));
```

--------------------------------

### Accessing and Modifying Data Attachments in NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/attachments

This Java code illustrates how to retrieve and set data for registered attachment types on various game objects like item stacks and players. It shows how to get data, check for its existence, and update it. It also highlights the need to manually mark block entities and chunks as dirty when modifying data obtained via getData.

```java
// Get the ItemStackHandler if it already exists, else attach a new one:
ItemStackHandler stackHandler = stack.getData(HANDLER);
// Get the current player mana if it is available, else attach 0:
int playerMana = player.getData(MANA);

// Check if the stack has the HANDLER attachment before doing anything.
if (stack.hasData(HANDLER)) {
    ItemStackHandler stackHandler = stack.getData(HANDLER);
    // Do something with stack.getData(HANDLER).
}

// Increment mana by 10.
player.setData(MANA, player.getData(MANA) + 10);

chunk.setData(MANA, chunk.getData(MANA) + 10); // will call setUnsaved automatically

var mana = chunk.getData(MUTABLE_MANA);
mana.set(10);
chunk.setUnsaved(true); // must be done manually because we did not use setData
```

--------------------------------

### Loot Table ID Condition (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/loottables

An example of the 'forge:loot_table_id' condition used in JSON loot tables. This condition allows loot generation to be restricted to specific loot table IDs, commonly used within global loot modifiers to target particular tables.

```json
{
  "conditions": [
    {
      "condition": "forge:loot_table_id",
      "loot_table_id": "minecraft:blocks/dirt"
    }
  ]
}
```

--------------------------------

### Lookup and Register Entries using BootstrapContext

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Illustrates how to use `BootstrapContext` to look up entries from one registry (e.g., `CONFIGURED_FEATURE`) and use them when registering entries in another registry (e.g., `PLACED_FEATURE`). This is useful for establishing relationships between different registry types.

```java
public static final ResourceKey<ConfiguredFeature<?, ?>> EXAMPLE_CONFIGURED_FEATURE = ResourceKey.create(
    Registries.CONFIGURED_FEATURE,
    new ResourceLocation(MOD_ID, "example_configured_feature")
);
public static final ResourceKey<PlacedFeature> EXAMPLE_PLACED_FEATURE = ResourceKey.create(
    Registries.PLACED_FEATURE,
    new ResourceLocation(MOD_ID, "example_placed_feature")
);

new RegistrySetBuilder()
    .add(Registries.CONFIGURED_FEATURE, bootstrap -> {
        bootstrap.register(EXAMPLE_CONFIGURED_FEATURE, ...);
    })
    .add(Registries.PLACED_FEATURE, bootstrap -> {
        HolderGetter<ConfiguredFeature<?, ?>> otherRegistry = bootstrap.lookup(Registries.CONFIGURED_FEATURE);
        bootstrap.register(EXAMPLE_PLACED_FEATURE, new PlacedFeature(
            otherRegistry.getOrThrow(EXAMPLE_CONFIGURED_FEATURE), // Get the configured feature
            List.of() // No-op when placement happens - replace with whatever your placement parameters are
        ));
    });
```

--------------------------------

### Creating and Manipulating ItemStacks in Java

Source: https://docs.neoforged.net/docs/1.20.4/items

Illustrates how to create new ItemStacks, access their components (item, count, NBT data), and modify them. It also covers handling empty ItemStacks and creating immutable copies.

```java
// Creating a new ItemStack
ItemStack newItemStack = new ItemStack(Item);

// Accessing ItemStack components
Item item = itemstack.getItem();
int count = itemstack.getCount();
CompoundTag nbt = itemstack.getTag(); // or itemstack.getOrCreateTag()

// Modifying ItemStack
itemstack.setCount(int);
itemstack.shrink(int);
itemstack.setTag(CompoundTag);

// Checking for empty ItemStack
boolean isEmpty = itemstack.isEmpty();

// Creating an immutable copy
ItemStack copiedStack = itemstack.copy();

// Representing an empty stack
ItemStack.EMPTY;
```

--------------------------------

### Define Custom Tool Tier in Java

Source: https://docs.neoforged.net/docs/1.20.4/items/tools

Defines a custom tool tier (COPPER_TIER) for NeoForged items. This involves setting properties like mining level, durability, mining speed, attack damage, enchantability, the tag for blocks the tool can break, and the repair ingredient. This example places copper tools between stone and iron.

```java
public static final Tier COPPER_TIER = new SimpleTier(
        // Determines the level of this tool. Since this is an int, there is no good way to place our tool between stone and iron.
        // NeoForge introduces the TierSortingRegistry to solve this problem, see below for more information. Use a best-effort approximation here.
        // Stone is 1, iron is 2.
        1,
        // Determines the durability of the tier.
        // Stone is 131, iron is 250.
        200,
        // Determines the mining speed of the tier. Unused by swords.
        // Stone uses 4, iron uses 6.
        5f,
        // Determines the attack damage bonus. Different tools use this differently. For example, swords do (getAttackDamageBonus() + 4) damage.
        // Stone uses 1, iron uses 2, corresponding to 5 and 6 attack damage for swords, respectively; our sword does 5.5 damage now.
        1.5f,
        // Determines the enchantability of the tier. This represents how good the enchantments on this tool will be.
        // Gold uses 22, we put copper slightly below that.
        20,
        // The tag that determines what blocks this tool can break. See below for more information.
        MyBlockTags.NEEDS_COPPER_TOOL,
        // Determines the repair ingredient of the tier. Use a supplier for lazy initializing.
        () -> Ingredient.of(Tags.Items.INGOTS_COPPER)
);

```

--------------------------------

### Create MobEffectInstance in Java

Source: https://docs.neoforged.net/docs/1.20.4/items/mobeffects

Demonstrates how to create a MobEffectInstance with various parameters like effect type, duration, amplifier, and visibility settings. Several constructor overloads are available to omit optional parameters.

```java
MobEffectInstance instance = new MobEffectInstance(  
        // The mob effect to use.  
        MobEffects.REGENERATION,  
        // The duration to use, in ticks. Defaults to 0 if not specified.  
        500,  
        // The amplifier to use. This is the "strength" of the effect, i.e. Strength I, Strength II, etc;  
        // starting at 0. Defaults to 0 if not specified.  
        0,  
        // Whether the effect is an "ambient" effect, meaning it is being applied by an ambient source,  
        // of which Minecraft currently has the beacon and the conduit. Defaults to false if not specified.  
        false,  
        // Whether the effect is visible in the inventory. Defaults to true if not specified.  
        true,  
        // Whether an effect icon is visible in the top right corner. Defaults to true if not specified.  
        true  
);  

```

--------------------------------

### Implement Menu Opening in a Mob (Java)

Source: https://docs.neoforged.net/docs/1.20.4/gui/menus

Illustrates how a mob can implement menu opening by overriding `Mob#mobInteract` and implementing the `MenuProvider` interface. This allows the mob itself to provide the menu details.

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

### Render Container Screen with Tooltips (Java)

Source: https://docs.neoforged.net/docs/1.20.4/gui/screens

Overrides the render method to first render the background, then call the superclass method for the container screen's default rendering, and finally render tooltips.

```java
import net.minecraft.client.gui.GuiGraphics;
import net.minecraft.client.gui.screens.inventory.AbstractContainerScreen;

// In some AbstractContainerScreen subclass  
@Override  
public void render(GuiGraphics graphics, int mouseX, int mouseY, float partialTick) {  
    this.renderBackground(graphics);
    super.render(graphics, mouseX, mouseY, partialTick);  
  
    /*  
     - This method is added by the container screen to render  
     - the tooltip of the hovered slot.  
     -/
    this.renderTooltip(graphics, mouseX, mouseY);
}

```

--------------------------------

### Implementing Click and Hover Events for Text

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/i18n

Illustrates how to create and apply click and hover events to text components. Supports various actions like opening URLs, running commands, and showing tooltips.

```java
// We have a total of 6 options for a click event, and a total of 3 options for a hover event.
ClickEvent clickEvent;
HoverEvent hoverEvent;

// Opens the given URL in your default browser when clicked.
clickEvent = new ClickEvent(ClickEvent.Action.OPEN_URL, "http://example.com/");
// Opens the given file when clicked. For security reasons, this cannot be sent from a server.
clickEvent = new ClickEvent(ClickEvent.Action.OPEN_FILE, "C:/example.txt");
// Runs the given command when clicked.
clickEvent = new ClickEvent(ClickEvent.Action.RUN_COMMAND, "/gamemode creative");
// Suggests the given command in the chat when clicked.
clickEvent = new ClickEvent(ClickEvent.Action.SUGGEST_COMMAND, "/gamemode creative");
// Changes a book page when clicked. Irrelevant outside of a book screen context.
clickEvent = new ClickEvent(ClickEvent.Action.CHANGE_PAGE, "1");
// Copies the given text to the clipboard.
clickEvent = new ClickEvent(ClickEvent.Action.COPY_TO_CLIPBOARD, "Hello World!");

// Shows the given component when hovered. May be formatted as well.
// Keep in mind that click or hover events won't work in a hover tooltip for obvious reasons.
hoverEvent = new HoverEvent(HoverEvent.Action.SHOW_TEXT, Component.literal("Hello World!"));
// Shows a complete tooltip of the given item stack when hovered.
// See the possible constructors of ItemStackInfo.
hoverEvent = new HoverEvent(HoverEvent.Action.SHOW_ITEM, new HoverEvent.ItemStackInfo(...));
// Shows a complete tooltip of the given entity when hovered.
// See the possible constructors of EntityTooltipInfo.
hoverEvent = new HoverEvent(HoverEvent.Action.SHOW_ENTITY, new HoverEvent.EntityTooltipInfo(...));

// Apply the events to a style.
Style clickable = Style.EMPTY.withClickEvent(clickEvent);
Style hoverable = Style.EMPTY.withHoverEvent(hoverEvent);
```

--------------------------------

### Applying Styles to Text Components

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/i18n

Demonstrates creating and applying various styles (color, italic, bold, etc.) to text components using the Style API. Styles are immutable and can be merged.

```java
Component text = Component.literal("Hello World!");

// Create a new style.
Style blue = Style.EMPTY.withColor(0x0000FF);
// Styles use a builder-like pattern.
Style blueItalic = Style.EMPTY.withColor(0x0000FF).withItalic(true);
// Besides italic, we can also make styles bold, underlined, strikethrough, or obfuscated.
Style bold          = Style.EMPTY.withBold(true);
Style underlined    = Style.EMPTY.withUnderlined(true);
Style strikethrough = Style.EMPTY.withStrikethrough(true);
Style obfuscated    = Style.EMPTY.withObfuscated(true);
// Let's merge some styles together!
Style merged = blueItalic.applyTo(bold).applyTo(strikethrough);

// Set a style on a component.
text.setStyle(merged);
// Merge a new style into it.
text.withStyle(Style.EMPTY.withColor(0xFF0000));
```

--------------------------------

### Creating a Custom Registry Key and Registry (Java)

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Defines a ResourceKey for a custom registry and then creates the registry itself using RegistryBuilder, with options for syncing, default keys, and maximum ID.

```java
// We use spells as an example for the registry here, without any details about what a spell actually is (as it doesn't matter).  
// Of course, all mentions of spells can and should be replaced with whatever your registry actually is.  
public static final ResourceKey<Registry<Spell>> SPELL_REGISTRY_KEY = ResourceKey.createRegistryKey(new ResourceLocation("yourmodid", "spells"));  
public static final Registry<YourRegistryContents> SPELL_REGISTRY = new RegistryBuilder<>(SPELL_REGISTRY_KEY)  
        // If you want to enable integer id syncing, for networking.  
        // These should only be used in networking contexts, for example in packets or purely networking-related NBT data.  
        .sync(true)  
        // The default key. Similar to minecraft:air for blocks. This is optional.  
        .defaultKey(new ResourceLocation("yourmodid", "empty"))  
        // Effectively limits the max count. Generally discouraged, but may make sense in settings such as networking.  
        .maxId(256)  
        // Build the registry.  
        .create();
```

--------------------------------

### Implement Quick Move Stack Logic in Java

Source: https://docs.neoforged.net/docs/1.20.4/gui/menus

This Java code snippet demonstrates the implementation of the `#quickMoveStack` method, which handles the logic for shifting or quickly moving item stacks within a menu. It interacts with `#moveItemStackTo` to transfer items between different inventory sections (data inventory, player inventory, hotbar).

```java
//@Override
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
    -/

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
    -/
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

### Handle Server-Side Configuration Task and Send Acknowledgement (Java)

Source: https://docs.neoforged.net/docs/1.20.4/networking/configuration-tasks

Handles a server-side configuration task payload (`MyData`) and sends an acknowledgement (`AckPayload`) upon successful processing. It uses `submitAsync` for potentially long-running operations, handles exceptions by disconnecting the client, and sends the acknowledgement using `replyHandler().send()`.

```java
public void onMyData(MyData data, ConfigurationPayloadContext context) {  
    context.submitAsync(() -> {  
        blah(data.name());  
    })  
    .exceptionally(e -> {  
        // Handle exception  
        context.packetHandler().disconnect(Component.translatable("my_mod.configuration.failed", e.getMessage()));  
        return null;  
    })  
    .thenAccept(v -> {  
        context.replyHandler().send(new AckPayload());  
    });       
}
```

--------------------------------

### Data Synchronization with Slots and DataSlots (Java)

Source: https://docs.neoforged.net/docs/1.20.4/gui/menus

Illustrates data synchronization between server and client using `SlotItemHandler` for item stacks and `DataSlot` for integers. It shows how to add these to a menu during construction, manage inventory slots, and handle integer data synchronization, noting the limitation of `DataSlot` to short values.

```Java
// Assume we have an inventory from a data object of size 5  
// Assume we have a DataSlot constructed on each initialization of the server menu  
  
// Client menu constructor  
public MyMenuAccess(int containerId, Inventory playerInventory) {  
  this(containerId, playerInventory, new ItemStackHandler(5), DataSlot.standalone());  
}  
  
// Server menu constructor  
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

### Register Datapack Provider with GatherDataEvent

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Demonstrates how to register a `DatapackBuiltinEntriesProvider` to the data generation system using the `GatherDataEvent`. This ensures that datapack registry data is generated when the server data is being prepared.

```java
@SubscribeEvent // on the mod event bus
public static void onGatherData(GatherDataEvent event) {
    event.getGenerator().addProvider(
        // Only run datapack generation when server data is being generated
        event.includeServer(),
        // Create the provider
        output -> new DatapackBuiltinEntriesProvider(
            output,
            event.getLookupProvider(),
            // Our registry set builder to generate the data from.
            new RegistrySetBuilder().add(...),
            // A set of mod ids we are generating. Usually only your own mod id.
            Set.of("yourmodid")
        )
    );
}
```

--------------------------------

### Registering Items with DeferredRegister.Items in Java

Source: https://docs.neoforged.net/docs/1.20.4/items

Demonstrates how to use DeferredRegister.Items to register standard items, including variations with and without properties, and factory methods. It covers registering items with custom constructors and simplified registration.

```java
public static final DeferredRegister.Items ITEMS = DeferredRegister.createItems(ExampleMod.MOD_ID);  

public static final Supplier<Item> EXAMPLE_ITEM = ITEMS.registerItem(
        "example_item",
        Item::new, // The factory that the properties will be passed into. 
        new Item.Properties() // The properties to use.
);

public static final Supplier<Item> EXAMPLE_ITEM = ITEMS.registerSimpleItem(
        "example_item",
        new Item.Properties() // The properties to use.
);

public static final Supplier<Item> EXAMPLE_ITEM = ITEMS.registerItem("example_item", Item::new);
public static final Supplier<Item> EXAMPLE_ITEM = ITEMS.registerSimpleItem("example_item");
```

--------------------------------

### Implement and Register IIngredientSerializer

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/ingredients

Demonstrates the implementation of the INSTANCE for an ingredient serializer, its registration using CraftingHelper during the RegisterEvent, and how an Ingredient subclass returns the serializer instance.

```java
// In some serializer class  
public static final ExampleIngredientSerializer INSTANCE = new ExampleIngredientSerializer();  
  
// In some event handler class  
@SubscribeEvent // on the mod event bus  
public static void registerSerializers(RegisterEvent event) {
  event.register(ForgeRegistries.Keys.RECIPE_SERIALIZERS,  
    helper -> CraftingHelper.register(registryName, INSTANCE)  
  );
}

// In some ingredient subclass  
@Override  
public IIngredientSerializer<? extends Ingredient> getSerializer() {
  return INSTANCE;
}
```

--------------------------------

### Implement and Register a RecipeProvider for Data Generation

Source: https://docs.neoforged.net/docs/1.20.4/resources

This snippet demonstrates how to create a custom RecipeProvider by extending the base class and overriding the buildRecipes method. It also shows how to register this provider within the GatherDataEvent handler, specifying conditions for generation.

```java
public class MyRecipeProvider extends RecipeProvider {  
    public MyRecipeProvider(PackOutput output) {  
        super(output);  
    }
      
    @Override  
    protected void buildRecipes(RecipeOutput output) {  
        // Register your recipes here.  
    }
}
  
// In some event handler class  
@SubscribeEvent // on the mod event bus  
public static void gatherData(GatherDataEvent event) {  
    DataGenerator generator = event.getGenerator();  
    PackOutput output = generator.getPackOutput();  
    ExistingFileHelper existingFileHelper = event.getExistingFileHelper();  
    CompletableFuture<HolderLookup.Provider> lookupProvider = event.getLookupProvider();  
      
    generator.addProvider(
            event.includeServer(),
            new MyRecipeProvider(output)
    );
}

```

--------------------------------

### Constructing Conditions with IConditionBuilder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/recipes

Demonstrates how to simplify the creation of condition instances for conditional recipes by implementing the IConditionBuilder interface. This interface provides methods for constructing complex logical conditions.

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

### Registering Configuration Task with Listener Capture (Java)

Source: https://docs.neoforged.net/docs/1.20.4/networking/configuration-tasks

This code snippet shows how to register a configuration task that captures the server's listener. This is done within the OnGameConfigurationEvent, where the listener is obtained using 'event.listener()' and passed to the configuration task constructor.

```java
@SubscribeEvent // on the mod event bus  
public static void register(final OnGameConfigurationEvent event) {  
    event.register(new MyConfigurationTask(event.listener()));  
}  

```

--------------------------------

### Create Custom Creative Tabs (Java)

Source: https://docs.neoforged.net/docs/1.20.4/items

This code demonstrates the creation of a custom creative tab in NeoForge. It uses a `DeferredRegister<CreativeModeTab>` and the `CreativeModeTab.builder()` to define the tab's properties, including its title, icon, and the items it will display.

```java
//CREATIVE_MODE_TABS is a DeferredRegister<CreativeModeTab>  
public static final Supplier<CreativeModeTab> EXAMPLE_TAB = CREATIVE_MODE_TABS.register("example", () -> CreativeModeTab.builder()  
    //Set the title of the tab. Don't forget to add a translation!  
    .title(Component.translatable("itemGroup." + MOD_ID + ".example"))  
    //Set the icon of the tab.  
    .icon(() -> new ItemStack(MyItemsClass.EXAMPLE_ITEM.get()))  
    //Add your items to the tab.  
    .displayItems((params, output) -> {  
        output.accept(MyItemsClass.MY_ITEM);  
        // Accepts an ItemLike. This assumes that MY_BLOCK has a corresponding item.  
        output.accept(MyBlocksClass.MY_BLOCK);  
    })  
    .build()  
);

```

--------------------------------

### Add Brewing Recipes with NeoForge

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/incode

Demonstrates how to add custom brewing recipes using the BrewingRecipeRegistry in NeoForge. Recipes must be added within FMLCommonSetupEvent and enqueued to the work queue for thread safety. It supports custom IBrewingRecipe implementations for complex transformations.

```java
public class ExampleModSetup {
    @Mod.EventBusSubscriber(modid = "examplemod", bus = Mod.EventBusSubscriber.Bus.MOD)
    public static class RegistryEvents {
        @SubscribeEvent
        public static void onCommonSetup(FMLCommonSetupEvent event) {
            event.enqueueWork(() -> {
                // Example: Add a simple brewing recipe
                BrewingRecipeRegistry.addRecipe(new ResourceLocation("examplemod", "my_potion_recipe"),
                    new ItemStack(Items.GOLDEN_CARROT),
                    new ItemStack(Items.NETHER_WART),
                    new ItemStack(Items.POTION, 1, PotionUtils.addPotion(new ItemStack(Items.POTION), Potions.NIGHT_VISION)));

                // Example: Add a custom IBrewingRecipe
                BrewingRecipeRegistry.addRecipe(new MyCustomBrewingRecipe());
            });
        }
    }

    // Custom IBrewingRecipe implementation example
    public static class MyCustomBrewingRecipe implements IBrewingRecipe {
        @Override
        public boolean isInput(ItemStack input) {
            return input.getItem() == Items.POTION;
        }

        @Override
        public boolean isIngredient(ItemStack ingredient) {
            return ingredient.getItem() == Items.DIAMOND;
        }

        @Override
        public ItemStack getOutput(ItemStack input, ItemStack ingredient) {
            // Example: Transform a Potion of Swiftness into a Potion of Haste with a Diamond
            if (input.getItem() == Items.POTION && ingredient.getItem() == Items.DIAMOND) {
                PotionType originalPotion = PotionUtils.getPotion(input);
                if (originalPotion == Potions.SWIFTNESS) {
                    return PotionUtils.setPotion(new ItemStack(Items.POTION), Potions.REGENERATION);
                }
            }
            return ItemStack.EMPTY;
        }
    }
}
```

--------------------------------

### Registering a MenuType with MenuSupplier in Java

Source: https://docs.neoforged.net/docs/1.20.4/gui/menus

Demonstrates how to register a new MenuType using a MenuSupplier. The MenuSupplier provides the logic to create a new AbstractContainerMenu instance. This is typically done using a DeferredRegister for MenuTypes.

```java
// For some DeferredRegister<MenuType<?>> REGISTER  
public static final Supplier<MenuType<MyMenu>> MY_MENU = REGISTER.register("my_menu", () -> new MenuType(MyMenu::new, FeatureFlags.DEFAULT_FLAGS));  

// In MyMenu, an AbstractContainerMenu subclass  
public MyMenu(int containerId, Inventory playerInv) {  
  super(MY_MENU.get(), containerId);  
  // ...  
}

```

--------------------------------

### Create Item, Potion, and VillagerType Tags in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/tags

Demonstrates how to create tag wrappers for different registry types like Items, Potions, and VillagerTypes using NeoForge's tagging system. It shows the usage of `ItemTags.create`, `ForgeRegistries.POTIONS.tags().createTagKey`, and `TagKey.create` for different registry contexts.

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

### Acknowledging Configuration Task by Capturing Listener (Java)

Source: https://docs.neoforged.net/docs/1.20.4/networking/configuration-tasks

This implementation of ICustomConfigurationTask captures the ServerConfigurationListener to acknowledge task completion directly on the server side. The 'run' method sends a payload and then calls 'listener.finishCurrentTask()' to signal completion.

```java
public record MyConfigurationTask(ServerConfigurationListener listener) implements ICustomConfigurationTask {  
    public static final ConfigurationTask.Type TYPE = new ConfigurationTask.Type(new ResourceLocation("mymod:my_task"));  
      
    @Override  
    public void run(final Consumer<CustomPacketPayload> sender) {  
        final MyData payload = new MyData();  
        sender.accept(payload);  
        listener.finishCurrentTask(type());  
    }  
  
    @Override  
    public ConfigurationTask.Type type() {  
        return TYPE;  
    }  
}  

```

--------------------------------

### Registering Contents to a Custom Registry (Java)

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Shows two methods for registering new contents to a custom registry: using DeferredRegister and using the RegisterEvent.

```java
public static final DeferredRegister<Spell> SPELLS = DeferredRegister.create("yourmodid", SPELL_REGISTRY);  
public static final Supplier<Spell> EXAMPLE_SPELL = SPELLS.register("example_spell", () -> new Spell(...));  
  
// Alternatively:
@SubscribeEvent // on the mod event bus  
public static void register(RegisterEvent event) {  
    event.register(SPELL_REGISTRY, registry -> {  
        registry.register(new ResourceLocation("yourmodid", "example_spell"), () -> new Spell(...));  
    });
}
```

--------------------------------

### Gradle Buildscript Configuration for Game Test Server

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Illustrates how to configure the Game Test Server in Gradle to prevent build failures caused by forced system exits. By setting '#setForceExit false' within the 'gameTestServer' block, the Gradle daemon is not killed when the server task completes.

```gradle
// Game Test Server run configuration  
gameTestServer {  
    // ...  
    setForceExit false  
}
```

--------------------------------

### Implement Custom Recipe Interface in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/custom

Demonstrates how to implement the `Recipe` interface for custom recipes. This includes defining the recipe's data structure and implementing methods for matching inputs and assembling the output. The `Container` passed to the recipe should be treated as immutable.

```java
public record ExampleRecipe(Ingredient input, int data, ItemStack output) implements Recipe<Container> {
  // Implement methods here
}
```

--------------------------------

### Implement Blockstate Definition and Default State

Source: https://docs.neoforged.net/docs/1.20.4/blocks/states

Demonstrates overriding `createBlockStateDefinition` to add properties and `registerDefaultState` to set the initial state of a block. This is a core part of defining a block's behavior and appearance.

```java
public class MyBlock extends Block {
    public static final DirectionProperty FACING = BlockStateProperties.FACING;
    public static final BooleanProperty LIT = BlockStateProperties.LIT;

    public MyBlock(BlockBehaviour.Properties pProperties) {
        super(pProperties);
        registerDefaultState(stateDefinition.any()
                .setValue(FACING, Direction.NORTH)
                .setValue(LIT, false)
        );
    }

    @Override
    protected void createBlockStateDefinition(StateDefinition.Builder<Block, BlockState> pBuilder) {
        pBuilder.add(FACING, LIT);
    }

    @Override
    @Nullable
    public BlockState getStateForPlacement(BlockPlaceContext pContext) {
        // Logic to determine state based on placement context
        return super.getStateForPlacement(pContext);
    }
}

```

--------------------------------

### Initialize AbstractContainerScreen in Java

Source: https://docs.neoforged.net/docs/1.20.4/gui/screens

This snippet demonstrates the constructor for a subclass of AbstractContainerScreen. It initializes the screen with menu, player inventory, and title, and sets initial positions for labels. Note the caution regarding imageHeight affecting inventoryLabelY.

```java
public MyContainerScreen(MyMenu menu, Inventory playerInventory, Component title) {
    super(menu, playerInventory, title);
  
    this.titleLabelX = 10;
    this.inventoryLabelX = 10;
  
    /*  
     - If the 'imageHeight' is changed, 'inventoryLabelY' must also be  
     - changed as the value depends on the 'imageHeight' value.  
     -/
}

```

--------------------------------

### Generate Data Maps with DataMapProvider in Java

Source: https://docs.neoforged.net/docs/1.20.4/datamaps

This Java code demonstrates how to generate data maps using `DataMapProvider`. By extending this class and overriding the `gather` method, developers can define data map entries. The `builder` method is used to specify the data map, and `add` overloads allow attaching values to specific items or tags, with options to control merging behavior and provide loading conditions for optional dependencies.

```java
public class DropHealingGen extends DataMapProvider {
  
    public DropHealingGen(PackOutput packOutput, CompletableFuture<HolderLookup.Provider> lookupProvider) {
        super(packOutput, lookupProvider);
    }

    @Override
    protected void gather() {
        // In the examples below, we do not need to forcibly replace any value as that's the default behaviour since a merger isn't provided, so the third parameter can be false.

        // If you were to provide a merger for your data map, then the third parameter will cause the old value to be overwritten if set to true, without invoking the merger  

        builder(DROP_HEALING)
                // Always give entities that drop any item in the minecraft:fox_food tag 12 hearts  
                .add(ItemTags.FOX_FOOD, new DropHealing(12, 1f), false)
                // Have a 10% chance of healing entities that drop an acacia boat by one point  
                .add(Items.ACACIA_BOAT.builtInRegistryHolder(), new DropHealing(1, 0.1f), false);
    }
}
```

--------------------------------

### Registering Block Items with DeferredRegister.Items in Java

Source: https://docs.neoforged.net/docs/1.20.4/items

Shows how to register block items using DeferredRegister.Items, including shortcuts for block items. It covers variations with and without properties, and omitting the name parameter to use the block's registry name.

```java
public static final Supplier<BlockItem> EXAMPLE_BLOCK_ITEM = ITEMS.registerSimpleBlockItem("example_block", ExampleBlocksClass.EXAMPLE_BLOCK, new Item.Properties());
public static final Supplier<BlockItem> EXAMPLE_BLOCK_ITEM = ITEMS.registerSimpleBlockItem("example_block", ExampleBlocksClass.EXAMPLE_BLOCK);
public static final Supplier<BlockItem> EXAMPLE_BLOCK_ITEM = ITEMS.registerSimpleBlockItem(ExampleBlocksClass.EXAMPLE_BLOCK, new Item.Properties());
public static final Supplier<BlockItem> EXAMPLE_BLOCK_ITEM = ITEMS.registerSimpleBlockItem(ExampleBlocksClass.EXAMPLE_BLOCK);
```

--------------------------------

### Register Key Mappings with NeoForged Event Bus

Source: https://docs.neoforged.net/docs/1.20.4/misc/keymappings

Demonstrates how to register a KeyMapping using the RegisterKeyMappingsEvent on the mod event bus. This code should only run on the physical client.

```java
import net.neoforged.neoforge.client.event.RegisterKeyMappingsEvent;
import net.neoforged.neoforge.common.util.Lazy;
import net.neoforged.bus.api.SubscribeEvent;
import net.minecraft.client.KeyMapping;

// In some physical client only class  
public class ExampleKeyMappings {

  // Key mapping is lazily initialized so it doesn't exist until it is registered  
  public static final Lazy<KeyMapping> EXAMPLE_MAPPING = Lazy.of(() -> new KeyMapping(
    "key.examplemod.example1", // Will be localized using this translation key
    InputConstants.Type.KEYSYM, // Default mapping is on the keyboard
    GLFW.GLFW_KEY_P, // Default key is P
    "key.categories.misc" // Mapping will be in the misc category
  ));  
  
  @SubscribeEvent // on the mod event bus only on the physical client  
  public static void registerBindings(RegisterKeyMappingsEvent event) {
    event.register(EXAMPLE_MAPPING.get());
  }

}
```

--------------------------------

### Create Record Codec with RecordCodecBuilder

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Demonstrates how to create a codec for a custom object (SomeObject) using `RecordCodecBuilder#create`. This method allows defining named fields and specifying how to construct the object from these fields. It handles required and optional fields with default values.

```java
public class SomeObject {
  
  public SomeObject(String s, int i, boolean b) { /* ... */ }
  
  public String s() { /* ... */ }
  
  public int i() { /* ... */ }
  
  public boolean b() { /* ... */ }
}

public static final Codec<SomeObject> RECORD_CODEC = RecordCodecBuilder.create(instance -> 
  instance.group(
    Codec.STRING.fieldOf("s").forGetter(SomeObject::s),
    Codec.INT.optionalFieldOf("i", 0).forGetter(SomeObject::i),
    Codec.BOOL.fieldOf("b").forGetter(SomeObject::b)
  ).apply(instance, SomeObject::new)
);

```

--------------------------------

### Registering a Block with DeferredRegister.Blocks

Source: https://docs.neoforged.net/docs/1.20.4/blocks

Demonstrates how to register a new block using NeoForged's `DeferredRegister.Blocks`. This method ensures blocks are instantiated only once during the registration phase, preventing game crashes and inconsistencies.

```java
public static final DeferredBlock<Block> MY_BLOCK = BLOCKS.register("my_block", () -> new Block(...));
```

--------------------------------

### Configure Data Generator Arguments in build.gradle

Source: https://docs.neoforged.net/docs/1.20.4/resources

This snippet shows how to add command line arguments to the data generation run configuration in a `build.gradle` file. It demonstrates adding both key-value pairs and boolean arguments.

```gradle
runs {
    // other run configurations here
      
    data {
        programArguments.addAll '--arg1', 'value1', '--arg2', 'value2', '--all' // boolean args have no value
    }
}
```

```gradle
runs {
    // other run configurations here
      
    data {
        programArguments.addAll '--mod', 'examplemod', // insert your own mod id
                '--output', file('src/generated/resources').getAbsolutePath(),
                '--includeClient',
                '--includeServer'
    }
}
```

--------------------------------

### AbstractContainerMenu Constructors in Java

Source: https://docs.neoforged.net/docs/1.20.4/gui/menus

Illustrates the required constructors for an AbstractContainerMenu subclass. It includes a client-side constructor that mirrors the server-side constructor, ensuring proper initialization on both ends.

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

### Create a Basic Game Test Method

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Demonstrates the fundamental structure of a Game Test method in Java. It requires a `@GameTest` annotation and accepts a `GameTestHelper` object to perform test logic. The method returns nothing, and its success is determined by the logic within.

```java
public class ExampleGameTests {
  @GameTest
  public static void exampleTest(GameTestHelper helper) {
    // Do stuff
  }
}
```

--------------------------------

### Custom Recipe Logic for Non-Item Recipes (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/custom

Demonstrates how to implement custom methods like 'matches' and 'assemble' within a recipe subclass for recipes that do not use items as input or output. It also shows how to retrieve and check these recipes using RecipeManager.

```java
// In some Recipe subimplementation ExampleRecipe  
  
// Checks the block at the position to see if it matches the stored data  
boolean matches(Level level, BlockPos pos);

// Creates the block state to set the block at the specified position to  
BlockState assemble(RegistryAccess access);

// In some manager class  
public Optional<ExampleRecipe> getRecipeFor(Level level, BlockPos pos) {
  return level.getRecipeManager()
    .getAllRecipesFor(exampleRecipeType) // Gets all recipes
    .stream() // Looks through all recipes for types
    .filter(recipe -> recipe.matches(level, pos)) // Checks if the recipe inputs are valid
    .findFirst(); // Finds the first recipe whose inputs match
}

```

--------------------------------

### Configure Access Transformers in build.gradle

Source: https://docs.neoforged.net/docs/1.20.4/advanced/accesstransformers

This snippet shows how to declare Access Transformer configuration within the build.gradle file. It specifies the path to the AT configuration file and is where the Minecraft mappings version is also defined. Multiple AT files can be specified and will be applied in order.

```gradle
minecraft {
  accessTransformers {
    file('src/main/resources/META-INF/accesstransformer.cfg')
  }
}

```

```gradle
minecraft {
  accessTransformers {
    file('src/main/resources/accesstransformer_main.cfg')
    file('src/additions/resources/accesstransformer_additions.cfg')
  }
}

```

```gradle
minecraft {
  accessTransformer = file('src/main/resources/META-INF/accesstransformer.cfg')
}

```

--------------------------------

### Registering a Custom Datapack Registry in Java

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

This snippet demonstrates how to register a custom datapack registry using NeoForge's DataPackRegistryEvent.NewRegistry event. It requires a ResourceKey for the registry and a Codec for serialization. The network codec can be the same as the normal codec or a reduced variant.

```java
public static final ResourceKey<Registry<Spell>> SPELL_REGISTRY_KEY = ResourceKey.createRegistryKey(new ResourceLocation("yourmodid", "spells"));  
  
@SubscribeEvent // on the mod event bus  
public static void registerDatapackRegistries(DataPackRegistryEvent.NewRegistry event) {  
    event.dataPackRegistry(  
            // The registry key.  
            SPELL_REGISTRY_KEY,  
            // The codec of the registry contents.  
            Spell.CODEC,  
            // The network codec of the registry contents. Often identical to the normal codec.  
            // May be a reduced variant of the normal codec that omits data that is not needed on the client.  
            // May be null. If null, registry entries will not be synced to the client at all.  
            // May be omitted, which is functionally identical to passing null (a method overload  
            // with two parameters is called that passes null to the normal three parameter method).  
            Spell.CODEC  
    );
}
```

--------------------------------

### BlockModelProvider Helpers for Cube Models

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

The BlockModelProvider, accessible via BlockStateProvider#models(), provides helpers for creating various cube-based models. These include methods for single textures, side/bottom/top textures, full cubes, and specialized variants like cube columns and orientable models.

```java
withExistingParent: Already mentioned before, this method returns a new model builder with the given parent. The parent must either already exist or be created before the model.
getExistingFile: Performs a lookup in the model provider's ExistingFileHelper, returning the corresponding ModelFile if present and throwing an IllegalStateException otherwise.
singleTexture: Accepts a parent and a single texture location, returning a model with the given parent, and with the texture variable "texture" set to the given texture location.
sideBottomTop: Accepts a parent and three texture locations, returning a model with the given parent and the side, bottom and top textures set to the three texture locations.
cube: Accepts six texture resource locations for the six sides, returning a full cube model with the six sides set to the six textures.
cubeAll: Accepts a texture location, returning a full cube model with the given texture applied to all six sides. A mix between singleTexture and cube, if you will.
cubeTop: Accepts two texture locations, returning a full cube model with the first texture applied to the sides and the bottom, and the second texture applied to the top.
cubeBottomTop: Accepts three texture locations, returning a full cube model with the side, bottom and top textures set to the three texture locations. A mix between cube and sideBottomTop, if you will.
cubeColumn and cubeColumnHorizontal: Accepts two texture locations, returning a "standing" or "laying" pillar cube model with the side and end textures set to the two texture locations. Used by BlockStateProvider#logBlock, BlockStateProvider#axisBlock and their variants.
orientable: Accepts three texture locations, returning a cube with a "front" texture. The three texture locations are the side, front and top texture, respectively.
orientableVertical: Variant of orientable that omits the top parameter, instead using the side parameter as well.
orientableWithBottom: Variant of orientable that has a fourth parameter for a bottom texture between the front and top parameter.
```

--------------------------------

### Creating a Parented Model with ModelProvider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Shows how to create a new model that inherits properties from an existing parent model using `withExistingParent(String name, ResourceLocation parent)`. This is a common pattern for variations of existing models.

```java
models.withExistingParent("stairs_inner", mcLoc("block/stairs_inner"));
itemModels.withExistingParent("my_custom_item", modLoc("item/base_item"));
```

--------------------------------

### Registering Objects with BootstrapContext in Datapack Registries

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Shows how to use the `BootstrapContext` to register objects, such as `ConfiguredFeature`, within a datapack registry. It involves defining a `ResourceKey` and then calling the `register` method on the `BootstrapContext`.

```java
// The resource key of our object.
public static final ResourceKey<ConfiguredFeature<?, ?>> EXAMPLE_CONFIGURED_FEATURE = ResourceKey.create(
    Registries.CONFIGURED_FEATURE,
    new ResourceLocation(MOD_ID, "example_configured_feature")
);

new RegistrySetBuilder()
    .add(Registries.CONFIGURED_FEATURE, bootstrap -> {
        bootstrap.register(
            // The resource key of our configured feature.
            EXAMPLE_CONFIGURED_FEATURE,
            // The actual configured feature.
            new ConfiguredFeature<>(Feature.ORE, new OreConfiguration(...))
        );
    })
    .add(Registries.PLACED_FEATURE, bootstrap -> {
    // ...
    });
```

--------------------------------

### Register Capability for All Items (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Iterates through all registered items to register a capability provider for a specific item type, like BucketItem. This is useful for applying a capability to all instances of a certain class.

```java
// For reference, you can find this code in the `CapabilityHooks` class.  
for (Item item : BuiltInRegistries.ITEM) {  
    if (item.getClass() == BucketItem.class) {  
        event.registerItem(Capabilities.FluidHandler.ITEM, (stack, ctx) -> new FluidBucketWrapper(stack), item);  
    }
}

```

--------------------------------

### Screen Closing and Cleanup Methods in Java

Source: https://docs.neoforged.net/docs/1.20.4/gui/screens

Demonstrates the implementation of onClose and removed methods for screen teardown. onClose is used for handling user-initiated closing actions like saving data, while removed handles final cleanup before the screen is garbage collected.

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

### Simplified Block Registration with `registerBlock`

Source: https://docs.neoforged.net/docs/1.20.4/blocks

This Java code shows a more concise way to register a block using the `registerBlock` helper method from `DeferredRegister.Blocks`. It takes the block's ID, a factory (often the constructor), and its properties as arguments.

```java
public static final DeferredRegister.Blocks BLOCKS = DeferredRegister.createBlocks("yourmodid");

public static final DeferredBlock<Block> EXAMPLE_BLOCK = BLOCKS.registerBlock(
        "example_block",
        Block::new, // The factory that the properties will be passed into.
        BlockBehaviour.Properties.of() // The properties to use.
);
```

--------------------------------

### Implement Recipe Serializer in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/custom

Illustrates the implementation of a `RecipeSerializer` for custom recipes. This involves decoding JSON, encoding for network transfer, and decoding from the network. The `Recipe#getSerializer` method should return the registered `RecipeSerializer` instance.

```java
// For some Supplier<RecipeSerializer<?>> EXAMPLE_SERIALIZER
// In ExampleRecipe
@Override
public RecipeSerializer<?> getSerializer() {
  return EXAMPLE_SERIALIZER.get();
}
```

--------------------------------

### Build Custom Block Models with ConfiguredModel.Builder

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Demonstrates how to directly build model objects using ConfiguredModel.Builder. This builder allows for setting model files, rotations, UV locks, and weights. The resulting configured models can be used in blockstate files.

```java
ConfiguredModel.Builder<?> builder = ConfiguredModel.builder()
        .modelFile(models().withExistingParent("example_model", this.mcLoc("block/cobblestone")))
        .rotationX(90)
        .rotationY(180)
        .uvlock(true)
        .weight(5);
ConfiguredModel[] model = builder.build();
```

--------------------------------

### Registering a Configuration Task in OnGameConfigurationEvent (Java)

Source: https://docs.neoforged.net/docs/1.20.4/networking/configuration-tasks

This code snippet demonstrates how to register a custom configuration task within the OnGameConfigurationEvent. The event is subscribed to on the mod event bus, and the 'register' method is used to add a new configuration task instance.

```java
@SubscribeEvent // on the mod event bus  
public static void register(final OnGameConfigurationEvent event) {  
    event.register(new MyConfigurationTask());  
}  

```

--------------------------------

### GUI Light Setting in ModelBuilder (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Explains how to configure the GUI light setting for a model using the `#guiLight` method in ModelBuilder. This determines the lighting applied when the model is viewed in the GUI.

```java
modelBuilder.guiLight(GuiLight.FRONT);
modelBuilder.guiLight(GuiLight.SIDE);
```

--------------------------------

### Register Capability with High Priority (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Registers a capability provider with a high event priority to ensure it is registered before NeoForge's default providers. This is useful for overriding or extending existing capabilities.

```java
modBus.addListener(RegisterCapabilitiesEvent.class, event -> {  
    event.registerItem(  
        Capabilities.FluidHandler.ITEM,  
        (stack, ctx) -> new MyCustomFluidBucketWrapper(stack),  
        // blocks to register for  
        MY_CUSTOM_BUCKET);  
}, EventPriority.HIGH); // use HIGH priority to register before NeoForge!

```

--------------------------------

### Create Custom Potion in Java

Source: https://docs.neoforged.net/docs/1.20.4/items/mobeffects

Demonstrates the creation of a custom Potion by registering it with a DeferredRegister. A Potion can contain multiple MobEffectInstances, and can be named or unnamed.

```java
//POTIONS is a DeferredRegister<Potion>
public static final Supplier<Potion> MY_POTION = POTIONS.register("my_potion", () -> new Potion(new MobEffectInstance(MY_MOB_EFFECT.get(), 3600)));

```

--------------------------------

### Render Background Texture (Java)

Source: https://docs.neoforged.net/docs/1.20.4/gui/screens

Renders the background texture for the container screen. It sets the texture, then uses graphics.blit to draw the texture at the specified position with given dimensions.

```java
import net.minecraft.client.gui.GuiGraphics;
import net.minecraft.resources.ResourceLocation;
import net.minecraft.client.renderer.RenderSystem;

// In some AbstractContainerScreen subclass  

// The location of the background texture (assets/<namespace>/<path>)  
private static final ResourceLocation BACKGROUND_LOCATION = new ResourceLocation(MOD_ID, "textures/gui/container/my_container_screen.png");  
  
@Override  
protected void renderBg(GuiGraphics graphics, float partialTick, int mouseX, int mouseY) {  
    /*  
     - Sets the texture location for the shader to use. While up to  
     - 12 textures can be set, the shader used within 'blit' only  
     - looks at the first texture index.  
     -/
    RenderSystem.setShaderTexture(0, BACKGROUND_LOCATION);

    /*  
     - Renders the background texture to the screen. 'leftPos' and  
     - 'topPos' should already represent the top left corner of where  
     - the texture should be rendered as it was precomputed from the  
     - 'imageWidth' and 'imageHeight'. The two zeros represent the  
     - integer u/v coordinates inside the 256 x 256 PNG file.  
     -/
    graphics.blit(BACKGROUND_LOCATION, this.leftPos, this.topPos, 0, 0, this.imageWidth, this.imageHeight);
}

```

--------------------------------

### Registering a MenuType with IContainerFactory in Java

Source: https://docs.neoforged.net/docs/1.20.4/gui/menus

Shows how to register a MenuType that requires additional data to be sent from the server to the client. It uses IForgeMenuType.create with an IContainerFactory, allowing for a FriendlyByteBuf to pass extra information.

```java
// For some DeferredRegister<MenuType<?>> REGISTER  
public static final Supplier<MenuType<MyMenuExtra>> MY_MENU_EXTRA = REGISTER.register("my_menu_extra", () -> IForgeMenuType.create(MyMenu::new));  

// In MyMenuExtra, an AbstractContainerMenu subclass  
public MyMenuExtra(int containerId, Inventory playerInv, FriendlyByteBuf extraData) {  
  super(MY_MENU_EXTRA.get(), containerId);  
  // Store extra data from buffer  
  // ...  
}

```

--------------------------------

### Implement CustomPacketPayload for Data Serialization

Source: https://docs.neoforged.net/docs/1.20.4/networking/payload

Implements the CustomPacketPayload interface for the MyData record. This includes defining a unique ResourceLocation ID, a constructor to read data from a FriendlyByteBuf, and a write method to serialize data into the buffer.

```java
public record MyData(String name, int age) implements CustomPacketPayload {  
      
    public static final ResourceLocation ID = new ResourceLocation("mymod", "my_data");  
      
    public MyData(final FriendlyByteBuf buffer) {  
        this(buffer.readUtf(), buffer.readInt());  
    }  
      
    @Override  
    public void write(final FriendlyByteBuf buffer) {  
        buffer.writeUtf(name());  
        buffer.writeInt(age());  
    }  
      
    @Override  
    public ResourceLocation id() {  
        return ID;  
    }  
}  

```

--------------------------------

### Define a List Config Value with NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/misc/config

Shows how to define a configuration value that is a list of elements using `ModConfigSpec.Builder#defineList`. This method allows specifying a validator to ensure each element in the list is valid. `defineListAllowEmpty` can be used if the list can be empty.

```java
// For some ModConfigSpec.Builder builder
// Assuming T is the type of elements in the list
// Validator<T> elementValidator = ...;
ConfigValue<List<T>> listValue = builder.comment("A list of values")
  .defineList("list_value_name", defaultValue, elementValidator, type);
```

--------------------------------

### Ultra-Simplified Block Registration with `registerSimpleBlock`

Source: https://docs.neoforged.net/docs/1.20.4/blocks

This Java code snippet illustrates the shortest way to register a basic block using `registerSimpleBlock`. This method is suitable when you only need to provide the block's ID and its properties, defaulting to the `Block` constructor.

```java
public static final DeferredBlock<Block> EXAMPLE_BLOCK = BLOCKS.registerSimpleBlock(
        "example_block",
        BlockBehaviour.Properties.of() // The properties to use.
);
```

--------------------------------

### BlockStateProvider Helpers for Common Block Models

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

The BlockStateProvider class offers helper methods for common block models such as stairs, slabs, doors, and more. These methods simplify the process of defining block states and their corresponding models, reducing boilerplate code.

```java
// Like #horizontalBlock, has an overload that accepts a Function<BlockState, ModelFile> instead.  
horizontalFaceBlock(block, exampleModel);  
// Similar to #horizontalBlock, but for blocks that are rotatable in all directions, including up and down.  
// Again, has an overload that accepts a Function<BlockState, ModelFile> instead.  
directionalBlock(block, exampleModel);
```

--------------------------------

### Schedule Actions within a Game Test

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Demonstrates methods in `GameTestHelper` for scheduling actions to occur at specific times during a Game Test. `#runAtTickTime` executes an action on a designated tick, `#runAfterDelay` runs it after a set number of ticks, and `#onEachTick` executes it every tick.

```java
helper.runAtTickTime(tick, action);
helper.runAfterDelay(delay, action);
helper.onEachTick(action);
```

--------------------------------

### Registering a Basic Block with Custom Properties

Source: https://docs.neoforged.net/docs/1.20.4/blocks

This Java code snippet demonstrates how to register a custom block with specific properties. It uses `DeferredRegister.Blocks` to manage block registration and `BlockBehaviour.Properties` to define attributes like destroy time, explosion resistance, sound, and light level.

```java
public static final DeferredBlock<Block> MY_BETTER_BLOCK = BLOCKS.register(
        "my_better_block",
        () -> new Block(BlockBehaviour.Properties.of()
                .destroyTime(2.0f)
                .explosionResistance(10.0f)
                .sound(SoundType.GRAVEL)
                .lightLevel(state -> 7)
        ));
```

--------------------------------

### Gradle Buildscript Configuration to Enable Game Tests

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Demonstrates how to enable Game Tests in custom run configurations within the Gradle buildscript. This is achieved by setting the 'forge.enableGameTest' property to 'true'.

```gradle
// Inside a run configuration  
property 'forge.enableGameTest', 'true'
```

--------------------------------

### Gradle Buildscript Configuration for Game Test Namespaces

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Shows how to configure the Gradle buildscript to enable Game Tests from additional namespaces. This is done by setting the 'forge.enabledGameTestNamespaces' property in a run configuration, specifying namespaces separated by commas.

```gradle
// Inside a run configuration  
property 'forge.enabledGameTestNamespaces', 'modid1,modid2,modid3'
```

--------------------------------

### Java Mod Entrypoint with @Mod Annotation

Source: https://docs.neoforged.net/docs/1.20.4/gettingstarted/modfiles

Defines the entry point for a Java mod using the @Mod annotation. The annotation value must match a mod ID from mods.toml, and the constructor receives the mod-specific event bus for initialization.

```java
@Mod("examplemod") // Must match a mod id in the mods.toml
public class Example {
  public Example(IEventBus modBus) { // The parameter is the mod-specific event bus, needed e.g. for registration and events
    // Initialize logic here
  }
}
```

--------------------------------

### Build Multipart Blockstates with ConfiguredModel.Builder

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Details the process of constructing multipart blockstate files using MultiPartBlockStateBuilder and ConfiguredModel.Builder. This includes defining models for parts, adding conditions based on block properties, and managing nested condition groups.

```java
MultiPartBlockStateBuilder multipartBuilder = getMultipartBuilder(MyBlocksClass.EXAMPLE_BLOCK.get());
multipartBuilder.part()
        .modelFile(models().withExistingParent("example_multipart_model", mcLoc("block/cobblestone")))
        .addModel()
        .condition(BlockStateProperties.FACING, Direction.NORTH, Direction.SOUTH)
        .useOr()
        .nestedGroup()
        .condition(BlockStateProperties.FACING, Direction.NORTH)
        .useOr()
        .nestedGroup()
        .endNestedGroup()
        .endNestedGroup()
        .end();
```

--------------------------------

### Resource Location Helpers in ModelProvider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Explains the utility methods `mcLoc(String name)` and `modLoc(String name)` provided by ModelProvider for easily creating ResourceLocations for the 'minecraft' namespace and the provider's mod ID, respectively.

```java
ResourceLocation stoneTexture = mcLoc("block/stone");
ResourceLocation myBlockTexture = modLoc("block/my_block_texture");
```

--------------------------------

### Adding a SubProviderEntry for LootTableProvider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

This code shows how to create a LootTableProvider.SubProviderEntry, linking a custom LootTableSubProvider implementation to a specific LootContextParamSet. This entry is then included in the list passed to the LootTableProvider constructor.

```java
new LootTableProvider.SubProviderEntry(  
  ExampleSubProvider::new,  
  // Loot table generator for the 'empty' param set  
  LootContextParamSets.EMPTY  
)
```

--------------------------------

### Create BlockCapabilityCache with Invalidation Listener (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Shows how to create a BlockCapabilityCache with optional parameters for validity checking and an invalidation listener. This allows reacting to capability changes, removals, or appearances. The validity check ensures the cache remains relevant, and the listener is called when the capability is invalidated.

```Java
// With optional invalidation listener:
this.capCache = BlockCapabilityCache.create(
        Capabilities.ItemHandler.BLOCK, // capability to cache
        level, // level
        pos, // target position
        Direction.NORTH, // context
        () -> !this.isRemoved(), // validity check (because the cache might outlive the object it belongs to)
        () -> onCapInvalidate() // invalidation listener
);
```

--------------------------------

### Creating a DeferredRegister for Blocks

Source: https://docs.neoforged.net/docs/1.20.4/blocks

Illustrates the initialization of `DeferredRegister.Blocks`, a specialized registry for handling block registrations in NeoForged. It requires a unique mod ID for proper identification.

```java
public static final DeferredRegister.Blocks BLOCKS = DeferredRegister.createBlocks("yourmodid");
```

--------------------------------

### Handle Client Acknowledgement Payload on Server (Java)

Source: https://docs.neoforged.net/docs/1.20.4/networking/configuration-tasks

Handles the `AckPayload` received from the client on the server side. This method is called when the client acknowledges a configuration task. It signals that the task is completed using `context.taskCompletedHandler().onTaskCompleted()`.

```java
public void onAck(AckPayload payload, ConfigurationPayloadContext context) {  
    context.taskCompletedHandler().onTaskCompleted(MyConfigurationTask.TYPE);  
}
```

--------------------------------

### Generate Smithing Trim Recipe with SmithingTrimRecipeBuilder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/recipes

Illustrates the creation of smithing recipes for armor trims using SmithingTrimRecipeBuilder. Custom upgrade recipes can be generated with a serializer. Unlock criteria can be set before saving.

```java
// In RecipeProvider#buildRecipes(writer)  
SmithingTrimRecipe builder = SmithingTrimRecipe.smithingTrim(template, base, addition, RecipeCategory.MISC)  
  .unlocks("criteria", criteria) // How the recipe is unlocked  
  .save(writer, name); // Add data to builder  

```

--------------------------------

### Target Methods with Access Transformers

Source: https://docs.neoforged.net/docs/1.20.4/advanced/accesstransformers

This is the syntax for targeting methods using Access Transformers. It includes the access modifier, fully qualified class name, method name, parameter types, and return type. This precise signature is necessary for accurate targeting.

```plaintext
<access modifier> <fully qualified class name> <method name>(<parameter types>)<return type>

```

--------------------------------

### OBJ Model Configuration (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Enables the use of Wavefront .obj 3D models within the game. It requires the .obj file and a corresponding .mtl file, with options to override the .mtl path and specify textures. Advanced properties control culling, shading, and UV flipping.

```json
{
  "loader": "neoforge:obj",
  "model": "examplemod:models/example.obj",
  "mtl_override": "examplemod:models/example_other_name.mtl",
  "textures": {
    "texture0": "minecraft:block/cobblestone",
    "particle": "minecraft:block/stone"
  },
  "automatic_culling": false,
  "shade_quads": false,
  "flip_v": true,
  "emissive_ambient": false
}
```

--------------------------------

### Custom Baked Model Implementation (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Implements a custom baked model by defining its properties and how to generate quads for rendering. It includes methods for ambient occlusion, GUI 3D display, block light usage, particle icon retrieval, item overrides, custom renderer checks, and quad generation.

```java
public class MyDynamicModel implements BakedModel {
    private final boolean useAmbientOcclusion;
    private final boolean isGui3d;
    private final boolean usesBlockLight;
    private final TextureAtlasSprite particle;
    private final ItemOverrides overrides;

    // It may specify any additional parameters to store in fields you deem necessary for your model to work.
    public MyDynamicModel(boolean useAmbientOcclusion, boolean isGui3d, boolean usesBlockLight, TextureAtlasSprite particle, ItemOverrides overrides) {
        this.useAmbientOcclusion = useAmbientOcclusion;
        this.isGui3d = isGui3d;
        this.usesBlockLight = usesBlockLight;
        this.particle = particle;
        this.overrides = overrides;
    }

    // Use our attributes. Refer to the article on baked models for more information on the method's effects.
    @Override
    public boolean useAmbientOcclusion() {
        return useAmbientOcclusion;
    }

    @Override
    public boolean isGui3d() {
        return isGui3d;
    }

    @Override
    public boolean usesBlockLight() {
        return usesBlockLight;
    }

    @Override
    public TextureAtlasSprite getParticleIcon() {
        // Return MISSING_TEXTURE.sprite() if you don't need a particle, e.g. when in an item model context.
        return particle;
    }

    @Override
    public ItemOverrides getOverrides() {
        // Return ItemOverrides.EMPTY when in a block model context.
        return overrides;
    }

    // Override this to true if you want to use a custom block entity renderer instead of the default renderer.
    @Override
    public boolean isCustomRenderer() {
        return false;
    }

    // This is where the magic happens. Return a list of the quads to render here. Parameters are:
    // - The blockstate being rendered. May be null if rendering an item.
    // - The side being culled against. May be null, which means quads that cannot be occluded should be returned.
    // - A client-bound random source you can use for randomizing stuff.
    // - The extra data to use. Originates from a block entity (if present), or from BakedModel#getModelData().
    // - The render type for which quads are being requested.
    // NOTE: This may be called many times in quick succession, up to several times per block.
    // This should be as fast as possible and use caching wherever applicable.
    @Override
    public List<BakedQuad> getQuads(@Nullable BlockState state, @Nullable Direction side, RandomSource rand, ModelData extraData, @Nullable RenderType renderType) {
        List<BakedQuad> quads = new ArrayList<>();
        // add elements to the quads list as needed here
        return quads;
    }
}
```

--------------------------------

### Generating Shapeless Recipes with ShapelessRecipeBuilder (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/recipes

This snippet illustrates the usage of ShapelessRecipeBuilder for creating shapeless recipes. It includes adding required ingredients, specifying unlock criteria, and saving the recipe.

```java
// In RecipeProvider#buildRecipes(writer)  
ShapelessRecipeBuilder builder = ShapelessRecipeBuilder.shapeless(RecipeCategory.MISC, result)  
  .requires(item) // Add item to the recipe  
  .unlockedBy("criteria", criteria) // How the recipe is unlocked  
  .save(writer); // Add data to builder  

```

--------------------------------

### Set BlockState in Level with Update Flags (Java)

Source: https://docs.neoforged.net/docs/1.20.4/blocks/states

Shows how to set a BlockState at a specific position in the world using Level#setBlock. It explains the use of update flags to control how the block update is propagated to neighbors and the client.

```java
level.setBlock(blockPos, blockState, Block.UPDATE_NEIGHBORS | Block.UPDATE_CLIENTS);

```

--------------------------------

### Menu Access Validation with #stillValid and ContainerLevelAccess (Java)

Source: https://docs.neoforged.net/docs/1.20.4/gui/menus

Demonstrates how to implement the `#stillValid` method to check if a player is within a certain range of a block for menu access. It utilizes `ContainerLevelAccess` to provide context about the block's location and `Player` object. The client menu defaults to always returning true, while the server implementation checks proximity.

```Java
// Client menu constructor  
public MyMenuAccess(int containerId, Inventory playerInventory) {  
  this(containerId, playerInventory, ContainerLevelAccess.NULL);  
}  
  
// Server menu constructor  
public MyMenuAccess(int containerId, Inventory playerInventory, ContainerLevelAccess access) {  
  // ...  
}  
  
// Assume this menu is attached to Supplier<Block> MY_BLOCK  
@Override  
public boolean stillValid(Player player) {  
  return AbstractContainerMenu.stillValid(this.access, player, MY_BLOCK.get());  
}  
```

--------------------------------

### Check KeyMapping Press in GUI (keyPressed)

Source: https://docs.neoforged.net/docs/1.20.4/misc/keymappings

Illustrates how to check if a KeyMapping is active and matches a key press within a GUI element. It uses the isActiveAndMatches method with InputConstants.getKey to verify the input.

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

### Register AbstractContainerScreen (Java)

Source: https://docs.neoforged.net/docs/1.20.4/gui/screens

Registers an AbstractContainerScreen with a menu using the RegisterMenuScreensEvent. This method should be called on the mod event bus on the physical client.

```java
import net.neoforged.bus.api.SubscribeEvent;
import net.neoforged.fml.event.lifecycle.RegisterMenuScreensEvent;

@SubscribeEvent // on the mod event bus only on the physical client  
public static void registerScreens(RegisterMenuScreensEvent event) {
    event.register(MY_MENU.get(), MyContainerScreen::new);
}

```

--------------------------------

### Registering a Common Configuration Spec in Mod Constructor

Source: https://docs.neoforged.net/docs/1.20.4/misc/config

Registers a common configuration specification within the mod's constructor. This allows NeoForge to load, track, and sync the configuration settings. It requires a `ModConfigSpec` object and specifies the `Type.COMMON` for both client and server-side loading.

```java
ModLoadingContext.get().registerConfig(Type.COMMON, CONFIG_SPEC);
```

--------------------------------

### Define Client Acknowledgement Payload (Java)

Source: https://docs.neoforged.net/docs/1.20.4/networking/configuration-tasks

Defines a custom packet payload for acknowledging configuration tasks on the client side. This record implements `CustomPacketPayload` and specifies a unique `ResourceLocation` for the payload. It includes methods for writing data (none in this case) and returning the payload's ID.

```java
public record AckPayload() implements CustomPacketPayload {  
    public static final ResourceLocation ID = new ResourceLocation("mymod:ack");  
      
    @Override  
    public void write(final FriendlyByteBuf buffer) {  
        // No data to write  
    }  
  
    @Override  
    public ResourceLocation id() {  
        return ID;  
    }  
}
```

--------------------------------

### Convert Between NBT and JSON Formats

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Shows how to convert data between NBT and JSON formats using DynamicOps. This allows for interoperability between different data representations within the game.

```java
// Convert Tag to JsonElement  
// Let exampleTag be a Tag  
JsonElement convertedJson = NbtOps.INSTANCE.convertTo(JsonOps.INSTANCE, exampleTag);
```

--------------------------------

### Generate Special Recipe with SpecialRecipeBuilder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/recipes

Explains how to generate empty JSONs for dynamic recipes that don't fit the standard format, such as dying armor or fireworks, using SpecialRecipeBuilder. The builder is initialized via the `#special` method.

```java
// In RecipeProvider#buildRecipes(writer)  
SpecialRecipeBuilder.special(dynamicRecipeSerializer)  
  .save(writer, name); // Add data to builder  

```

--------------------------------

### Create KeyMapping with Shift Modifier

Source: https://docs.neoforged.net/docs/1.20.4/misc/keymappings

Demonstrates how to create a KeyMapping that requires the SHIFT key to be held down. This is useful for differentiating actions when a modifier key is pressed.

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

### Register Custom Banner Patterns with NeoForge

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/incode

This Java code demonstrates how to register custom banner patterns using NeoForge's DeferredRegister. It shows the creation of a BannerPattern instance and its registration with a unique resource location, enabling its use in the loom.

```java
import net.minecraft.core.registries.Registries;
import net.minecraft.resources.ResourceLocation;
import net.minecraft.world.item.BannerPattern;
import net.minecraft.world.item.crafting.BannerPatternBuilder;
import net.minecraftforge.common.crafting.BannerPatternBuilder;
import net.minecraftforge.registries.DeferredRegister;
import net.minecraftforge.registries.RegistryObject;

public class ModBannerPatterns {

    // Create a DeferredRegister for BannerPattern objects
    private static final DeferredRegister<BannerPattern> REGISTER = DeferredRegister.create(Registries.BANNER_PATTERN, "examplemod");

    // Register a new banner pattern
    public static final RegistryObject<BannerPattern> EXAMPLE_PATTERN = REGISTER.register("example_pattern", () -> new BannerPattern("examplemod:ep"));

    // Example of how to use it in a setup method (e.g., FMLCommonSetupEvent)
    public static void registerPatterns() {
        // The registration happens automatically when the DeferredRegister is initialized
        // You can then use EXAMPLE_PATTERN.get() to get the registered BannerPattern object
    }

    // You would typically call ModBannerPatterns.registerPatterns() during your mod's setup phase.
    // The DeferredRegister itself needs to be added to the Forge event bus.
}
```

--------------------------------

### Datagen Custom Sounds.json with SoundDefinitionsProvider

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/sounds

Extends SoundDefinitionsProvider and overrides registerSounds to define custom sounds for sounds.json. Supports setting volume, pitch, weight, attenuation, streaming, preloading, subtitles, and replacement.

```java
public class MySoundDefinitionsProvider extends SoundDefinitionsProvider {
    public MySoundDefinitionsProvider(PackOutput output, ExistingFileHelper existingFileHelper) {
        super(output, "examplemod", existingFileHelper);
    }

    @Override
    public void registerSounds() {
        add(MySoundsClass.MY_SOUND, SoundDefinition.definition()
                .with(
                        sound("examplemod:sound_1", SoundDefinition.SoundType.SOUND)
                                .volume(0.8f)
                                .pitch(1.2f)
                                .weight(2)
                                .attenuationDistance(8)
                                .stream(true)
                                .preload(true),
                        sound("examplemod:sound_2")
                )
                .subtitle("sound.examplemod.sound_1")
                .replace(true)
        );
    }
}
```

--------------------------------

### Check KeyMapping Click in Game (ClientTickEvent)

Source: https://docs.neoforged.net/docs/1.20.4/misc/keymappings

Shows how to check if a KeyMapping has been clicked within the game by listening to the ClientTickEvent. The consumeClick() method is used to ensure the logic executes only once per click.

```java
@SubscribeEvent // on the mod event bus only on the physical client
public static void onClientTick(ClientTickEvent event) {
  if (event.phase == TickEvent.Phase.END) { // Only call code once as the tick event is called twice every tick
    while (EXAMPLE_MAPPING.get().consumeClick()) {
      // Execute logic to perform on click here
    }
  }
}
```

--------------------------------

### BlockModelProvider Helpers for Special Models

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

In addition to cube models, BlockModelProvider offers helpers for specialized block types. These include models for crops, cross-shaped foliage, torches, wall torches, and carpets, simplifying the creation of unique block appearances.

```java
crop: Accepts a texture location, returning a crop-like model with the given texture, as used by the four vanilla crops.
cross: Accepts a texture location, returning a cross model with the given texture, as used by flowers, saplings and many other foliage blocks.
torch: Accepts a texture location, returning a torch model with the given texture.
wall_torch: Accepts a texture location, returning a wall torch model with the given texture (wall torches are separate blocks from standing torches).
carpet: Accepts a texture location, returning a carpet model with the given texture.
```

--------------------------------

### Generate Stonecutting Recipe with SingleItemRecipeBuilder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/recipes

Demonstrates how to create a stonecutting recipe using SingleItemRecipeBuilder. This builder can also be used for custom single item recipes with a serializer. The recipe group and unlock criteria can be set before saving.

```java
// In RecipeProvider#buildRecipes(writer)  
SingleItemRecipeBuilder builder = SingleItemRecipeBuilder.stonecutting(input, RecipeCategory.MISC, result)  
  .unlockedBy("criteria", criteria) // How the recipe is unlocked  
  .save(writer); // Add data to builder  

```

--------------------------------

### Custom Particle Implementation Extending TextureSheetParticle in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/particles

This code illustrates how to create a custom particle effect by extending `TextureSheetParticle`. It shows how to initialize the particle with a `SpriteSet` and override the `tick` method to control particle animation and movement. This approach simplifies rendering and animation compared to extending `Particle` directly.

```java
public class MyParticle extends TextureSheetParticle {
    private final SpriteSet spriteSet;

    // First four parameters are self-explanatory. The SpriteSet parameter is provided by the
    // ParticleProvider, see below. You may also add additional parameters as needed, e.g. xSpeed/ySpeed/zSpeed.
    public MyParticle(ClientLevel level, double x, double y, double z, SpriteSet spriteSet) {
        super(level, x, y, z);
        this.spriteSet = spriteSet;
        this.gravity = 0; // Our particle floats in midair now, because why not.
    }

    @Override
    public void tick() {
        // Set the sprite for the current particle age, i.e. advance the animation.
        setSpriteFromAge(spriteSet);
        // Let super handle further movement. You may replace this with your own movement if needed.
        // You may also override move() if you only want to modify the built-in movement.
        super.tick();
    }
}
```

--------------------------------

### Check Physical Client/Server Side with FMLEnvironment.dist

Source: https://docs.neoforged.net/docs/1.20.4/concepts/sides

FMLEnvironment.dist checks the physical environment, returning Dist.CLIENT for a physical client and Dist.SERVER for a physical server. This is important for handling client-only classes safely. Calls to client-only code should be wrapped in a Dist.CLIENT check and ideally delegated to a separate class to prevent accidental classloading on the server.

```java
if (FMLEnvironment.dist == Dist.CLIENT) {
    SomeClientClass.someClientMethod();
}
```

--------------------------------

### Create Variant Blockstates with ConfiguredModel.Builder

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Explains how to use ConfiguredModel.Builder within a VariantBlockStateBuilder to define models for specific block states. It covers adding models directly and using `forAllStates` to generate models based on block properties.

```java
VariantBlockStateBuilder variantBuilder = getVariantBuilder(MyBlocksClass.EXAMPLE_BLOCK.get());
VariantBlockStateBuilder.PartialBlockstate partialState = variantBuilder.partialState();
variantBuilder.addModels(
        partialState,
        partialState.modelForState()
                .modelFile(models().withExistingParent("example_variant_model", this.mcLoc("block/cobblestone")))
                .uvlock(true)
);
variantBuilder.forAllStates(state -> {
    return ConfiguredModel.builder()
            .modelFile(models().withExistingParent("example_variant_model", this.mcLoc("block/cobblestone")))
            .rotationY((int) state.getValue(BlockStateProperties.HORIZONTAL_FACING).toYRot())
            .build();
});
```

--------------------------------

### Create and Register DataMapType (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datamaps

Demonstrates the creation of a DataMapType for Items, associating them with DropHealing values, and its registration. The DataMapType is built using a builder, specifying its ID, the registry it applies to, and its Codec. Registration occurs via the RegisterDataMapTypesEvent.

```java
public static final DataMapType<Item, DropHealing> DROP_HEALING = DataMapType.builder(
        new ResourceLocation("mymod:drop_healing"), Registries.ITEM, DropHealing.CODEC
).build();

// Registration happens via RegisterDataMapTypesEvent#register

```

--------------------------------

### Create DeferredRegister for Blocks

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Initializes a DeferredRegister instance specifically for Block objects. It requires the registry to use (e.g., BuiltInRegistries.BLOCKS) and the mod ID. This sets up the system for deferred registration of blocks.

```java
public static final DeferredRegister<Block> BLOCKS = DeferredRegister.create(
        // The registry we want to use.
        // Minecraft's registries can be found in BuiltInRegistries, NeoForge's registries can be found in NeoForgeRegistries.
        // Mods may also add their own registries, refer to the individual mod's documentation or source code for where to find them.
        BuiltInRegistries.BLOCKS,
        // Our mod id.
        ExampleMod.MOD_ID
);

```

--------------------------------

### Register Custom Tool Tier with Mixed Tiers and ResourceLocations

Source: https://docs.neoforged.net/docs/1.20.4/items/tools

Demonstrates registering a custom tool tier with a mix of `Tier` objects and `ResourceLocation` identifiers for lower and higher tiers. This provides flexibility in defining the tool's position in the mining hierarchy, allowing integration with tiers from other mods.

```java
public static final Tier COPPER_TIER = new SimpleTier(...);  

static {
    TierSortingRegistry.registerTier(
            COPPER_TIER,
            new ResourceLocation("minecraft", "copper"),
            List.of(Tiers.STONE),
            //We can mix and match Tiers and ResourceLocations here.
            List.of(Tiers.IRON, new ResourceLocation("mekanism", "osmium"))
    );
}
```

--------------------------------

### Create Either Codec in Java

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Generates a codec for two different encoding/decoding methods using Codec#either. It attempts to decode with the first codec, and if it fails, tries the second. If both fail, the error from the second failure is returned.

```java
public static final Codec<Either<Integer, String>> EITHER_CODEC = Codec.either(
  Codec.INT,
  Codec.STRING
);

```

--------------------------------

### Merging sounds.json Across Resource Packs

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/sounds

Demonstrates how sounds.json files from different resource packs are merged. The 'replace' property dictates whether sounds from a higher-priority pack overwrite or are added to sounds from lower-priority packs.

```json
{
  "sound_1": {
    "sounds": [
      "sound_5",
      "sound_1"
    ]
  },
  "sound_2": {
    "sounds": [
      "sound_2"
    ]
  },
  "sound_3": {
    "sounds": [
      "sound_7",
      "sound_3"
    ]
  },
  "sound_4": {
    "sounds": [
      "sound_8"
    ]
  }
}
```

--------------------------------

### Accessing Mod Properties in Java

Source: https://docs.neoforged.net/docs/1.20.4/gettingstarted/modfiles

Demonstrates how to access custom properties defined for a mod within its Java code. Properties are retrieved from the mod's info object using a specific key.

```java
@Mod("mod1")
public class ModOne {
  
    private final String key;
  
    public ModOne(ModContainer container) {
        // Will store 'value1' in key
        this.key = (String) container.getModInfo().getModProperties().get("key");
    }
}

@Mod("mod2")
public class ModTwo {
  
    private final String key;
  
    public ModTwo(ModContainer container) {
        // Will store 'value2' in key
        this.key = (String) container.getModInfo().getModProperties().get("key");
    }
}
```

--------------------------------

### Define a Whitelisted Config Value with NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/misc/config

Demonstrates how to define a configuration value that must be one of the values present in a provided collection using `ModConfigSpec.Builder#defineInList`. This ensures the configuration adheres to a predefined set of acceptable options.

```java
// For some ModConfigSpec.Builder builder
// Assuming T is the type of elements in the collection
Collection<T> allowedValues = ...;
ConfigValue<T> whitelistedValue = builder.comment("Value must be one of the allowed options")
  .defineInList("whitelisted_value_name", defaultValue, allowedValues, type);
```

--------------------------------

### Java Game Test Annotations and Structure

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Demonstrates how to define Game Tests in Java using annotations like @GameTestHolder, @GameTest, and @PrefixGameTestTemplate. It shows how these annotations control the naming and location of structure templates, which are .nbt files defining test scenes.

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

### Register Game Tests with RegisterGameTestsEvent

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Shows how to register test classes or methods programmatically using the RegisterGameTestsEvent. This involves subscribing to the event on the mod event bus and using the event's 'register' method. Test methods registered this way require the mod ID to be supplied to GameTest#templateNamespace.

```java
// In some class
@SubscribeEvent // on the mod event bus
public static void registerTests(RegisterGameTestsEvent event) {
  event.register(ExampleGameTests.class);
}

// In ExampleGameTests
@GameTest(templateNamespace = MODID)
public static void exampleTest3(GameTestHelper helper) {
  // Perform setup
}
```

--------------------------------

### Implement Custom ParticleOptions

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/particles

Defines a custom ParticleOptions class to store additional data for particles. It requires implementing `writeToNetwork` for network serialization and `writeToString` for command string representation. A Deserializer is also provided for command and network deserialization.

```java
public class MyParticleOptions implements ParticleOptions {
    // Does not need any parameters, but may define any fields necessary for the particle to work.
    public MyParticleOptions() {}
      
    @Override
    public void writeToNetwork(FriendlyByteBuf buf) {
        // Write your custom info to the given buffer.
    }
  
    @Override
    public String writeToString() {
        // Return a stringified version of your custom info, for use in commands.
        // We don't have any info in this type, so we return the empty string.
        return "";
    }
      
    // The deserializer object to use. We will discuss how to use this in a moment.
    public static final ParticleOptions.Deserializer<MyParticleOptions> DESERIALIZER = 
        new ParticleOptions.Deserializer<MyParticleOptions>() {
            public MyParticleOptions fromCommand(ParticleType<MyParticleOptions> type, StringReader reader)
                    throws CommandSyntaxException {
                // You may deserialize things using the given StringReader and pass them to your
                // particle options object if needed.
                return new MyParticleOptions();
            }
              
            public MyParticleOptions fromNetwork(ParticleType<MyParticleOptions> type, FriendlyByteBuf buf) {
                // Similar to above, deserialize any needed info from the given buffer.
                return new MyParticleOptions();
            }
        };
}

```

--------------------------------

### Java Client-Side Event for Registering Item Properties

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models

This Java code snippet demonstrates how to register a custom item property on the client side using NeoForge's event system. It utilizes `ItemProperties#register` within a `FMLClientSetupEvent` to link an item to a method that calculates its override value. Ensure this code runs on the main thread using `event.enqueueWork` as `ItemProperties#register` is not thread-safe.

```java
@SubscribeEvent
public static void onClientSetup(FMLClientSetupEvent event) {
    event.enqueueWork(() -> {
        ItemProperties.register(
                ExampleItems.EXAMPLE_ITEM,
                new ResourceLocation("examplemod", "property"),
                (stack, level, player, seed) -> someMethodThatReturnsAFloat()
        );
    });
}
```

--------------------------------

### Triggering SimpleCriterionTrigger Instances (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/advancements

Demonstrates how to trigger a SimpleCriterionTrigger instance, which is unique for each trigger. This method internally calls the `#trigger` method to handle listener checking.

```java
public void trigger(ServerPlayer player, ItemStack stack) {
  this.trigger(player,
    // The condition checker method within the SimpleCriterionTrigger.SimpleInstance subclass
    triggerInstance -> triggerInstance.matches(stack)
  );
}
```

--------------------------------

### Defining Codec for SimpleCriterionTrigger Instance Serialization (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/advancements

Shows how to define a codec for serializing and deserializing SimpleCriterionTrigger instances. The codec is typically defined as a constant within the instance implementation.

```java
class ExampleTrigger extends SimpleCriterionTrigger<ExampleTrigger.ExampleTriggerInstance> {
  @Override
  public Codec<ExampleTriggerInstance> codec() {
    return ExampleTriggerInstance.CODEC;
  }
  // ...
  public class ExampleTriggerInstance implements SimpleCriterionTrigger.SimpleInstance {
    public static final Codec<ExampleTriggerInstance> CODEC = ...;
    // ...
  }
}
```

```java
RecordCodecBuilder.create(instance -> instance.group(
  ExtraCodecs.strictOptionalField(EntityPredicate.ADVANCEMENT_CODEC, "player").forGetter(ExampleTriggerInstance::player),
  ItemPredicate.CODEC.fieldOf("item").forGetter(ExampleTriggerInstance::item)
).apply(instance, ExampleTriggerInstance::new));
```

--------------------------------

### Generate Smithing Transform Recipe with SmithingTransformRecipeBuilder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/recipes

Shows how to generate smithing recipes that transform an item using SmithingTransformRecipeBuilder. Custom recipes can also be created with a serializer. Unlock criteria can be specified before saving.

```java
// In RecipeProvider#buildRecipes(writer)  
SmithingTransformRecipeBuilder builder = SmithingTransformRecipeBuilder.smithing(template, base, addition, RecipeCategory.MISC, result)  
  .unlocks("criteria", criteria) // How the recipe is unlocked  
  .save(writer, name); // Add data to builder  

```

--------------------------------

### Play Sound at Location (Level Class)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/sounds

Allows playing a `SoundEvent` at a specific 3D location. The client plays the sound to the local player if provided, while the server sends a packet to other players. Useful for client-initiated actions on both sides or server-initiated actions for all players.

```java
void playSound(Player player, double x, double y, double z, SoundEvent soundEvent, SoundSource soundSource, float volume, float pitch)
void playSound(Player player, BlockPos pos, SoundEvent soundEvent, SoundSource soundSource, float volume, float pitch)
```

--------------------------------

### Handle Codec Parsing Results with DataResult

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Demonstrates how to handle the DataResult returned by Codec parsing operations. This includes extracting successful results or handling errors gracefully using methods like resultOrPartial.

```java
// Let exampleCodec represent a Codec<ExampleJavaObject>  
// Let exampleJson be a JsonElement  
  
// Decode JsonElement into Java object  
DataResult<ExampleJavaObject> result = exampleCodec.parse(JsonOps.INSTANCE, exampleJson);  
  
result  
  // Get result or partial on error, report error message  
  .resultOrPartial(errorMessage -> /* Do something with error message */)  
  // If result or partial is present, do something  
  .ifPresent(decodedObject -> /* Do something with decoded object */);
```

--------------------------------

### Using Custom Loader Builder in Datagen (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Demonstrates how to use a custom loader builder within the block or item model data generation process. It obtains the builder using `getBuilder` and `customLoader`, then applies custom setters.

```java
// This assumes a BlockStateProvider. Use getBuilder("my_cool_block") directly in an ItemModelProvider.
// The parameter for customLoader() is a BiFunction. The parameters of the BiFunction
// are the result of the getBuilder() call and the provider's ExistingFileHelper.
MyLoaderBuilder loaderBuilder = models().getBuilder("my_cool_block").customLoader(MyLoaderBuilder::new);

// Then, call your field setters on the `loaderBuilder`.
```

--------------------------------

### Reusing Default Model Loader with Custom Geometry in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Demonstrates how to reuse the vanilla model loader by tricking the deserializer and then baking custom geometry. This involves creating custom IGeometryLoader, IUnbakedGeometry, and IDynamicBakedModel implementations.

```java
public class MyGeometryLoader implements IGeometryLoader<MyGeometry> {
    public static final MyGeometryLoader INSTANCE = new MyGeometryLoader();
    public static final ResourceLocation ID = new ResourceLocation(...);

    private MyGeometryLoader() {}

    @Override
    public MyGeometry read(JsonObject jsonObject, JsonDeserializationContext context) throws JsonParseException {
        // Trick the deserializer into thinking this is a normal model by removing the loader field and then passing it back into the deserializer.
        jsonObject.remove("loader");
        BlockModel base = context.deserialize(jsonObject, BlockModel.class);
        // other stuff here if needed
        return new MyGeometry(base);
    }
}

public class MyGeometry implements IUnbakedGeometry<MyGeometry> {
    private final BlockModel base;

    // Store the block model for usage below.
    public MyGeometry(BlockModel base) {
        this.base = base;
    }

    @Override
    public BakedModel bake(IGeometryBakingContext context, ModelBaker baker, Function<Material, TextureAtlasSprite> spriteGetter, ModelState modelState, ItemOverrides overrides, ResourceLocation modelLocation) {
        BakedModel bakedBase = new ElementsModel(base.getElements()).bake(context, baker, spriteGetter, modelState, overrides, modelLocation);
        return new MyDynamicModel(bakedBase, /* other parameters here */);
    }

    @Override
    public void resolveParents(Function<ResourceLocation, UnbakedModel> modelGetter, IGeometryBakingContext context) {
        base.resolveParents(modelGetter);
    }
}

public class MyDynamicModel implements IDynamicBakedModel {
    private final BakedModel base;
    // other fields here

    public MyDynamicModel(BakedModel base, /* other parameters here */) {
        this.base = base;
        // set other fields here
    }

    // other override methods here

    @Override
    public List<BakedQuad> getQuads(@Nullable BlockState state, @Nullable Direction side, RandomSource rand, ModelData extraData, @Nullable RenderType renderType) {
        List<BakedQuad> quads = new ArrayList<>();
        // Add the base model's quads. Can also do something different with the quads here, depending on what you need.
        quads.add(base.getQuads(state, side, rand, extraData, renderType));
        // add other elements to the quads list as needed here
        return quads;
    }

    // Apply the base model's transforms to our model as well.
    @Override
    public BakedModel applyTransform(ItemDisplayContext transformType, PoseStack poseStack, boolean applyLeftHandTransform) {
        return base.applyTransform(transformType, poseStack, applyLeftHandTransform);
    }
}
```

--------------------------------

### Supplying DeferredRegister Entries to BlockLootSubProvider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

This code demonstrates how to provide registered blocks from a DeferredRegister to the BlockLootSubProvider's getKnownBlocks method. It streams the entries, maps them to their values, and collects them into a list.

```java
// In some BlockLootSubProvider subclass for some DeferredRegister BLOCK_REGISTRAR  
@Override  
protected Iterable<Block> getKnownBlocks() {  
  return BLOCK_REGISTRAR.getEntries() // Get all registered entries  
    .stream() // Stream the wrapped objects  
    .map(Holder::value) // Get the object if available  
    .toList(); // Create the iterable
```

--------------------------------

### Register Additional Models with ModelEvent.RegisterAdditional (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models

This snippet demonstrates how to register additional models that are not directly tied to blocks or items but are necessary for other functionalities, such as block entity renderers. It utilizes the ModelEvent.RegisterAdditional event, which should be subscribed to the mod event bus and executed only on the physical client. The `register` method takes a ResourceLocation of the model to be registered.

```java
@SubscribeEvent // on the mod event bus only on the physical client  
public static void registerAdditional(ModelEvent.RegisterAdditional event) {  
    event.register(new ResourceLocation("examplemod", "block/example_unused_model"));  
}
```

--------------------------------

### Query Block Capability with Block State and Entity

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Queries a capability on a level at a specific position, optionally providing the block state and block entity for optimization. This is the most comprehensive way to query block capabilities.

```java
var object = level.getCapability(CAP, pos, blockState, blockEntity, context);
if (object != null) {
    // Use object
}

```

--------------------------------

### Generate Block Models with BlockStateProvider - Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Extends BlockStateProvider to register blockstate and block model files. It utilizes helper methods for various block types, such as simpleBlock, logBlock, axisBlock, and horizontalBlock. Requires PackOutput and ExistingFileHelper for data generation.

```java
public class MyBlockStateProvider extends BlockStateProvider {
    public MyBlockStateProvider(PackOutput output, ExistingFileHelper existingFileHelper) {
        super(output, "examplemod", existingFileHelper);
    }
      
    @Override
    protected void registerStatesAndModels() {
        ModelFile exampleModel = models().withExistingParent("example_model", this.mcLoc("block/cobblestone"));
        Block block = MyBlocksClass.EXAMPLE_BLOCK.get();
        ResourceLocation exampleTexture = modLoc("block/example_texture");
        ResourceLocation bottomTexture = modLoc("block/example_texture_bottom");
        ResourceLocation topTexture = modLoc("block/example_texture_top");
        ResourceLocation sideTexture = modLoc("block/example_texture_front");
        ResourceLocation frontTexture = modLoc("block/example_texture_front");

        simpleBlock(block);
        simpleBlock(block, exampleModel);
        simpleBlock(block, ConfiguredModel.builder().build());
        simpleBlockItem(block, exampleModel);
        simpleBlockWithItem(block, exampleModel);
          
        logBlock(block);
        axisBlock(block);
        logBlockWithRenderType(block, "minecraft:cutout");
        axisBlockWithRenderType(block, mcLoc("cutout_mipped"));
          
        horizontalBlock(block, sideTexture, frontTexture, topTexture);
        horizontalBlock(block, exampleModel);
    }
}
```

--------------------------------

### Implement Custom Geometry Loader - Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

This Java code demonstrates the implementation of a custom geometry loader. It follows a singleton pattern and defines a unique ID for registration. The `read` method is responsible for parsing the model JSON and returning a `MyGeometry` object.

```java
public class MyGeometryLoader implements IGeometryLoader<MyGeometry> {
    // It is highly recommended to use a singleton pattern for geometry loaders, as all models can be loaded through one loader.
    public static final MyGeometryLoader INSTANCE = new MyGeometryLoader();
    // The id we will use to register this loader. Also used in the loader datagen class.
    public static final ResourceLocation ID = new ResourceLocation("examplemod", "my_custom_loader");

    // In accordance with the singleton pattern, make the constructor private.
    private MyGeometryLoader() {}

    @Override
    public MyGeometry read(JsonObject jsonObject, JsonDeserializationContext context) throws JsonParseException {
        // Use the given JsonObject and, if needed, the JsonDeserializationContext to get properties from the model JSON.
        // The MyGeometry constructor may have constructor parameters (see below).
        return new MyGeometry();
    }
}
```

--------------------------------

### Create a Custom BakedModelWrapper in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/bakedmodel

Demonstrates how to extend BakedModelWrapper to create a custom wrapper for a BakedModel. The original model is passed to the super constructor, and specific methods can be overridden to modify behavior. The generic parameter can be a more specific subclass of BakedModel if needed.

```java
public class MyBakedModelWrapper extends BakedModelWrapper<BakedModel> {
    // Pass the original model to super.
    public MyBakedModelWrapper(BakedModel originalModel) {
        super(originalModel);
    }
      
    // Override whatever methods you want here. You may also access originalModel if needed.
}
```

--------------------------------

### Registering Particle Providers in Neoforged

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/particles

This Java code demonstrates how to register a custom ParticleProvider for a specific particle type using the RegisterParticleProvidersEvent. It shows how to associate a particle type with its provider factory, often using a method reference.

```java
import net.minecraftforge.client.event.RegisterParticleProvidersEvent;
import net.minecraftforge.eventbus.api.SubscribeEvent;
import net.minecraft.client.particle.ParticleProvider;
import net.minecraft.client.particle.SpriteSet;
import net.minecraft.client.multiplayer.ClientLevel;
import net.minecraft.client.particle.Particle;
import net.minecraft.core.particles.SimpleParticleType;

// Assuming MyParticleTypes and MyParticle are defined elsewhere
// import com.example.MyParticleTypes;
// import com.example.MyParticle;

public class ParticleRegistration {

    @SubscribeEvent // on the mod event bus only on the physical client
    public static void registerParticleProviders(RegisterParticleProvidersEvent event) {
        // Registering a particle provider that uses a SpriteSet
        // MyParticleProvider::new is a method reference to the constructor of MyParticleProvider
        event.registerSpriteSet(MyParticleTypes.MY_PARTICLE.get(), MyParticleProvider::new);

        // Other registration methods exist, like registerSprite and registerSpecial,
        // which map to different factory types (Supplier<TextureSheetParticle> and Supplier<Particle> respectively).
        // Refer to the event source code for more details.
    }

    // Example of a custom ParticleProvider
    public static class MyParticleProvider implements ParticleProvider<SimpleParticleType> {
        private final SpriteSet spriteSet;

        public MyParticleProvider(SpriteSet spriteSet) {
            this.spriteSet = spriteSet;
        }

        @Override
        public Particle createParticle(SimpleParticleType type, ClientLevel level, double x, double y, double z, double xSpeed, double ySpeed, double zSpeed) {
            // Create and return a new instance of your custom particle
            // The parameters 'type' and speed are not used in this simple example but can be utilized.
            return new MyParticle(level, x, y, z, spriteSet);
        }
    }

    // Dummy classes for compilation - replace with your actual implementations
    public static class MyParticleTypes {
        public static final net.minecraft.core.RegistryObject<SimpleParticleType> MY_PARTICLE = null; // Replace with actual registration
    }

    public static class MyParticle extends TextureSheetParticle {
        protected MyParticle(ClientLevel level, double x, double y, double z, SpriteSet spriteSet) {
            super(level, x, y, z);
            this.setSpriteSet(spriteSet);
        }
    }
}

```

--------------------------------

### Register Item Models using ItemModelProvider in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

This Java code demonstrates how to create a custom `ItemModelProvider` to register item models. It extends `ItemModelProvider` and overrides the `registerModels` method to define model parents and textures. Dependencies include NeoForged's data generation API.

```java
public class MyItemModelProvider extends ItemModelProvider {
    public MyItemModelProvider(PackOutput output, ExistingFileHelper existingFileHelper) {
        super(output, "examplemod", existingFileHelper);
    }

    // Assume that EXAMPLE_BLOCK_ITEM and EXAMPLE_ITEM are both DeferredItems
    @Override
    protected void registerModels() {
        // Block items generally use their corresponding block models as parent.
        withExistingParent(MyItemsClass.EXAMPLE_BLOCK_ITEM.getId().toString(), modLoc("block/example_block"));
        // Items generally use a simple parent and one texture. The most common parents are item/generated and item/handheld.
        // In this example, the item texture would be located at assets/examplemod/textures/item/example_item.png.
        // If you want a more complex model, you can use getBuilder() and then work from that, like you would with block models.
        withExistingParent(MyItemsClass.EXAMPLE_ITEM.getId().toString(), mcLoc("item/generated")).texture("layer0", "item/example_item");
        // The above line is so common that there is a shortcut for it. Note that the item registry name and the
        // texture path, relative to textures/item, must match.
        basicItem(MyItemsClass.EXAMPLE_ITEM.get());
    }
}
```

--------------------------------

### Implement Custom Item Renderer with BlockEntityWithoutLevelRenderer (Java)

Source: https://docs.neoforged.net/docs/1.20.4/items/bewlr

This code snippet demonstrates how to register a custom BlockEntityWithoutLevelRenderer for an item in NeoForged. It involves overriding the `initializeClient` method in the `Item` class and providing an instance of your custom renderer via `IClientItemExtensions`. This allows for dynamic rendering of items.

```Java
import net.minecraft.client.resources.model.BakedModel;
import net.minecraft.client.renderer.block.model.ItemTransforms.TransformType;
import net.minecraft.client.renderer.blockentity.BlockEntityRenderer;
import net.minecraft.client.renderer.blockentity.BlockEntityRendererProvider.Context;
import net.minecraft.client.renderer.blockentity.BlockEntityRendererRegistry;
import net.minecraft.client.renderer.blockentity.EntityBlockRenderer;
import net.minecraft.client.renderer.blockentity.StainedGlassPaneRenderer;
import net.minecraft.client.renderer.entity.ItemRenderer;
import net.minecraft.client.resources.model.ModelResourceLocation;
import net.minecraft.core.BlockPos;
import net.minecraft.nbt.CompoundTag;
import net.minecraft.world.item.BlockItem;
import net.minecraft.world.item.Item;
import net.minecraft.world.item.ItemStack;
import net.minecraft.world.level.Level;
import net.minecraft.world.level.block.Block;
import net.minecraft.world.level.block.RenderShape;
import net.minecraft.world.level.block.entity.BlockEntity;
import net.minecraft.world.level.block.entity.BlockEntityType;
import net.minecraft.world.level.block.state.BlockState;
import net.minecraftforge.client.extensions.common.IClientItemExtensions;
import net.minecraftforge.client.model.data.ModelData;
import net.minecraftforge.client.model.data.ModelDataManager;
import net.minecraftforge.client.model.data.ModelProperty;
import net.minecraftforge.fml.loading.FMLLoader;
import net.minecraftforge.fml.util.thread.SidedThreadGroups;

import java.util.function.Consumer;

import com.mojang.blaze3d.vertex.PoseStack;
import com.mojang.blaze3d.vertex.VertexConsumer;

public class MyItem extends Item {
    public MyItem(Properties properties) {
        super(properties);
    }

    // Assuming myBEWLRInstance is a properly initialized instance of your custom BlockEntityWithoutLevelRenderer
    private BlockEntityWithoutLevelRenderer myBEWLRInstance = new MyCustomBEWLR(null); // Replace null with actual Context if needed

    @Override
    public void initializeClient(Consumer<IClientItemExtensions> consumer) {
        consumer.accept(new IClientItemExtensions() {
            @Override
            public BlockEntityWithoutLevelRenderer getCustomRenderer() {
                return myBEWLRInstance;
            }
        });
    }

    // Dummy custom BEWLR for demonstration
    private static class MyCustomBEWLR extends BlockEntityWithoutLevelRenderer {
        public MyCustomBEWLR(Context context) {
            super(context);
        }

        @Override
        public void renderByItem(ItemStack itemStack, ItemDisplayContext ctx, PoseStack poseStack, MultiBufferSource bufferSource, int combinedLight, int combinedOverlay) {
            // Custom rendering logic here
            super.renderByItem(itemStack, ctx, poseStack, bufferSource, combinedLight, combinedOverlay);
        }
    }
}

```

--------------------------------

### Generate Game Tests with @GameTestGenerator

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Demonstrates how to dynamically generate test methods using the @GameTestGenerator annotation. These generator methods must return a Collection of TestFunctions and take no parameters. This allows for programmatic creation of tests.

```java
public class ExampleGameTests {
  @GameTestGenerator
  public static Collection<TestFunction> exampleTests() {
    // Return a collection of TestFunctions
  }
}
```

--------------------------------

### Register a Block using DeferredRegister

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Registers a new Block object with a specified registry name using the DeferredRegister. It takes a registry name and a supplier function that provides the Block instance. The result is stored in a DeferredHolder.

```java
public static final DeferredHolder<Block, Block> EXAMPLE_BLOCK = BLOCKS.register(
        "example_block", // Our registry name.
        () -> new Block(...)
);

```

--------------------------------

### Render Labels on Container Screen (Java)

Source: https://docs.neoforged.net/docs/1.20.4/gui/screens

Renders text labels on the container screen. It calls the superclass method first and then uses the font renderer to draw the specified string at given coordinates.

```java
import net.minecraft.client.gui.GuiGraphics;

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

### Define Particle Sprites with ParticleDescriptionProvider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/particles

This Java code snippet demonstrates how to extend `ParticleDescriptionProvider` to define particle sprite definitions. It shows how to add single sprite particles and multi-sprite particles using `sprite()` and `spriteSet()` methods, respectively. Ensure that all referenced particle types and textures exist.

```java
public class MyParticleDescriptionProvider extends ParticleDescriptionProvider {
    // Get the parameters from GatherDataEvent.
    public AMParticleDefinitionsProvider(PackOutput output, ExistingFileHelper existingFileHelper) {
        super(output, existingFileHelper);
    }

    // Assumes that all the referenced particles actually exists. Replace "examplemod" with your mod id.
    @Override
    protected void addDescriptions() {
        // Adds a single sprite particle definition with the file at
        // assets/examplemod/textures/particle/my_single_particle.png.
        sprite(MyParticleTypes.MY_SINGLE_PARTICLE.get(), new ResourceLocation("examplemod", "my_single_particle"));
        // Adds a multi sprite particle definition, with a vararg parameter. Alternatively accepts a list.
        spriteSet(MyParticleTypes.MY_MULTI_PARTICLE.get(),
                new ResourceLocation("examplemod", "my_multi_particle_0"),
                new ResourceLocation("examplemod", "my_multi_particle_1"),
                new ResourceLocation("examplemod", "my_multi_particle_2")
        );
        // Alternative for the above, appends "_<index>" to the base name given, for the given amount of textures.
        spriteSet(MyParticleTypes.MY_ALT_MULTI_PARTICLE.get(),
                // The base name.
                new ResourceLocation("examplemod", "my_multi_particle"),
                // The amount of textures.
                3,
                // Whether to reverse the list, i.e. start at the last element instead of the first.
                false
        );
    }
}
```

--------------------------------

### Register Item Capability Provider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Registers a capability provider for specific items. The provider function receives the item stack and context, allowing you to return a capability like IItemHandler based on the item.

```java
event.registerItem(  
    Capabilities.ItemHandler.ITEM, // capability to register for  
    (itemStack, context) -> <return the IItemHandler for the itemStack>,  
    // items to register for  
    MY_ITEM,  
    MY_OTHER_ITEM);

```

--------------------------------

### Root Transform with Components (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models

Applies root-level transformations using individual components like translation, rotation, scale, post-rotation, and origin. This method offers more granular control and readability for transformations.

```json
{
  "transform": {
    "translation": [0, 0, 0],
    "rotation": {"x": 90},
    "scale": [1, 1, 1],
    "post_rotation": [0, 45, 0],
    "origin": "center"
  }
}
```

--------------------------------

### Define Recipe Type in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/custom

Shows how to associate a custom recipe with a specific `RecipeType`. If a suitable existing type is not available, a new `RecipeType` must be registered. The `Recipe#getType` method should return the appropriate `RecipeType` instance.

```java
// For some Supplier<RecipeType<?>> EXAMPLE_TYPE
// In ExampleRecipe
@Override
public RecipeType<?> getType() {
  return EXAMPLE_TYPE.get();
}
```

--------------------------------

### Implement Advancement Generation Logic

Source: https://docs.neoforged.net/docs/1.20.4/datagen/advancements

This code demonstrates the structure of an AdvancementGenerator, which is responsible for creating advancements. The generate method receives registry lookups, a writer for advancements, and an existing file helper.

```java
// In some subclass of ForgeAdvancementProvider$AdvancementGenerator or as a lambda reference  
  
@Override  
public void generate(HolderLookup.Provider registries, Consumer<Advancement> writer, ExistingFileHelper existingFileHelper) {  
  // Build advancements here  
}
```

--------------------------------

### Custom Particle Provider Implementation in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/particles

This Java code defines a custom ParticleProvider responsible for creating particle instances on the client side. It takes a SpriteSet during initialization and uses it within the createParticle method to instantiate a new particle.

```java
import net.minecraft.client.particle.Particle;
import net.minecraft.client.particle.ParticleProvider;
import net.minecraft.client.particle.SpriteSet;
import net.minecraft.client.multiplayer.ClientLevel;
import net.minecraft.core.particles.SimpleParticleType;

// Assuming MyParticle is defined elsewhere
// import com.example.MyParticle;

// The generic type of ParticleProvider must match the type of the particle type this provider is for.
public class MyParticleProvider implements ParticleProvider<SimpleParticleType> {
    // A set of particle sprites.
    private final SpriteSet spriteSet;

    // The registration function passes a SpriteSet, so we accept that and store it for further use.
    public MyParticleProvider(SpriteSet spriteSet) {
        this.spriteSet = spriteSet;
    }

    // This is where the magic happens. We return a new particle each time this method is called!
    // The type of the first parameter matches the generic type passed to the super interface.
    @Override
    public Particle createParticle(SimpleParticleType type, ClientLevel level,
            double x, double y, double z, double xSpeed, double ySpeed, double zSpeed) {
        // We don't use the type and speed, and pass in everything else. You may of course use them if needed.
        // Assuming MyParticle constructor takes level, x, y, z, and spriteSet.
        return new MyParticle(level, x, y, z, spriteSet);
    }
}

// Dummy class for compilation - replace with your actual implementation
class MyParticle extends TextureSheetParticle {
    protected MyParticle(ClientLevel level, double x, double y, double z, SpriteSet spriteSet) {
        super(level, x, y, z);
        this.setSpriteSet(spriteSet);
    }
}

```

--------------------------------

### Criterion Creation Helper Method (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/advancements

Provides a static helper method in Java to easily create a Criterion object for data generation. It uses a DeferredHolder for the trigger and constructs the trigger instance with specified player and item predicates.

```java
// In this example, EXAMPLE_TRIGGER is a DeferredHolder<CriterionTrigger<?>>
public static Criterion<ExampleTriggerInstance> instance(ContextAwarePredicate player, ItemPredicate item) {
  return EXAMPLE_TRIGGER.get().createCriterion(new ExampleTriggerInstance(Optional.of(player), item));
}
```

--------------------------------

### Register Item Color Handler (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models

Registers a custom color handler for items on the physical client. This enables dynamic tinting of item textures. The handler receives the item stack and tint index, returning an RGB color value. This is crucial for items that use generated models with multiple tint layers.

```java
@SubscribeEvent // on the mod event bus only on the physical client  
public static void registerItemColorHandlers(RegisterColorHandlersEvent.Item event) {  
    // Parameters are the item stack and the tint index.  
    event.register((stack, tintIndex) -> {  
            // Like above, replace with your own calculation. Vanilla values are in the ItemColors class.  
            // Also like above, tint index -1 means no tint and should use a default value instead.  
            return 0xFFFFFF;  
    });  
}

```

--------------------------------

### Declare Access Transformers in mods.toml

Source: https://docs.neoforged.net/docs/1.20.4/advanced/accesstransformers

This snippet demonstrates how to declare Access Transformer files within the mods.toml configuration file. Each entry specifies the path to an AT file that will be applied. Multiple entries can be used to include several AT files.

```toml
[[accessTransformers]]
file="META-INF/accesstransformer.cfg"

```

```toml
[[accessTransformers]]
file="accesstransformer_main.cfg"

[[accessTransformers]]
file="accesstransformer_additions.cfg"

```

--------------------------------

### Creating a Custom Recipe Type Tag Provider in NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/datagen/tags

This Java code defines a constructor for a custom `RecipeTypeTagsProvider` in NeoForged. It extends `TagsProvider` and initializes it with the necessary parameters for generating tags related to recipe types.

```java
public RecipeTypeTagsProvider(PackOutput output, CompletableFuture<HolderLookup.Provider> registries, ExistingFileHelper fileHelper) {  
  super(output, Registries.RECIPE_TYPE, registries, MOD_ID, fileHelper);
}

```

--------------------------------

### Register Armor Items (Java)

Source: https://docs.neoforged.net/docs/1.20.4/items/tools

This Java code demonstrates how to register armor items using a custom armor material. It utilizes NeoForged's deferred registration system to create instances of ArmorItem for each armor slot (HELMET, CHESTPLATE, LEGGINGS, BOOTS), linking them to the previously defined COPPER_ARMOR_MATERIAL.

```java
//ITEMS is a DeferredRegister<Item>  
public static final Supplier<ArmorItem> COPPER_HELMET = ITEMS.register("copper_helmet", () -> new ArmorItem(  
        // The armor material to use.  
        COPPER_ARMOR_MATERIAL,  
        // The armor type to use.  
        ArmorItem.Type.HELMET,  
        // The item properties. We don't need to set the durability here because ArmorItem handles that for us.  
        new Item.Properties()
));
public static final Supplier<ArmorItem> COPPER_CHESTPLATE = ITEMS.register("copper_chestplate", () -> new ArmorItem(...));
public static final Supplier<ArmorItem> COPPER_LEGGINGS = ITEMS.register("copper_leggings", () -> new ArmorItem(...));
public static final Supplier<ArmorItem> COPPER_BOOTS = ITEMS.register("copper_boots", () -> new ArmorItem(...));

```

--------------------------------

### Perform Assertions in Game Tests

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Explains how to perform assertions within a Game Test using `GameTestHelper`. Assertions are typically implemented by throwing a `GameTestAssertException` if a condition is not met, which signals a test failure.

```java
if (!condition) {
    throw new GameTestAssertException("Condition not met");
}
```

--------------------------------

### Generate Conditional Recipes with ConditionalRecipe.Builder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/recipes

Details the process of data-generating conditional recipes using ConditionalRecipe.Builder. This involves adding conditions and corresponding recipes, and optionally generating or setting advancements.

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

### Create Block Capability with Context

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Declares a BlockCapability for side-aware IItemHandlers, specifying the queried type and context type. The capability is identified by a unique ResourceLocation.

```java
public static final BlockCapability<IItemHandler, @Nullable Direction> ITEM_HANDLER_BLOCK =  
    BlockCapability.create(  
        // Provide a name to uniquely identify the capability.  
        new ResourceLocation("mymod", "item_handler"),  
        // Provide the queried type. Here, we want to look up `IItemHandler` instances.  
        IItemHandler.class,  
        // Provide the context type. We will allow the query to receive an extra `Direction side` parameter.  
        Direction.class);

```

--------------------------------

### Screen Ticking Logic for Client-Side Updates in Java

Source: https://docs.neoforged.net/docs/1.20.4/gui/screens

Shows how to implement ticking logic within a screen for client-side operations, such as updating an EditBox for cursor blinking. The tick method is called every game tick to perform ongoing updates.

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

### Register Data Provider for Item Model Generation in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

This Java code shows how to register the custom `MyItemModelProvider` with the data generation system using the `GatherDataEvent`. It obtains necessary components like `PackOutput` and `ExistingFileHelper` from the event and adds the provider to the generator.

```java
@SubscribeEvent // on the mod event bus
public static void gatherData(GatherDataEvent event) {
    DataGenerator generator = event.getGenerator();
    PackOutput output = generator.getPackOutput();
    ExistingFileHelper existingFileHelper = event.getExistingFileHelper();

    // other providers here
    generator.addProvider(
            event.includeClient(),
            new MyItemModelProvider(output, existingFileHelper)
    );
}
```

--------------------------------

### Define a Boolean Config Value with NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/misc/config

Demonstrates defining a simple boolean configuration value using `ModConfigSpec.Builder#define`. This is a straightforward way to include boolean settings in your mod's configuration.

```java
// For some ModConfigSpec.Builder builder
ConfigValue<Boolean> booleanValue = builder.comment("A boolean setting")
  .define("boolean_value_name", defaultValue);
```

--------------------------------

### Registering Recipe Provider with DataGenerator (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/recipes

This code snippet demonstrates how to register a custom RecipeProvider with the DataGenerator using the @SubscribeEvent annotation. It ensures the provider runs only during server data generation.

```java
@SubscribeEvent // on the mod event bus  
public static void gatherData(GatherDataEvent event) {  
    event.getGenerator().addProvider(  
        // Tell generator to run only when server data are generating  
        event.includeServer(),  
        MyRecipeProvider::new  
    );  
}  

```

--------------------------------

### Play Local Sound (Client-Side)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/sounds

Plays a `SoundEvent` directly on the client without server interaction. It can optionally delay the sound based on player distance. Primarily used for client-side effects or sounds initiated by server packets.

```java
void playLocalSound(double x, double y, double z, SoundEvent soundEvent, SoundSource soundSource, float volume, float pitch, boolean distanceDelay)
void playLocalSound(BlockPos pos, SoundEvent soundEvent, SoundSource soundSource, float volume, float pitch, boolean distanceDelay)
```

--------------------------------

### Texture Assignment in ModelBuilder (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Demonstrates how to assign textures to a model using the `#texture` method in ModelBuilder. This method takes a key and a ResourceLocation for the texture, allowing for dynamic texture definition in generated models.

```java
modelBuilder.texture("particle", new ResourceLocation("minecraft", "block/stone"));
modelBuilder.texture("all", new ResourceLocation("mymod", "block/my_block"));
```

--------------------------------

### Register ParticleDescriptionProvider in GatherDataEvent (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/particles

This Java code snippet shows how to register your custom `MyParticleDescriptionProvider` within the `GatherDataEvent`. It retrieves the `DataGenerator`, `PackOutput`, and `ExistingFileHelper` from the event and adds the provider to the generator. This ensures your particle definitions are processed during the data generation phase.

```java
@SubscribeEvent // on the mod event bus
public static void gatherData(GatherDataEvent event) {
    DataGenerator generator = event.getGenerator();
    PackOutput output = generator.getPackOutput();
    ExistingFileHelper existingFileHelper = event.getExistingFileHelper();

    // other providers here
    generator.addProvider(
            event.includeClient(),
            new MyParticleDescriptionProvider(output, existingFileHelper)
    );
}
```

--------------------------------

### Advancement Rewards Configuration (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/advancements

Defines the rewards that can be given out when an advancement is completed. This includes experience points, loot tables, recipes, and functions.

```json
// In some advancement JSON
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

### Registering Sound Events with NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/sounds

This Java code demonstrates how to register custom SoundEvents for a Minecraft mod using NeoForged's DeferredRegister. It covers creating both variable-range (attenuating) and fixed-range (non-attenuating) sound events. Ensure your mod ID and resource locations are correctly set.

```java
public class MySoundsClass {
    // Assuming that your mod id is examplemod
    public static final DeferredRegister<SoundEvent> SOUND_EVENTS =
            DeferredRegister.create(BuiltInRegistries.SOUND_EVENT, "examplemod");

    // All vanilla sounds use variable range events.
    public static final Supplier<SoundEvent> MY_SOUND = SOUND_EVENTS.register(
            "my_sound", // must match the resource location on the next line
            () -> SoundEvent.createVariableRangeEvent(new ResourceLocation("examplemod", "my_sound"))
    );

    // There is a currently unused method to register fixed range (= non-attenuating) events as well:
    public static final Supplier<SoundEvent> MY_FIXED_SOUND = SOUND_EVENTS.register("my_fixed_sound",
            // 16 is the default range of sounds. Be aware that due to OpenAL limitations,
            // values above 16 have no effect and will be capped to 16.
            () -> SoundEvent.createFixedRangeEvent(new ResourceLocation("examplemod", "my_fixed_sound"), 16)
    );
}
```

```java
public ExampleMod(IEventBus modBus) {
    MySoundsClass.SOUND_EVENTS.register(modBus);
    // other things here
}
```

--------------------------------

### Mark Data Map as Synced (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datamaps

Illustrates how to mark a DataMapType as synced for client-server communication. The `synced` method on the DataMapType.Builder is used, providing a network codec and a mandatory flag. This allows for efficient data transfer and control over client compatibility.

```java
DataMapType.builder<Item, DropHealing>(
        new ResourceLocation("mymod:drop_healing"), Registries.ITEM, DropHealing.CODEC
).synced(networkCodec, mandatory).build();

```

--------------------------------

### Registering Particle Types with DeferredRegister in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/particles

This snippet demonstrates how to register custom particle types in NeoForged using `DeferredRegister` and `ParticleType`. It shows the creation of a `ParticleType` registry and the registration of a simple particle type using `SimpleParticleType`. This is essential for server-side particle management.

```java
public class MyParticleTypes {
    // Assuming that your mod id is examplemod
    public static final DeferredRegister<ParticleType<?>> PARTICLE_TYPES =
            DeferredRegister.create(BuiltInRegistries.PARTICLE_TYPE, "examplemod");

    // The easiest way to add new particle types is reusing vanilla's SimpleParticleType.
    // Implementing a custom ParticleType is also possible, see below.
    public static final Supplier<SimpleParticleType> MY_PARTICLE = PARTICLE_TYPES.register(
            // The name of the particle type.
            "my_particle",
            // The supplier. The boolean parameter denotes whether setting the Particles option in the
            // video settings to Minimal will affect this particle type or not; this is false for
            // most vanilla particles, but true for e.g. explosions, campfire smoke, or squid ink.
            () -> new SimpleParticleType(false)
    );
}
```

--------------------------------

### Block Breaking Pseudocode

Source: https://docs.neoforged.net/docs/1.20.4/blocks

A pseudocode representation of the block breaking process, illustrating the sequence of actions from initial click to final breaking.

```pseudocode
leftClick();
initiatingStage();
while (leftClickIsBeingHeld()) {
    miningStage();
    if (blockIsBroken()) {
        actuallyBreakingStage();
        break;
    }
}
```

--------------------------------

### Create and Populate NBT CompoundTag in Java

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/nbt

Demonstrates how to create a CompoundTag object and add various data types like integers, strings, and doubles. This is the foundational step for storing structured data in NBT format.

```java
CompoundTag tag = new CompoundTag();
tag.putInt("Color", 0xffffff);
tag.putString("Level", "minecraft:overworld");
tag.putDouble("IAmRunningOutOfIdeasForNamesHere", 1d);

```

--------------------------------

### Check KeyMapping Click in GUI (mouseClicked)

Source: https://docs.neoforged.net/docs/1.20.4/misc/keymappings

Demonstrates how to check if a KeyMapping is active and matches a mouse click within a GUI element. It utilizes isActiveAndMatches with InputConstants.TYPE.MOUSE.getOrCreate to validate the mouse button input.

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

### Transform Configuration in ModelBuilder (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Shows how to access and configure model transforms (for display properties) using the `#transforms()` method in ModelBuilder. This returns a TransformVecBuilder for modifying rotation, scale, and translation.

```java
TransformVecBuilder transformBuilder = modelBuilder.transforms();
// Configure transforms using transformBuilder methods
```

--------------------------------

### Register Block Color Handler (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models

Registers a custom color handler for blocks on the physical client. This allows dynamic tinting of block textures based on state, position, or other factors. The handler receives the block state, level, position, and tint index, returning an RGB color value.

```java
@SubscribeEvent // on the mod event bus only on the physical client  
public static void registerBlockColorHandlers(RegisterColorHandlersEvent.Block event) {  
    // Parameters are the block's state, the level the block is in, the block's position, and the tint index.  
    // The level and position may be null.  
    event.register((state, level, pos, tintIndex) -> {  
            // Replace with your own calculation. See the BlockColors class for vanilla references.  
            // All vanilla uses assume alpha 255 (= 1f), but modded consumers may also account  
            // for alpha values specified here. Generally, if the tint index is -1,  
            // it means that no tinting should take place and a default value should be used instead.  
            return 0xFFFFFF;  
    });  
}

```

--------------------------------

### Query Item Stack Capability

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Attempts to retrieve a capability from an item stack. Returns the capability implementation if found, otherwise returns null.

```java
var object = stack.getCapability(CAP, context);
if (object != null) {
    // Use object
}

```

--------------------------------

### Register Block Entity Capability Provider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Registers a capability provider for a specific block entity type. This helper method simplifies the process of providing capabilities for block entities, taking the block entity instance and side as input.

```java
event.registerBlockEntity(  
    Capabilities.ItemHandler.BLOCK, // capability to register for  
    MY_BLOCK_ENTITY_TYPE, // block entity type to register for  
    (myBlockEntity, side) -> <return the IItemHandler for myBlockEntity and side>);

```

--------------------------------

### Create Custom Instantaneous MobEffect in Java

Source: https://docs.neoforged.net/docs/1.20.4/items/mobeffects

Illustrates creating a custom instantaneous MobEffect by extending the InstantenousMobEffect class. This is used for effects that apply their logic only once. Requires the LivingEntity and MobEffectCategory classes.

```java
public class MyMobEffect extends InstantenousMobEffect {
    public MyMobEffect(MobEffectCategory category, int color) {
        super(category, color);
    }

    @Override
    public void applyEffectTick(LivingEntity entity, int amplifier) {
        // Apply your effect logic here.
    }
}
```

--------------------------------

### Add Values to Data Maps (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/datamaps/structure

Demonstrates how to attach values to registry entries or tags within a data map JSON file. It shows how to specify individual items like 'minecraft:carrot' or tags like '#minecraft:logs' and associate them with specific data, such as 'amount' and 'chance'.

```json
{
    "values": {
        "minecraft:carrot": {
            "amount": 12,
            "chance": 1
        },
        "#minecraft:logs": {
            "amount": 1,
            "chance": 0.1
        }
    }
}
```

--------------------------------

### Encode/Decode Java Objects with JsonOps

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Demonstrates how to use Codecs with JsonOps to serialize Java objects into JsonElements and deserialize JsonElements back into Java objects. Supports both standard and compressed JSON formats.

```java
// Let exampleCodec represent a Codec<ExampleJavaObject>  
// Let exampleObject be a ExampleJavaObject  
// Let exampleJson be a JsonElement  
  
// Encode Java object to regular JsonElement  
exampleCodec.encodeStart(JsonOps.INSTANCE, exampleObject);  
  
// Encode Java object to compressed JsonElement  
exampleCodec.encodeStart(JsonOps.COMPRESSED, exampleObject);  
  
// Decode JsonElement into Java object  
// Assume JsonElement was parsed normally  
exampleCodec.parse(JsonOps.INSTANCE, exampleJson);
```

--------------------------------

### Register Entity Capability Provider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Registers a capability provider for a specific entity type. This method allows you to define a function that returns a capability, such as an IItemHandler, for a given entity instance.

```java
event.registerEntity(  
    Capabilities.ItemHandler.ENTITY, // capability to register for  
    MY_ENTITY_TYPE, // entity type to register for  
    (myEntity, context) -> <return the IItemHandler for myEntity>);

```

--------------------------------

### Register Custom MobEffect in Java

Source: https://docs.neoforged.net/docs/1.20.4/items/mobeffects

Shows how to register a custom MobEffect using NeoForged's DeferredRegister. This involves providing a unique registry name and an instance of the custom MobEffect, along with its category and particle color. Requires a DeferredRegister instance and MobEffectCategory.

```java
//MOB_EFFECTS is a DeferredRegister<MobEffect>
public static final Holder<MyMobEffect> MY_MOB_EFFECT = MOB_EFFECTS.register("my_mob_effect", () -> new MyMobEffect(
        //Can be either BENEFICIAL, NEUTRAL or HARMFUL. Used to determine the potion tooltip color of this effect.
        MobEffectCategory.BENEFICIAL,
        //The color of the effect particles.
        0xffffff
));
```

--------------------------------

### Implement Custom Dynamic Baked Model - Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

This Java code defines a custom dynamic baked model. It utilizes `BakedModelWrapper` for convenience and stores essential attributes like ambient occlusion, GUI 3D rendering capability, block light usage, particle sprite, and item overrides.

```java
// BakedModelWrapper can be used as well to return default values for most methods, allowing you to only override what actually needs to be overridden.
public class MyDynamicModel implements IDynamicBakedModel {
    // Material of the missing texture. Its sprite can be used as a fallback when needed.
    private static final Material MISSING_TEXTURE =
            new Material(TextureAtlas.LOCATION_BLOCKS, MissingTextureAtlasSprite.getLocation());

    // Attributes for use in the methods below. Optional, the methods may also use constant values if applicable.
    private final boolean useAmbientOcclusion;
    private final boolean isGui3d;
    private final boolean usesBlockLight;
    private final TextureAtlasSprite particle;
    private final ItemOverrides overrides;

    // The constructor does not require any parameters other than the ones for instantiating the final fields.

```

--------------------------------

### Configure Parrot Imitation Sounds with neoforge:parrot_imitations Data Map

Source: https://docs.neoforged.net/docs/1.20.4/datamaps/neo_maps

The `neoforge:parrot_imitations` data map allows configuring the sounds parrots produce when imitating mobs, replacing `Parrot#MOB_SOUND_MAP`. This data map is located at `neoforge/data_maps/entity_type/parrot_imitations.json`. It uses a 'sound' string to specify the imitation sound ID. This provides a way to customize parrot behaviors.

```json
{
    "values": {
        "minecraft:allay": {
            "sound": "minecraft:ambient.cave"
        }
    }
}
```

--------------------------------

### Register Event Handler with IEventBus#addListener

Source: https://docs.neoforged.net/docs/1.20.4/concepts/events

Demonstrates registering an event handler method directly using `IEventBus#addListener`. This method requires a static reference to the handler method. The handler receives an event object and performs an action, in this case, healing an entity on the server side.

```java
@Mod("yourmodid")
public class YourMod {
    public YourMod(IEventBus modBus) {
        NeoForge.EVENT_BUS.addListener(YourMod::onLivingJump);
    }

    // Heals an entity by half a heart every time they jump.
    private static void onLivingJump(LivingJumpEvent event) {
        Entity entity = event.getEntity();
        // Only heal on the server side
        if (!entity.level().isClientSide()) {
            entity.heal(1);
        }
    }
}
```

--------------------------------

### Render Type Configuration in ModelBuilder (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Shows how to set the render type for a model using the `#renderType` method in ModelBuilder. This is crucial for defining how the model is rendered in-game, with support for both ResourceLocation and String inputs.

```java
modelBuilder.renderType(new ResourceLocation("minecraft", "cutout"));
modelBuilder.renderType("translucent");
```

--------------------------------

### Manually Profiling Custom Code Sections (Java)

Source: https://docs.neoforged.net/docs/1.20.4/misc/debugprofiler

Demonstrates how to manually profile specific code sections using ProfilerFiller in Java. This is useful for tracking the performance of custom modded code that isn't automatically captured by the profiler.

```java
ProfilerFiller#push(yourSectionName : String);
//The code you want to profile
ProfilerFiller#pop();

```

--------------------------------

### Register Payload with Handlers

Source: https://docs.neoforged.net/docs/1.20.4/networking/payload

Registers the 'MyData' payload with the IPayloadRegistrar. It specifies how to create the payload from a buffer and registers handlers for client and server-side reception. The 'play' method is used for payloads sent during the play phase.

```java
@SubscribeEvent // on the mod event bus  
public static void register(final RegisterPayloadHandlerEvent event) {  
    final IPayloadRegistrar registrar = event.registrar("mymod");  
    registrar.play(MyData.ID, MyData::new, handler -> handler  
            .client(ClientPayloadHandler.getInstance()::handleData)  
            .server(ServerPayloadHandler.getInstance()::handleData));  
}  

```

--------------------------------

### IntrinsicHolderTagsProvider Constructor - Java

Source: https://docs.neoforged.net/docs/1.20.4/datagen/tags

Demonstrates the constructor for AttributeTagsProvider, a subtype of IntrinsicHolderTagsProvider. It initializes the provider with necessary parameters and defines a function to obtain the ResourceKey for an attribute.

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

### BakedModel Overridable Methods

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/bakedmodel

This section details the methods within the BakedModel class that can be overridden to customize rendering behavior. It includes methods for ambient occlusion, GUI rendering, lighting, custom renderers, item overrides, model data, particle icons, and render types for both blocks and items.

```APIDOC
## BakedModel Overridable Methods

### Description
Methods in `BakedModel` that can be overridden to customize rendering behavior.

### Methods

- **`TriState useAmbientOcclusion(BlockState state, RenderType renderType, ModelData modelData)`**
  - Determines whether to use ambient occlusion. Returns a `TriState` to force-enable or disable AO.
  - Overloads `boolean useAmbientOcclusion()` and `boolean useAmbientOcclusion(BlockState state)` are deprecated.

- **`boolean isGui3d()`**
  - Determines if the model should render as 3D or flat in GUI slots.

- **`boolean usesBlockLight()`**
  - Determines whether to use 3D lighting (`true`) or flat lighting (`false`).

- **`boolean isCustomRenderer()`**
  - If true, skips normal rendering and uses `BlockEntityWithoutLevelRenderer`. If false, uses the default renderer.

- **`ItemOverrides getOverrides()`**
  - Returns the `ItemOverrides` associated with this model. Relevant for item models.

- **`ModelData getModelData(BlockAndTintGetter getter, BlockPos pos, BlockState state, ModelData modelData)`**
  - Returns the model data to use for the model. Can be used for blocks without block entities that require model data.

- **`TextureAtlasSprite getParticleIcon(ModelData modelData)`**
  - Returns the particle sprite for the model. Can use model data for different sprites. NeoForge addition.

- **`ChunkRenderTypeSet getRenderTypes(BlockState state, RandomSource random, ModelData modelData)`**
  - Returns a `ChunkRenderTypeSet` for block model rendering. Falls back to model JSON by default.

- **`List<RenderType> getRenderTypes(ItemStack stack, boolean isItemPropertyActive)`**
  - Returns a `List<RenderType>` for item model rendering. Falls back to model-bound render type lookup.

```

--------------------------------

### Define Sound Events in sounds.json

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/sounds

This JSON structure defines sound events and their associated sound files. Each key is a sound event ID, mapping to a JSON object that specifies the sounds to be played. Properties like 'sounds', 'replace', and 'subtitle' control playback behavior and display.

```json
{
  "my_sound": {
    "sounds": [
      {
        "name": "examplemod:sound_1",
        "type": "sound",
        "volume": 0.8,
        "pitch": 1.1,
        "weight": 3,
        "stream": true,
        "attenuation_distance": 8,
        "preload": true
      },
      "examplemod:sound_2"
    ]
  },
  "my_fixed_sound": {
    "replace": true,
    "subtitle": "examplemod.my_fixed_sound",
    "sounds": [
      "examplemod:sound_1",
      "examplemod:sound_2"
    ]
  }
}
```

--------------------------------

### Calling SimpleCriterionTrigger#trigger Method (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/advancements

Illustrates how to call the `#trigger` method of a SimpleCriterionTrigger subclass when the action being checked is performed. This ensures that all attached listeners are notified.

```java
// In some piece of code where the action is being performed
// Again, EXAMPLE_TRIGGER is a supplier for the registered instance of the custom criteria trigger
public void performExampleAction(ServerPlayer player, ItemStack stack) {
  // Run code to perform action
  EXAMPLE_TRIGGER.get().trigger(player, stack);
}
```

--------------------------------

### Client-Side Payload Handler Implementation

Source: https://docs.neoforged.net/docs/1.20.4/networking/payload

Implements the client-side handler for the 'MyData' payload. The 'handleData' method receives the payload and a PlayPayloadContext. It demonstrates processing data on the network thread and submitting tasks to the main thread using the workHandler.

```java
public class ClientPayloadHandler {  
      
    private static final ClientPayloadHandler INSTANCE = new ClientPayloadHandler();  
      
    public static ClientPayloadHandler getInstance() {  
        return INSTANCE;  
    }  
      
    public void handleData(final MyData data, final PlayPayloadContext context) {  
        // Do something with the data, on the network thread  
        blah(data.name());  
          
        // Do something with the data, on the main thread  
        context.workHandler().submitAsync(() -> {  
            blah(data.age());  
        })  
        .exceptionally(e -> {  
            // Handle exception  
            context.packetHandler().disconnect(Component.translatable("my_mod.networking.failed", e.getMessage()));  
            return null;  
        });  
    }  
}  

```

--------------------------------

### Separate Transforms Model Configuration (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Allows switching between different models based on the game's perspective (e.g., first-person, third-person, dropped item). It defines a base model and provides perspective-specific overrides, often by referencing other models.

```json
{
  "loader": "neoforge:separate_transforms",
  "base": {
    "parent": "minecraft:block/cobblestone"
  },
  "perspectives": {
    "ground": {
      "parent": "minecraft:block/stone"
    }
  }
}
```

--------------------------------

### Register Event Handler with @Mod.EventBusSubscriber

Source: https://docs.neoforged.net/docs/1.20.4/concepts/events

Demonstrates the most streamlined way to register event handlers using the `@Mod.EventBusSubscriber` annotation. This annotation automatically registers all static `@SubscribeEvent`-annotated methods within the class, eliminating the need for manual registration in the mod constructor. Specifying `modid` is recommended for easier debugging.

```java
@Mod.EventBusSubscriber(modid = "yourmodid")
public class EventHandler {
    @SubscribeEvent
    public static void onLivingJump(LivingJumpEvent event) {
        Entity entity = event.getEntity();
        if (!entity.level().isClientSide()) {
            entity.heal(1);
        }
    }
}
```

--------------------------------

### Encode/Decode Java Objects with NbtOps

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Illustrates the use of Codecs with NbtOps for serializing Java objects into NBT Tag instances and deserializing Tag instances back into Java objects. This is essential for handling Minecraft's NBT data format.

```java
// Let exampleCodec represent a Codec<ExampleJavaObject>  
// Let exampleObject be a ExampleJavaObject  
// Let exampleNbt be a Tag  
  
// Encode Java object to Tag  
exampleCodec.encodeStart(JsonOps.INSTANCE, exampleObject);  
  
// Decode Tag into Java object  
exampleCodec.parse(JsonOps.INSTANCE, exampleNbt);
```

--------------------------------

### Create Pair Codec in Java

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

Generates a codec for pairs of objects using Codec#pair. It decodes by first decoding the left object, then the right object from the remaining data. The codecs must either describe the encoded object after decoding or be augmented into a MapCodec.

```java
public static final Codec<Pair<Integer, String>> PAIR_CODEC = Codec.pair(
  Codec.INT.fieldOf("left").codec(),
  Codec.STRING.fieldOf("right").codec()
);

```

--------------------------------

### Using TagAppender for Tag Manipulation in NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/datagen/tags

This Java code illustrates how to use the `TagAppender` in NeoForged to add, optionally add, add tags, optionally add tags, replace, or remove elements from a tag. It showcases common operations for managing tag contents within a `TagProvider`.

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

### Transform Codecs with xmap (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/codecs

The xmap function transforms a codec into an equivalent or partially equivalent representation. It takes two functions: one to transform the current type to the new type, and one to transform the new type back to the current type. This is useful for creating codecs for equivalent classes.

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

### LootTable Builder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

Constructs a loot table by defining pools and functions. Pools are applied sequentially, and functions modify the resulting items.

```java
LootTable lootTable = LootTable.lootTable()
    .withPool(pool1)
    .withPool(pool2)
    .apply(function1)
    .build();
```

--------------------------------

### Registering LootTableProvider with DataGenerator (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

This snippet shows how to register a custom LootTableProvider with the DataGenerator using the @SubscribeEvent annotation. It specifies that the provider should run during server data generation and configures it with necessary sub-providers.

```java
@SubscribeEvent // on the mod event bus  
public static void gatherData(GatherDataEvent event) {  
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

### Custom Loader Assignment in ModelBuilder (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Illustrates how to assign a custom model loader using the `#customLoader` method in ModelBuilder. This allows for advanced model loading logic defined by a BiFunction factory.

```java
modelBuilder.customLoader(MyCustomLoader::new);
// Or with a specific BiFunction implementation
```

--------------------------------

### Root Transform with Matrix (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models

Applies a root-level transformation to a model using a 3x4 matrix. This matrix defines translation, rotation, and scale, applied in a specific order. It's a concise way to define complex transformations.

```json
{
  "transform": {
    "matrix": [
      [0, 0, 0, 0],
      [0, 0, 0, 0],
      [0, 0, 0, 0]
    ]
  }
}
```

--------------------------------

### Implement Custom ParticleType

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/particles

Creates a custom ParticleType that utilizes the custom ParticleOptions. This involves extending `ParticleType` and providing the custom deserializer. It also includes defining a Codec for serialization, which is the modern approach recommended by Mojang.

```java
public class MyParticleType extends ParticleType<MyParticleOptions> {
    // The boolean parameter again determines whether to limit particles at lower particle settings.
    // See implementation of the MyParticleTypes class near the top of the article for more information.
    public MyParticleType(boolean overrideLimiter) {
        // Pass the deserializer to super.
        super(overrideLimiter, MyParticleOptions.DESERIALIZER);
    }
      
    // Mojang is moving towards codecs for particle types, so expect the old deserializer approach to vanish soon.
    // We define our codec and then return it in the codec() method. Since our example uses no parameters
    // for serialization, we use an empty unit codec. Refer to the Codecs article for more information.
    public static final Codec<MyParticleOptions> CODEC = Codec.unit(new MyParticleOptions());
      
    @Override
    public Codec<MyParticleOptions> codec() {
        return CODEC;
    }
}

```

--------------------------------

### Register Custom Tool Tier with TierSortingRegistry

Source: https://docs.neoforged.net/docs/1.20.4/items/tools

Registers a custom tool tier (`COPPER_TIER`) with the `TierSortingRegistry`. This allows the game to correctly determine the mining hierarchy between custom and vanilla tiers. It specifies the tier, its internal name, and lists of lower and higher tiers.

```java
public static final Tier COPPER_TIER = new SimpleTier(...);  

static {
    TierSortingRegistry.registerTier(
            COPPER_TIER,
            //The name to use for internal resolution. May use the Minecraft namespace if appropriate.
            new ResourceLocation("minecraft", "copper"),
            //A list of tiers that are considered lower than the type being added. For example, stone is lower than copper.
            //We don't need to add wood and gold here because those are already lower than stone.
            List.of(Tiers.STONE),
            //A list of tiers that are considered higher than the type being added. For example, iron is higher than copper.
            //We don't need to add diamond and netherite here because those are already higher than iron.
            List.of(Tiers.IRON)
    );
}
```

--------------------------------

### Custom Criterion Trigger Instance Implementation (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/advancements

Implements a custom criterion trigger instance in Java, extending SimpleCriterionTrigger.SimpleInstance. This defines the conditions for a specific advancement criterion and includes a method to check if the player meets these conditions.

```java
public record ExampleTriggerInstance(Optional<ContextAwarePredicate> player, ItemPredicate item) implements SimpleCriterionTrigger.SimpleInstance {
  // extra methods here
}
```

--------------------------------

### Define Custom Data Structure

Source: https://docs.neoforged.net/docs/1.20.4/networking/payload

Defines a simple data structure 'MyData' with a name (String) and age (int). This record will be used as the payload.

```java
public record MyData(String name, int age) {}  

```

--------------------------------

### Register Block Capability Provider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Registers a capability provider for specific blocks. This method is called within the RegisterCapabilitiesEvent and allows you to define a function that returns an IItemHandler for given block instances.

```java
@SubscribeEvent // on the mod event bus  
public static void registerCapabilities(RegisterCapabilitiesEvent event) {  
    event.registerBlock(  
        Capabilities.ItemHandler.BLOCK, // capability to register for  
        (level, pos, state, be, side) -> <return the IItemHandler>,  
        // blocks to register for  
        MY_ITEM_HANDLER_BLOCK,  
        MY_OTHER_ITEM_HANDLER_BLOCK);
}

```

--------------------------------

### Register Blocks using RegisterEvent

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Registers multiple Block objects using the RegisterEvent. This method is called for each registry after mod constructors and before config loading. It requires the registry key and a lambda function to register individual objects.

```java
@SubscribeEvent // on the mod event bus
public static void register(RegisterEvent event) {
    event.register(
            // This is the registry key of the registry.
            // Get these from BuiltInRegistries for vanilla registries,
            // or from NeoForgeRegistries.Keys for NeoForge registries.
            BuiltInRegistries.BLOCKS,
            // Register your objects here.
            registry -> {
                registry.register(new ResourceLocation(MODID, "example_block_1"), new Block(...));
                registry.register(new ResourceLocation(MODID, "example_block_2"), new Block(...));
                registry.register(new ResourceLocation(MODID, "example_block_3"), new Block(...));
            }
    );
}

```

--------------------------------

### Target Fields with Access Transformers

Source: https://docs.neoforged.net/docs/1.20.4/advanced/accesstransformers

This snippet illustrates the syntax for targeting fields with Access Transformers. It requires the access modifier, the fully qualified class name, and the specific field name.

```plaintext
<access modifier> <fully qualified class name> <field name>

```

--------------------------------

### Query Entity Capability

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Attempts to retrieve a capability from an entity. Returns the capability implementation if found, otherwise returns null.

```java
var object = entity.getCapability(CAP, context);
if (object != null) {
    // Use object
}

```

--------------------------------

### Java Saved Data Implementation and Usage

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/saveddata

Demonstrates how to implement a custom `SavedData` class in Java for NeoForged, including methods for creating, loading, saving data, and marking it as dirty. It also shows how to attach this saved data to a level using `DimensionDataStorage#computeIfAbsent`.

```java
public class ExampleSavedData extends SavedData {  
  
    // Create new instance of saved data  
    public static ExampleSavedData create() {
        return new ExampleSavedData();  
    }

    // Load existing instance of saved data  
    public static ExampleSavedData load(CompoundTag tag) {
        ExampleSavedData data = ExampleSavedData.create();  
        // Load saved data  
        return data;
    }

    @Override  
    public CompoundTag save(CompoundTag tag) {
        // Write data to tag  
        return tag;
    }

    public void foo() {
        // Change data in saved data  
        // Call set dirty if data changes  
        this.setDirty();  
    }
}

// In some method within the class  
netherDataStorage.computeIfAbsent(new Factory<>(ExampleSavedData::create, ExampleSavedData::load), "example");
```

--------------------------------

### Set BlockState Property Value (Java)

Source: https://docs.neoforged.net/docs/1.20.4/blocks/states

Illustrates how to create a new BlockState with a modified property value. Since BlockStates are immutable, this method returns a new BlockState instance with the updated property.

```java
endPortalFrameBlockState = endPortalFrameBlockState.setValue(EndPortalFrameBlock.FACING, Direction.SOUTH);

```

--------------------------------

### Play Sound from Entity

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/sounds

Plays a `SoundEvent` associated with an entity's position. This method forwards the call to `Level.playSound`, using `SoundSource.ENTITY` and the entity's location.

```java
void playSound(SoundEvent soundEvent, float volume, float pitch)
```

--------------------------------

### Boolean Operator Conditions (Not/And/Or)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/conditional

Shows how to use boolean logic operators like 'neoforge:not', 'neoforge:and', and 'neoforge:or'. The 'neoforge:not' condition inverts the result of its nested condition, while 'neoforge:and' and 'neoforge:or' combine multiple conditions.

```json
{
  // Inverts the result of the stored condition
  "type": "neoforge:not",
  "value": {
    // A condition
  }
}
```

```json
{
  // ANDs the stored conditions together (or ORs for 'neoforge:or')
  "type": "neoforge:and",
  "values": [
    {
      // First condition
    },
    {
      // Second condition to be ANDed (or ORed for 'neoforge:or')
    }
  ]
}
```

--------------------------------

### Check for NBT Tag Existence in Java

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/nbt

Illustrates methods for verifying the presence of a tag within a CompoundTag, both generally and by specific type. This is crucial for robust data handling and preventing runtime errors.

```java
boolean hasColor = tag.contains("Color");
boolean hasColorMoreExplicitly = tag.contains("Color", Tag.TAG_INT);

```

--------------------------------

### LootItemCondition Builder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

Specifies requirements for loot operations. By default, all conditions must be met. Options include using 'OR' logic or inverting the condition's output.

```java
LootItemCondition condition = LootItemCondition.lootItemCondition(conditionType)
    .or(condition2)
    .invert()
    .build();
```

--------------------------------

### Register Event Handler with @SubscribeEvent (Instance)

Source: https://docs.neoforged.net/docs/1.20.4/concepts/events

Shows how to register event handlers using the `@SubscribeEvent` annotation on instance methods. An instance of the handler class is passed to the event bus. The handler checks if the game is running on the server side before applying healing to the entity.

```java
public class EventHandler {
    @SubscribeEvent
    public void onLivingJump(LivingJumpEvent event) {
        Entity entity = event.getEntity();
        if (!entity.level().isClientSide()) {
            entity.heal(1);
        }
    }
}

@Mod("yourmodid")
public class YourMod {
    public YourMod(IEventBus modBus) {
        NeoForge.EVENT_BUS.addListener(new EventHandler());
    }
}
```

--------------------------------

### Language File Structure for Translations

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/i18n

Defines the structure of JSON language files used for in-game text translations. These files map translation keys to their corresponding text in a specific language.

```json
{
  "translation.key.1": "Translation 1",
  "translation.key.2": "Translation 2"
}
```

--------------------------------

### Read NBT Data from CompoundTag in Java

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/nbt

Shows how to retrieve integer, string, and double values from an existing CompoundTag. It also explains the default return values for absent numeric and string tags, and the exception behavior for complex types.

```java
int color = tag.getInt("Color");
String level = tag.getString("Level");
double d = tag.getDouble("IAmRunningOutOfIdeasForNamesHere");

```

--------------------------------

### Synchronize BlockEntity Data on Chunk Load (Java)

Source: https://docs.neoforged.net/docs/1.20.4/blockentities

This Java code illustrates synchronizing BlockEntity data to the client upon chunk load by overriding `getUpdateTag` to collect data and `handleUpdateTag` to process it. It warns against synchronizing excessive data to prevent network congestion.

```java
  
// Override BlockEntity#getUpdateTag() to collect data
// Override IForgeBlockEntity#handleUpdateTag(CompoundTag tag) to process data
```

--------------------------------

### Check Logical Client Side with Level#isClientSide()

Source: https://docs.neoforged.net/docs/1.20.4/concepts/sides

The Level#isClientSide() method is a boolean check to determine the logical side a Level object belongs to. It returns true if the level is running on the logical client and false if on the logical server. This is the default and recommended method for checking sides when a Level object is available, crucial for preventing desynchronization by ensuring game logic runs only on the appropriate logical side.

```java
boolean isClient = level.isClientSide();
```

--------------------------------

### LootPoolEntryContainer Builder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

Defines operations for generating items within a loot pool. Entries can execute sequentially or in parallel, with options for default actions on failure.

```java
LootPoolEntryContainer entry = LootPoolEntryContainer.lootPoolEntryContainer(entryType)
    .append(entry1)
    .then(entry2)
    .otherwise(defaultEntry)
    .build();
```

--------------------------------

### Implement Ticking BlockEntity in Java

Source: https://docs.neoforged.net/docs/1.20.4/blockentities

This snippet demonstrates how to create a ticking BlockEntity by overriding the `getTicker` method in a `Block` subclass. It includes a helper method for creating the ticker and a static tick method for the `BlockEntity`. This is useful for processes like smelting progress tracking.

```java
  
// Inside some Block subclass  
  
// We use a second method here due to generic conversions  
// If extending `BaseEntityBlock`, this method is also available there as a protected static method  
private static <E extends BlockEntity, A extends BlockEntity> @Nullable BlockEntityTicker<A> createTickerHelper(  
    BlockEntityType<A> type, BlockEntityType<E> checkedType, BlockEntityTicker<? super E> ticker  
) {  
    return checkedType == type ? (BlockEntityTicker<A>) ticker : null;  
}  
  
@Nullable  
@Override  
public <T extends BlockEntity> BlockEntityTicker<T> getTicker(Level level, BlockState state, BlockEntityType<T> type) {  
    // You can return different tickers here, depending on whatever factors you want. A common use case would be  
    // to return different tickers on the client or server, only tick one side to begin with,  
    // or only return a ticker for some blockstates (e.g. when using a "my machine is working" blockstate property).  
    return createTickerHelper(type, MY_BLOCK_ENTITY.get(), MyBlockEntity::tick);  
}  
  
// Inside MyBlockEntity  
public static void tick(Level level, BlockPos pos, BlockState state, MyBlockEntity blockEntity) {  
  // Do stuff  
}
```

--------------------------------

### Saving and Loading Block Entity Data with NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/blockentities

Illustrates the methods to override in a BlockEntity subclass for data persistence. `saveAdditional` is used for writing data to a tag when the chunk is saved, and `load` is used for reading data when the chunk is loaded. It emphasizes calling super methods and the importance of `setChanged()`.

```java
BlockEntity#saveAdditional(CompoundTag tag)  

BlockEntity#load(CompoundTag tag)
```

--------------------------------

### Implement Custom Geometry Class - Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

This Java code defines a custom geometry class. The `bake` method handles the model baking process, returning a `MyDynamicModel`. The `resolveParents` method is crucial for handling nested models or reusing vanilla loaders.

```java
public class MyGeometry implements IUnbakedGeometry<MyGeometry> {
    // The constructor may have any parameters you need, and store them in fields for further usage below.
    // If the constructor has parameters, the constructor call in MyGeometryLoader#read must match them.
    public MyGeometry() {}

    // Method responsible for model baking, returning our dynamic model. Parameters in this method are:
    // - The geometry baking context. Contains many properties that we will pass into the model, e.g. light and ao values.
    // - The model baker. Can be used for baking sub-models.
    // - The sprite getter. Maps materials (= texture variables) to TextureAtlasSprites. Materials can be obtained from the context.
    //   For example, to get a model's particle texture, call spriteGetter.apply(context.getMaterial("particle"));
    // - The model state. This holds the properties from the blockstate file, e.g. rotations and the uvlock boolean.
    // - The item overrides. This is the code representation of an "overrides" block in an item model.
    // - The resource location of the model.
    @Override
    public BakedModel bake(IGeometryBakingContext context, ModelBaker baker, Function<Material, TextureAtlasSprite> spriteGetter, ModelState modelState, ItemOverrides overrides, ResourceLocation modelLocation) {
        // See info on the parameters below.
        return new MyDynamicModel(context.useAmbientOcclusion(), context.isGui3d(), context.useBlockLight(),
                spriteGetter.apply(context.getMaterial("particle")), overrides);
    }

    // Method responsible for correctly resolving parent properties. Required if this model loads any nested models or reuses the vanilla loader on itself (see below).
    @Override
    public void resolveParents(Function<ResourceLocation, UnbakedModel> modelGetter, IGeometryBakingContext context) {
        base.resolveParents(modelGetter);
    }
}
```

--------------------------------

### Register Event Handler with @SubscribeEvent (Static Class)

Source: https://docs.neoforged.net/docs/1.20.4/concepts/events

Illustrates registering static event handlers using the `@SubscribeEvent` annotation. The class itself is passed to the event bus, allowing NeoForge to discover and register all static handlers within that class. This approach is useful for organizing event logic.

```java
public class EventHandler {
	@SubscribeEvent
    public static void onLivingJump(LivingJumpEvent event) {
        Entity entity = event.getEntity();
        if (!entity.level().isClientSide()) {
            entity.heal(1);
        }
    }
}

@Mod("yourmodid")
public class YourMod {
    public YourMod(IEventBus modBus) {
        NeoForge.EVENT_BUS.addListener(EventHandler.class);
    }
}
```

--------------------------------

### Item Layer Model Configuration (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Defines an item model with an unlimited number of layers and per-layer render types. It specifies texture paths for each layer and maps render types to specific layer indices. This model extends the default item/generated model.

```json
{
  "loader": "neoforge:item_layers",
  "textures": {
    "layer0": "...",
    "layer1": "...",
    "layer2": "...",
    "layer3": "...",
    "layer4": "...",
    "layer5": "...",
  },
  "render_types": {
    "minecraft:cutout": [0, 2, 4],
    "minecraft:cutout_mipped": [1, 3],
    "minecraft:translucent": [5]
  }
}
```

--------------------------------

### Add Items to Existing Creative Tabs (Java)

Source: https://docs.neoforged.net/docs/1.20.4/items

This code snippet demonstrates how to add custom items and blocks to existing creative tabs in Minecraft using NeoForge. It utilizes the `BuildCreativeModeTabContentsEvent` to accept `ItemLike` objects into specified tabs like `CreativeModeTabs.INGREDIENTS`.

```java
//MyItemsClass.MY_ITEM is a Supplier<? extends Item>, MyBlocksClass.MY_BLOCK is a Supplier<? extends Block>  
@SubscribeEvent // on the mod event bus  
public static void buildContents(BuildCreativeModeTabContentsEvent event) {  
    // Is this the tab we want to add to?  
    if (event.getTabKey() == CreativeModeTabs.INGREDIENTS) {  
        event.accept(MyItemsClass.MY_ITEM);  
        // Accepts an ItemLike. This assumes that MY_BLOCK has a corresponding item.  
        event.accept(MyBlocksClass.MY_BLOCK);  
    }  
}

```

--------------------------------

### Element Addition in ModelBuilder (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Demonstrates the process of adding a new element to a model using the `#element()` method in ModelBuilder. This method returns an ElementBuilder, allowing for detailed configuration of the model's geometry.

```java
ElementBuilder element = modelBuilder.element();
// Further configure the element using element builder methods
```

--------------------------------

### Custom Loader Builder for Datagen (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Defines a builder class for generating custom block models during data generation. It extends `CustomLoaderBuilder` and allows for defining custom fields and serializing them into JSON format for model files.

```java
// This assumes a block model. Use ItemModelBuilder as the generic parameter instead
// if you're making a custom item model.
public class MyLoaderBuilder extends CustomLoaderBuilder<BlockModelBuilder> {
    public MyLoaderBuilder(BlockModelBuilder parent, ExistingFileHelper existingFileHelper) {
        super(
                // Your model loader's id.
                MyGeometryLoader.ID,
                // The parent builder we use. This is always the first constructor parameter.
                parent,
                // The existing file helper we use. This is always the second constructor parameter.
                existingFileHelper,
                // Whether the loader allows inline vanilla elements as a fallback if the loader is absent.
                false
        );
    }

    // Add fields and setters for the fields here. The fields can then be used below.

    // Serialize the model to JSON.
    @Override
    public JsonObject toJson(JsonObject json) {
        // Add your fields to the given JsonObject.
        // Then call super, which adds the loader property and some other things.
        return super.toJson(json);
    }
}
```

--------------------------------

### Create Block Capability without Context

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Declares a BlockCapability that does not require any additional context for querying. This is useful for capabilities that have a universally applicable behavior.

```java
public static final BlockCapability<IItemHandler, Void> ITEM_HANDLER_NO_CONTEXT =  
    BlockCapability.createVoid(  
        // Provide a name to uniquely identify the capability.  
        new ResourceLocation("mymod", "item_handler_no_context"),  
        // Provide the queried type. Here, we want to look up `IItemHandler` instances.  
        IItemHandler.class);

```

--------------------------------

### Dynamic Fluid Container Model JSON - NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Configures a dynamic fluid container model in JSON for NeoForged, used for items like buckets to display fluid. It requires fluid and texture definitions and supports optional properties for customization.

```json
{
  "loader": "neoforge:fluid_container",
  "fluid": "minecraft:water",
  "textures": {
    "base": "examplemod:item/custom_container",
    "fluid": "examplemod:item/custom_container_fluid"
  },
  "flip_gas": true,
  "cover_is_mask": false,
  "apply_fluid_luminosity": false
}
```

```json
{
  "loader": "neoforge:fluid_container",
  "parent": "neoforge:item/bucket",
  "fluid": "minecraft:water"
}
```

--------------------------------

### Add MobEffectInstance to Entity in Java

Source: https://docs.neoforged.net/docs/1.20.4/items/mobeffects

Shows how to add a created MobEffectInstance to a LivingEntity. MobEffectInstances are mutable, and a new instance should be created if a copy is needed.

```java
MobEffectInstance instance = new MobEffectInstance(...);
entity.addEffect(instance);

```

--------------------------------

### Create Sided Block Capability

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

A convenience method to create a BlockCapability for sided access, simplifying the declaration for common block-side interactions. It automatically handles the Direction context.

```java
public static final BlockCapability<IItemHandler, @Nullable Direction> ITEM_HANDLER_BLOCK =  
    BlockCapability.createSided(  
        // Provide a name to uniquely identify the capability.  
        new ResourceLocation("mymod", "item_handler"),  
        // Provide the queried type. Here, we want to look up `IItemHandler` instances.  
        IItemHandler.class);

```

--------------------------------

### Recipe Result with NBT (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes

Specifies the result of a recipe in JSON format, including the item, count, and optional NBT data. This allows for more complex item outputs beyond simple item names and quantities. The 'nbt' tag can also be a string for SNBT.

```json
{
  "item": "examplemod:example_item",
  "count": 4,
  "nbt": {
      
  }
}
```

--------------------------------

### Boolean Conditions (True/False)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/conditional

Illustrates the use of 'neoforge:true' and 'neoforge:false' conditions. These are simple boolean conditions that return a fixed true or false value, respectively, without requiring any additional data.

```json
{
  // Will always return true (or false for 'neoforge:false')
  "type": "neoforge:true"
}
```

--------------------------------

### Texture Metadata Structure (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/textures

Defines the structure for texture metadata files (.mcmeta) used to control properties like blur, clamp, GUI scaling, and animation for textures in NeoForged. The 'scaling' property can be 'stretch', 'tile', or 'nine_slice', each with specific configuration options. The 'animation' object is used for animated textures.

```json
{
  "blur": true,
  "clamp": true,
  "gui": {
    "scaling": "stretch"
  },
  "gui": {
    "scaling": {
      "tile": {
        "width": 16,
        "height": 16
      }
    }
  },
  "gui": {
    "scaling": {
      "nine_slice": {
        "width": 16,
        "height": 16,
        "border": {
          "left": 0,
          "top": 0,
          "right": 0,
          "bottom": 0
        }
      }
    }
  },
  "animation": {}
}
```

--------------------------------

### Target Classes with Access Transformers

Source: https://docs.neoforged.net/docs/1.20.4/advanced/accesstransformers

This is the syntax for targeting classes using Access Transformers. It involves specifying the desired access modifier followed by the fully qualified class name. Inner classes are denoted using a '$' separator.

```plaintext
<access modifier> <fully qualified class name>

```

--------------------------------

### Define Custom Block Tag in Java

Source: https://docs.neoforged.net/docs/1.20.4/items/tools

Defines a custom block tag using `TagKey.create` in Java. This tag can then be used to group blocks that require a specific tool (e.g., copper tools) for mining. It requires a `ResourceLocation` and the tag name.

```java
public static final TagKey<Block> NEEDS_COPPER_TOOL = TagKey.create(BuiltInRegistries.BLOCK.key(), new ResourceLocation(MOD_ID, "needs_copper_tool"));  
```

--------------------------------

### Query Block Capability with Position

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Queries a capability on a level at a specific position. This method is used when the block entity or state is not readily available.

```java
var object = level.getCapability(CAP, pos, context);
if (object != null) {
    // Use object
}

```

--------------------------------

### Register Game Tests with @GameTestHolder

Source: https://docs.neoforged.net/docs/1.20.4/misc/gametest

Illustrates registering all test methods within a class using the @GameTestHolder annotation. The 'value' parameter must be set to the mod ID for the tests to be recognized under default configurations.

```java
@GameTestHolder(MODID)
public class ExampleGameTests {
  // ...
}
```

--------------------------------

### Configure Composter Values with neoforge:compostables Data Map

Source: https://docs.neoforged.net/docs/1.20.4/datamaps/neo_maps

The `neoforge:compostables` data map allows configuration of composter values, replacing the ignored `ComposterBlock#COMPOSTABLES`. It is located at `neoforge/data_maps/item/compostables.json`. The structure includes a 'chance' float (0 to 1) for item compostability. This enables dynamic configuration of how items affect composter levels.

```json
{
    "values": {
        "minecraft:acacia_log": {
            "chance": 0.5
        }
    }
}
```

--------------------------------

### Animated Texture Metadata (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/textures

Specifies the structure for the 'animation' object within a texture metadata file (.mcmeta). This allows for custom frame order, frame timing, interpolation between frames, and defining the width and height of individual animation stages. If omitted, frames play top to bottom with a default frametime of 1 and no interpolation.

```json
{
  "animation": {
    "frames": [1, 0],
    "frametime": 5,
    "interpolate": true,
    "width": 12,
    "height": 12
  }
}
```

--------------------------------

### Register Custom Tool Items in Java

Source: https://docs.neoforged.net/docs/1.20.4/items/tools

Registers custom tool items (Sword, Axe, Pickaxe, Shovel, Hoe) using a previously defined custom tier (COPPER_TIER). It demonstrates how to set item-specific properties like attack damage bonus and attack speed modifier, along with general item properties. Assumes 'ITEMS' is a DeferredRegister<Item>.

```java
//ITEMS is a DeferredRegister<Item>
public static final Supplier<SwordItem> COPPER_SWORD = ITEMS.register("copper_sword", () -> new SwordItem(
        // The tier to use.
        COPPER_TIER,
        // The type-specific attack damage bonus. 3 for swords, 1.5 for shovels, 1 for pickaxes, varying for axes and hoes.
        3,
        // The type-specific attack speed modifier. The player has a default attack speed of 4, so to get to the desired
        // value of 1.6f, we use -2.4f. -2.4f for swords, -3f for shovels, -2.8f for pickaxes, varying for axes and hoes.
        -2.4f,
        // The item properties. We don't need to set the durability here because TieredItem handles that for us.
        new Item.Properties()
));
public static final Supplier<AxeItem> COPPER_AXE = ITEMS.register("copper_axe", () -> new AxeItem(...));
public static final Supplier<PickaxeItem> COPPER_PICKAXE = ITEMS.register("copper_pickaxe", () -> new PickaxeItem(...));
public static final Supplier<ShovelItem> COPPER_SHOVEL = ITEMS.register("copper_shovel", () -> new ShovelItem(...));
public static final Supplier<HoeItem> COPPER_HOE = ITEMS.register("copper_hoe", () -> new HoeItem(...));

```

--------------------------------

### ItemOverrides Class

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/bakedmodel

Explains the `ItemOverrides` class, which allows baked models to resolve to a new baked model based on an `ItemStack`'s state. It details the parameters for the `#resolve` method and the concept of `BakedOverride`s.

```APIDOC
## ItemOverrides Class

### Description
The `ItemOverrides` class enables baked models to dynamically resolve to a different baked model based on the state of an `ItemStack`.

### `#resolve` Method Parameters

- **`BakedModel originalModel`**: The initial model.
- **`ItemStack stack`**: The item stack being rendered.
- **`ClientLevel level`**: The level the model is rendered in (for querying, not mutation). May be null.
- **`LivingEntity entity`**: The entity the model is rendered on. May be null.
- **`int seed`**: A seed for randomization.

### `BakedOverride`

- `ItemOverrides` contains `BakedOverride` objects, which are in-code representations of a model's `overrides` block.
- These can be used by baked models to return different models based on their contents.
- A list of all `BakedOverride`s can be retrieved using `ItemOverrides#getOverrides()`.

```

--------------------------------

### Copy Entity Data on Player Death with PlayerEvent.Clone

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/attachments

This snippet demonstrates how to copy specific data fields from an original entity to a new entity upon player death using the PlayerEvent.Clone event. It checks if the event was triggered by death and if the original entity possesses the specified data before copying the relevant field. This method allows for granular control over data copying.

```java
NeoForge.EVENT_BUS.register(PlayerEvent.Clone.class, event -> {
    if (event.isWasDeath() && event.getOriginal().hasData(MY_DATA)) {
        event.getEntity().getData(MY_DATA).fieldToCopy = event.getOriginal().getData(MY_DATA).fieldToCopy;
    }
});
```

--------------------------------

### Define a Range Config Value with NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/misc/config

Illustrates defining a configuration value that must fall within a specified range using `ModConfigSpec.Builder#defineInRange`. This is suitable for numerical values like integers, doubles, or longs that have minimum and maximum bounds.

```java
// For some ModConfigSpec.Builder builder
// Assuming T is Comparable, e.g., Integer, Double, Long
ConfigValue<T> rangeValue = builder.comment("Value must be between min and max")
  .defineInRange("range_value_name", defaultValue, minValue, maxValue, type);
```

--------------------------------

### Implement Container Tick in Java

Source: https://docs.neoforged.net/docs/1.20.4/gui/screens

This snippet shows how to override the `containerTick` method in an AbstractContainerScreen subclass. This method is called when the screen is active and the player is looking at it, commonly used for ticking recipe books or other screen-specific logic.

```java
@Override
protected void containerTick() {
    super.containerTick();
  
    // Tick things here
}

```

--------------------------------

### Register ForgeAdvancementProvider in DataGenerator

Source: https://docs.neoforged.net/docs/1.20.4/datagen/advancements

This snippet shows how to register a ForgeAdvancementProvider to the DataGenerator using the GatherDataEvent. It specifies that the provider should run during server data generation and accepts a list of sub-providers for generating advancements.

```java
@SubscribeEvent // on the mod event bus  
public static void gatherData(GatherDataEvent event) {  
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

### LootPool Builder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

Defines a group of operations for loot generation. It includes entries, conditions, functions, and roll counts. Conditions determine if operations are performed, and functions modify the results.

```java
LootPool lootPool = LootPool.lootPool()
    .add(entry1)
    .add(entry2)
    .when(condition1)
    .apply(function1)
    .setRolls(rolls)
    .setBonusRolls(bonusRolls)
    .build();
```

--------------------------------

### Implementing generate Method in BlockLootSubProvider (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

This snippet shows the overridden generate method within a BlockLootSubProvider subclass. This is where the specific logic for generating loot tables for blocks is implemented.

```java
// In some BlockLootSubProvider subclass  
@Override  
public void generate() {  
  // Add loot tables here  
}
```

--------------------------------

### Empty Model JSON - NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Specifies an empty model in JSON for NeoForged, which results in no rendering. This loader is useful when a model is needed for registration but should not be visible.

```json
{
  "loader": "neoforge:empty"
}
```

--------------------------------

### Define an Enum Config Value with NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/misc/config

Illustrates defining a configuration value that must be an enum type using `ModConfigSpec.Builder#defineEnum`. This requires a getter to convert string or integer representations to the enum type and a collection of allowed enum values.

```java
// For some ModConfigSpec.Builder builder
// Assuming T is an Enum type
// Getter<String, T> getter = ...; // Or Getter<Integer, T>
// Collection<T> enumValues = ...;
ConfigValue<T> enumValue = builder.comment("An enum value")
  .defineEnum("enum_value_name", defaultValue, getter, enumValues, type);
```

--------------------------------

### Create DirectionProperty for Blockstates

Source: https://docs.neoforged.net/docs/1.20.4/blocks/states

Defines a direction property for blockstates, representing a specific direction (e.g., NORTH, SOUTH, EAST, WEST). It extends EnumProperty and is useful for blocks that have an orientation.

```java
DirectionProperty myDirectionProperty = DirectionProperty.create("my_property_name");

```

```java
DirectionProperty horizontalFacing = DirectionProperty.create("facing", Direction.Plane.HORIZONTAL);

```

--------------------------------

### Registering a Custom Tag Provider in NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/datagen/tags

This Java code snippet demonstrates how to register a custom tag provider for server-side data generation within a NeoForged mod. It uses the `@SubscribeEvent` annotation to hook into the `GatherDataEvent` and adds a `MyBlockTagsProvider` to the data generator.

```java
@SubscribeEvent // on the mod event bus  
public static void gatherData(GatherDataEvent event) {  
    event.getGenerator().addProvider(  
        // Tell generator to run only when server data are generating  
        event.includeServer(),  
        // Extends net.neoforged.neoforge.common.data.BlockTagsProvider  
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

### Configure Furnace Fuel Burn Times with neoforge:furnace_fuels Data Map

Source: https://docs.neoforged.net/docs/1.20.4/datamaps/neo_maps

The `neoforge:furnace_fuels` data map enables configuration of item burn times in ticks, serving as a replacement for `IItemExtension#getBurnTime`. It's found at `neoforge/data_maps/item/furnace_fuels.json`. The structure includes a 'burn_time' integer. This data map is recommended for static burn times, allowing user configuration.

```json
{
    "values": {
        "minecraft:anvil": {
            "burn_time": 40
        }
    }
}
```

--------------------------------

### Item Rendering Perspectives

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/bakedmodel

Details the 8 (plus a fallback) perspective types recognized by Minecraft's render engine for item rendering. These are specified in the model JSON's `display` block and correspond to enum values in `ItemDisplayContext`.

```APIDOC
## Item Rendering Perspectives

### Description
Minecraft recognizes 8 perspective types for item rendering, represented by the `ItemDisplayContext` enum and used in the `display` block of model JSON.

### Perspectives

| Enum Value              | JSON Key            | Usage                                                                  |
|-------------------------|---------------------|------------------------------------------------------------------------|
| `THIRD_PERSON_RIGHT_HAND` | `"thirdperson_righthand"` | Right hand in third person (F5 view, or on other players)              |
| `THIRD_PERSON_LEFT_HAND`  | `"thirdperson_lefthand"`  | Left hand in third person (F5 view, or on other players)               |
| `FIRST_PERSON_RIGHT_HAND` | `"firstperson_righthand"` | Right hand in first person                                             |
| `FIRST_PERSON_LEFT_HAND`  | `"firstperson_lefthand"`  | Left hand in first person                                              |
| `HEAD`                  | `"head"`            | Player's head armor slot (often via commands)                          |
| `GUI`                   | `"gui"`           | Inventories, player hotbar                                             |
| `GROUND`                | `"ground"`          | Dropped items (rotation handled by dropped item renderer)            |
| `FIXED`                 | `"fixed"`           | Item frames                                                            |
| `NONE`                  | `"none"`            | Fallback purposes in code; should not be used in JSON                  |

```

--------------------------------

### Register Loot Modifier Codec with NeoForge

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/glm

Defines the codec for a custom `ExampleModifier` to enable its serialization and deserialization from JSON. It utilizes `LootModifier.codecStart` and `RecordCodecBuilder` to define the structure, including properties like `prop1`, `prop2`, and `prop3`.

```java
// For some DeferredRegister<Codec<? extends IGlobalLootModifier>> REGISTRAR
public static final Supplier<Codec<ExampleModifier>> = REGISTRAR.register("example_codec", () ->
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

### Conditional Data Structure

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/conditional

Demonstrates the basic structure for conditionally loading JSON data. The 'neoforge:conditions' array specifies conditions that must all be met for the rest of the object to be loaded. This is a general structure applicable to various data types.

```json
{
  "neoforge:conditions": [
    // Condition 1
    {

    },
    // Condition 2
    {

    }
  ],
  
  // The rest of the object
  ...
}
```

--------------------------------

### Create IntegerProperty for Blockstates

Source: https://docs.neoforged.net/docs/1.20.4/blocks/states

Defines an integer property for blockstates. It supports a range of integer values, but not negative numbers. This is useful for properties like a block's level or count.

```java
IntegerProperty myIntegerProperty = IntegerProperty.create("my_property_name", 0, 15);

```

--------------------------------

### Attach DeferredRegister to Mod Event Bus

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

Attaches the DeferredRegister to the mod's event bus within the mod constructor. This ensures that the deferred registration process is initiated when the game loads and events are fired.

```java
//This is our mod constructor
public ExampleMod(IEventBus modBus) {
    ExampleBlocksClass.BLOCKS.register(modBus);
    //Other stuff here
}

```

--------------------------------

### Registering Block State Provider with Data Generator

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

This code snippet shows how to register a custom BlockStateProvider with the Neoforged data generator. It uses the @SubscribeEvent annotation to hook into the GatherDataEvent and provides the necessary PackOutput and ExistingFileHelper instances to the provider.

```java
@SubscribeEvent // on the mod event bus  
public static void gatherData(GatherDataEvent event) {  
    DataGenerator generator = event.getGenerator();  
    PackOutput output = generator.getPackOutput();  
    ExistingFileHelper existingFileHelper = event.getExistingFileHelper();  
  
    // other providers here  
    generator.addProvider(  
            event.includeClient(),  
            new MyBlockStateProvider(output, existingFileHelper)  
    );  
}
```

--------------------------------

### Advancement Requirements Definition (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/advancements

Defines the structure for advancement requirements in JSON, specifying criteria that must be met for an advancement to be unlocked. This involves listing criteria and defining logical relationships (AND/OR) between them.

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

### Synchronize BlockEntity Data on Block Update (Java)

Source: https://docs.neoforged.net/docs/1.20.4/blockentities

This Java snippet shows how to synchronize BlockEntity data to the client when a block update occurs. It involves overriding `getUpdateTag` to prepare the data, `getUpdatePacket` to create the packet, and potentially `onDataPacket` to handle it on the client. The `Level#sendBlockUpdated` method is used to trigger the update.

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

// To send the packet, call this on the server:
// Level#sendBlockUpdated(BlockPos pos, BlockState oldState, BlockState newState, int flags)
// where flags should include 2 (Block#UPDATE_CLIENTS)
```

--------------------------------

### Registering Custom Geometry Loaders (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Registers a custom geometry loader with the Neoforged mod event bus on the physical client. This is essential for the game to recognize and utilize custom model loaders.

```java
@SubscribeEvent // on the mod event bus only on the physical client
public static void registerGeometryLoaders(ModelEvent.RegisterGeometryLoaders event) {
    event.register(MyGeometryLoader.ID, MyGeometryLoader.INSTANCE);
}
```

--------------------------------

### Custom Recipe JSON Structure

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/custom

Defines the structure for custom recipe JSON files. The 'type' field must match the registry name of the recipe serializer. Additional data fields are determined by the specific serializer implementation.

```json
{
  // The custom serializer registry name
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

### Register a Block using DeferredRegister with Supplier Type

Source: https://docs.neoforged.net/docs/1.20.4/concepts/registries

An alternative way to register a Block using DeferredRegister where the field type is Supplier<Block>. This leverages the fact that DeferredHolder implements Supplier, allowing for more flexible type handling.

```java
public static final Supplier<Block> EXAMPLE_BLOCK = BLOCKS.register(
        "example_block", // Our registry name.
        () -> new Block(...)
);

```

--------------------------------

### Register SoundDefinitionsProvider to GatherDataEvent

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/sounds

Registers the custom SoundDefinitionsProvider to the DataGenerator within the GatherDataEvent. This ensures that the custom sounds.json file is generated.

```java
@SubscribeEvent
public static void gatherData(GatherDataEvent event) {
    DataGenerator generator = event.getGenerator();
    PackOutput output = generator.getPackOutput();
    ExistingFileHelper existingFileHelper = event.getExistingFileHelper();

    generator.addProvider(
            event.includeClient(),
            new MySoundDefinitionsProvider(output, existingFileHelper)
    );
}
```

--------------------------------

### NbtProvider (CopyNbtFunction)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

A specialized loot function for copying NBT data. It defines the source from which tag information should be pulled.

```java
CopyNbtFunction.Builder nbtProvider = CopyNbtFunction.copyNbt(NbtProviderType.SOURCE)
    .build();
```

--------------------------------

### Register Custom ParticleType

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/particles

Registers the custom ParticleType with the game's particle system. This is done using the `PARTICLE_TYPES.register` method, providing a unique name and a supplier for the custom particle type instance.

```java
public static final Supplier<MyParticleType> MY_CUSTOM_PARTICLE = PARTICLE_TYPES.register(
        "my_custom_particle",
        () -> new MyParticleType(false));

```

--------------------------------

### Create EnumProperty for Blockstates

Source: https://docs.neoforged.net/docs/1.20.4/blocks/states

Defines an enum property for blockstates, allowing a property to take on values from a specified enum class. This is useful for properties with a fixed set of distinct states.

```java
EnumProperty<MyEnum> myEnumProperty = EnumProperty.create("my_property_name", MyEnum.class);

```

--------------------------------

### Copying Block Tags to Item Tags in NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/datagen/tags

This Java code snippet shows how to efficiently generate item tags that mirror existing block tags using the `#copy` method within `ItemTagsProvider`. This is useful when block tags have corresponding item representations.

```java
//In ItemTagsProvider#addTags  
this.copy(EXAMPLE_BLOCK_TAG, EXAMPLE_ITEM_TAG);

```

--------------------------------

### Checking Block Type at a Position

Source: https://docs.neoforged.net/docs/1.20.4/blocks

Shows how to check if a block at a specific position in the game world is of a registered type. It utilizes the `getBlockState` method and compares it with the registered block constant.

```java
level.getBlockState(position) // returns the blockstate placed in the given level (world) at the given position  
        .is(MyBlockRegistrationClass.MY_BLOCK.get());
```

--------------------------------

### Handle Anvil Updates with NeoForge Event

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes/incode

This Java code snippet shows how to modify anvil behavior by subscribing to the AnvilUpdateEvent. It allows developers to define custom outputs, experience costs, and material costs for anvil operations based on the input items.

```java
import net.minecraft.world.item.ItemStack;
import net.minecraftforge.event.AnvilUpdateEvent;
import net.minecraftforge.eventbus.api.SubscribeEvent;
import net.minecraftforge.fml.common.Mod;

@Mod.EventBusSubscriber // Subscribe to the NeoForge event bus
public class AnvilRecipeHandler {

    @SubscribeEvent
    public static void updateAnvil(AnvilUpdateEvent event) {
        ItemStack leftStack = event.getLeft();
        ItemStack rightStack = event.getRight();

        // Example: If left is a sword and right is a diamond, apply a custom enchantment effect
        if (leftStack.getItem() instanceof net.minecraft.world.item.SwordItem && rightStack.getItem() == net.minecraft.world.item.Items.DIAMOND) {
            // Check if the sword is damaged
            if (leftStack.getDamageValue() > 0) {
                ItemStack outputStack = leftStack.copy(); // Create a copy to modify
                // Apply custom logic here, e.g., repair damage, add enchantments
                // For demonstration, let's just set a placeholder output
                outputStack.setDamageValue(leftStack.getDamageValue() - 1);
                event.setOutput(outputStack);
                event.setCost(5); // Set experience cost
                event.setMaterialCost(1); // Set material cost (e.g., 1 diamond)
            }
        }
    }
}
```

--------------------------------

### Register Payload Registrar

Source: https://docs.neoforged.net/docs/1.20.4/networking/payload

Retrieves the IPayloadRegistrar for a given namespace from the RegisterPayloadHandlerEvent. This is the first step in registering custom payloads.

```java
@SubscribeEvent // on the mod event bus  
public static void register(final RegisterPayloadHandlerEvent event) {  
    final IPayloadRegistrar registrar = event.registrar("mymod");  
}  

```

--------------------------------

### Recipe Advancement Criteria (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/recipes

Defines the criteria for unlocking a recipe advancement in Minecraft. It uses the 'minecraft:recipe_unlocked' trigger and specifies the recipe that needs to be unlocked. This JSON snippet is typically part of a recipe advancement file.

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

### Create a Map Value Remover in Java

Source: https://docs.neoforged.net/docs/1.20.4/datamaps

This Java code defines a `MapRemover` that implements `DataMapValueRemover`. It allows selective removal of a specific key from a String-to-String map within a data map. The `CODEC` is used for decoding remover instances, and the `remove` method modifies the map by removing the specified key, returning an Optional of the new map or an empty Optional to remove the value entirely.

```java
public record MapRemover(String key) implements DataMapValueRemover<Item, Map<String, String>> {
    public static final Codec<MapRemover> CODEC = Codec.STRING.xmap(MapRemover::new, MapRemover::key);
      
    @Override
    public Optional<Map<String, String>> remove(Map<String, String> value, Registry<Item> registry, Either<TagKey<Item>, ResourceKey<Item>> source, Item object) {
        final Map<String, String> newMap = new HashMap<>(value);
        newMap.remove(key);
        return Optional.of(newMap);
    }
}
```

--------------------------------

### Declare Tag Groupings with JSON

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/tags

This JSON structure defines how to declare custom tag groupings for game objects. It supports replacing existing tags, adding new values, and optionally including or excluding specific items with a 'required' flag. This is used within a mod's datapack.

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

### Advanced Value Removal in Data Maps (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/datamaps/structure

Presents an advanced method for removing values from data maps when custom removers are involved. This approach transforms the 'remove' array into a map, allowing for the specification of remover arguments for specific registry entries.

```json
{
    "remove": {
        "minecraft:carrot": "somekey1"
    }
}
```

--------------------------------

### Check Tool Action with Loot Condition (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/loottables

This JSON snippet demonstrates how to use the 'forge:can_tool_perform_action' condition within a loot table. It specifies that the loot should only apply if the tool in the LootContextParams can perform the 'axe_strip' action. This is useful for creating loot that depends on the player having the correct tool equipped.

```json
{
  "conditions": [
    {
      "condition": "forge:can_tool_perform_action",
      "action": "axe_strip"
    }
  ]
}
```

--------------------------------

### Custom Trigger Instance Match Method (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/advancements

Implements the 'matches' method for a custom trigger instance in Java. This method checks if the provided ItemStack meets the conditions defined by the ItemPredicate, which is part of the trigger instance's state.

```java
// This method is unique for each instance and is as such not overridden
public boolean matches(ItemStack stack) {
  // Since ItemPredicate matches a stack, a stack is the input
  return this.item.matches(stack);
}
```

--------------------------------

### Conditional Value Removal in Data Maps (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/datamaps/structure

Explains how to combine adding and removing values within the same data map JSON. It demonstrates attaching a value to a tag and then removing it from a specific entry within that tag, effectively applying the value to all but the excluded entries.

```json
{
    "values": {
        "#minecraft:logs": 12
    },
    "remove": ["minecraft:acacia_log"]
}
```

--------------------------------

### Querying Data Map Values on Item Drop Events (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datamaps

This snippet demonstrates how to retrieve a custom data map value (`DROP_HEALING`) associated with an ItemStack during an `ItemTossEvent`. It checks if the value exists and then uses its properties to determine if the player should be healed. This requires the NeoForged event bus and access to `ItemStack` and `Holder` objects.

```java
@SubscribeEvent
public static void onItemDrop(final ItemTossEvent event) {
    final ItemStack stack = event.getEntity().getItem();
    final DropHealing value = stack.getItemHolder().getData(DROP_HEALING);
    if (value != null) {
        if (event.getPlayer().level().getRandom().nextFloat() > value.chance()) {
            event.getPlayer().heal(value.amount());
        }
    }
}
```

--------------------------------

### Ambient Occlusion Toggle in ModelBuilder (Java)

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/datagen

Illustrates how to enable or disable ambient occlusion for a model using the `#ao` method in ModelBuilder. This affects how lighting interacts with the model's surfaces.

```java
modelBuilder.ao(true);
modelBuilder.ao(false);
```

--------------------------------

### Create BooleanProperty for Blockstates

Source: https://docs.neoforged.net/docs/1.20.4/blocks/states

Defines a boolean property for blockstates, representing a true/false state. This is commonly used for properties like 'powered', 'lit', or 'enabled'.

```java
BooleanProperty myBooleanProperty = BooleanProperty.create("my_property_name");

```

--------------------------------

### NumberProvider Builder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

Determines the number of times a loot pool executes. Various provider types exist, including those that use scoreboard values.

```java
NumberProvider numberProvider = NumberProvider.numberProvider(providerType)
    .build();
```

--------------------------------

### Particle Definition JSON Format

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/particles

This JSON structure defines the textures used for a sprite-based particle. It specifies a list of texture names that will be played in sequence, looping if necessary. Texture locations are relative to the `textures/particle` folder within the asset namespace.

```json
{
  "textures": [
    "examplemod:my_particle_0",
    "examplemod:my_particle_1",
    "examplemod:my_particle_2",
    "examplemod:my_particle_3"
  ]
}
```

--------------------------------

### Define Data Map Value with Codec (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datamaps

Defines an immutable record representing a data map value and its associated Codec for serialization/deserialization. The Codec is built using RecordCodecBuilder, specifying fields and their corresponding getters. This ensures data integrity and proper handling of data map values.

```java
public record DropHealing(
        float amount, float chance
) {
    public static final Codec<DropHealing> CODEC = RecordCodecBuilder.create(in -> in.group(
            Codec.FLOAT.fieldOf("amount").forGetter(DropHealing::amount),
            Codec.floatRange(0, 1).fieldOf("chance").forGetter(DropHealing::chance)
    ).apply(in, DropHealing::new));
}

```

--------------------------------

### Registering Global Loot Modifiers with JSON

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/glm

This JSON file defines the global loot modifiers to be loaded into the game. It specifies whether to replace existing modifiers or append to the list and lists the resource locations of individual modifier configurations. This file must be placed in 'data/forge/loot_modifiers/global_loot_modifiers.json'.

```json
{
  "replace": false, 
  "entries": [
    "examplemod:example_glm",
    "examplemod:example_glm2"
  ]
}
```

--------------------------------

### Item Exists Condition

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/conditional

The 'neoforge:item_exists' condition verifies if a given item has been registered within the current application. The 'item' property specifies the item ID to check.

```json
{
  "type": "neoforge:item_exists",
   // Returns true if 'examplemod:example_item' has been registered
  "item": "examplemod:example_item"
}
```

--------------------------------

### Configure Vibration Frequencies with neoforge:vibration_frequencies Data Map

Source: https://docs.neoforged.net/docs/1.20.4/datamaps/neo_maps

The `neoforge:vibration_frequencies` data map configures shulker vibration frequencies emitted by game events, replacing `VibrationSystem#VIBRATION_FREQUENCY_FOR_EVENT`. It is located at `neoforge/data_maps/game_event/vibration_frequencies.json`. The structure includes a 'frequency' integer between 1 and 15.

```json
{
    "values": {
        "minecraft:splash": {
            "frequency": 2
        }
    }
}
```

--------------------------------

### Implementing IGlobalLootModifier in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/glm

This Java code snippet outlines the structure for implementing a custom global loot modifier. It requires defining the '#apply' method to handle loot modifications and the '#codec' method to return the registered codec for JSON serialization/deserialization. The '#apply' method receives loot and context, returning the modified loot list.

```java
public abstract class GlobalLootModifier implements IGlobalLootModifier {
    // ...

    public abstract Object apply(Object $$0, Object $$1);

    public abstract Codec<? extends GlobalLootModifier> codec();

    // ...
}
```

--------------------------------

### Define Custom Armor Material (Java)

Source: https://docs.neoforged.net/docs/1.20.4/items/tools

This Java code defines a custom armor material named 'COPPER_ARMOR_MATERIAL'. It specifies properties like name, durability, defense values, toughness, knockback resistance, enchantability, equip sound, and repair ingredient. The durability and defense values are determined based on the armor piece type (HELMET, CHESTPLATE, LEGGINGS, BOOTS).

```java
// We place copper somewhere between chainmail and iron.  
public static final ArmorMaterial COPPER_ARMOR_MATERIAL = new ArmorMaterial() {  
    // The name of the armor material. Mainly determines where the armor texture is. Should contain  
    // a leading mod id to guarantee uniqueness, otherwise there may be issues when two mods  
    // try to add the same armor material. (If the mod id is omitted, the "minecraft" namespace will be used.)  
    @Override  
    public String getName() {  
        return "modid:copper";  
    }  
  
    // Override for StringRepresentable. Should generally return the same as getName().  
    @Override  
    public String getSerializedName() {  
        return getName();  
    }  
  
    // Determines the durability of this armor material, depending on what armor piece it is.  
    // ArmorItem.Type is an enum of four values: HELMET, CHESTPLATE, LEGGINGS and BOOTS.  
    // Vanilla armor materials determine this by using a base value and multiplying it with a type-specific constant.  
    // The constants are 13 for BOOTS, 15 for LEGGINGS, 16 for CHESTPLATE and 11 for HELMET.  
    // Both chainmail and iron use 15 as the base value, so we'll use it as well.  
    @Override  
    public int getDurabilityForType(ArmorItem.Type type) {  
        return switch (type) {  
            case HELMET -> 11 * 15;  
            case CHESTPLATE -> 16 * 15;  
            case LEGGINGS -> 15 * 15;  
            case BOOTS -> 13 * 15;  
        };
    }  
  
    // Determines the defense value of this armor material, depending on what armor piece it is.  
    @Override  
    public int getDurabilityForType(ArmorItem.Type type) {  
        return switch (type) {  
            case HELMET -> 2;  
            case CHESTPLATE -> 4;  
            case LEGGINGS -> 6;  
            case BOOTS -> 2;  
        };
    }  
  
    // Returns the toughness value of the armor. The toughness value is an additional value included in  
    // damage calculation, for more information, refer to the Minecraft Wiki's article on armor mechanics:  
    // https://minecraft.wiki/w/Armor#Armor_toughness  
    // Only diamond and netherite have values greater than 0 here, so we just return 0.  
    @Override  
    public float getToughness() {  
        return 0;  
    }  
  
    // Returns the knockback resistance value of the armor. While wearing this armor, the player is  
    // immune to knockback to some degree. If the player has a total knockback resistance value of 1 or greater  
    // from all armor pieces combined, they will not take any knockback at all.  
    // Only netherite has values greater than 0 here, so we just return 0.  
    @Override  
    public float getKnockbackResistance() {  
        return 0;  
    }  
  
    // Determines the enchantability of the tier. This represents how good the enchantments on this armor will be.  
    // Gold uses 25, we put copper slightly below that.  
    @Override  
    public int getEnchantmentValue(ArmorItem.Type type) {  
        return 20;  
    }  
  
    // Determines the sound played when equipping this armor.  
    @Override  
    public SoundEvent getEquipSound() {  
        return SoundEvents.ARMOR_EQUIP_GENERIC;  
    }  
  
    // Determines the repair item for this armor.  
    @Override  
    public Ingredient getRepairIngredient() {  
        return Ingredient.of(Tags.Items.INGOTS_COPPER);  
    }  
      
    // Optionally, you can also override #getArmorTexture here. This method returns a ResourceLocation  
    // that determines where the armor location is stored, in case you want to store it in a non-default location.  
    // See the default implementation in Tier for an example.  
}

```

--------------------------------

### Defining a Serialized Global Loot Modifier

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/glm

This JSON file defines the specific properties and conditions for a single global loot modifier. It includes a 'type' for the codec, a list of 'conditions' for activation, and any custom properties that the modifier's operational code will read. This allows data pack creators to adjust balance without modifying the code.

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

### Tag Empty Condition

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/conditional

This condition, 'neoforge:tag_empty', evaluates to true if a specified item tag contains no items. The 'tag' property indicates the item tag to check.

```json
{
  "type": "neoforge:tag_empty",
   // Returns true if 'examplemod:example_tag' is an item tag with no entries
  "tag": "examplemod:example_tag"
}
```

--------------------------------

### Invalidate Block Capabilities (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datastorage/capabilities

Provides the code snippet for explicitly invalidating block capabilities at a given position. This method must be called by modders whenever a capability changes, appears, or disappears to ensure caches are updated correctly. NeoForge handles some common cases automatically.

```Java
// whenever a capability changes, appears, or disappears:
level.invalidateCapabilities(pos);
```

--------------------------------

### Elements Model JSON - NeoForged

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models/modelloaders

Defines an elements model in JSON for NeoForged, consisting of block model elements and an optional root transform. This is primarily for use outside standard model rendering, such as in BlockEntityRenderers.

```json
{
  "loader": "neoforge:elements",
  "elements": [...],
  "transform": {...}
}
```

--------------------------------

### Mod Loaded Condition

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/conditional

This condition, 'neoforge:mod_loaded', checks if a specific mod is loaded in the current application. It requires the 'modid' of the mod to be specified.

```json
{
  "type": "neoforge:mod_loaded",
   // Returns true if 'examplemod' is loaded
  "modid": "examplemod"
}
```

--------------------------------

### Overwrite Values in Data Maps (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/datamaps/structure

Illustrates how to overwrite existing values for specific registry entries in a data map JSON. This method uses a 'replace': true flag and nests the new value under a 'value' object, ensuring that the merger is not invoked for this specific object.

```json
{
    "values": {
        "minecraft:carrot": {
            "replace": true,
            "value": {
                "amount": 12,
                "chance": 1
            }
        }
    }
}
```

--------------------------------

### Implement LootModifier Abstract Class in Java

Source: https://docs.neoforged.net/docs/1.20.4/resources/server/glm

Extends the `LootModifier` abstract class to create a custom loot modifier. The constructor takes an array of `LootItemCondition`s, and the `#doApply` method is overridden to implement the loot modification logic. The `codec()` method must return the codec for serialization.

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

### JSON Item Override Model Definition

Source: https://docs.neoforged.net/docs/1.20.4/resources/client/models

This JSON structure defines how an item's model can change based on specific 'predicate' conditions. It's commonly used for items like bows to alter their appearance based on charge level. The order of overrides is important, as the last matching predicate determines the model.

```json
{
  "overrides": [
    {
      "predicate": {
        "pulling": 1
      },
      "model": "item/bow_pulling_0"
    },
    {
      "predicate": {
        "pulling": 1,
        "pull": 0.65
      },
      "model": "item/bow_pulling_1"
    },
    {
      "predicate": {
        "pulling": 1,
        "pull": 0.9
      },
      "model": "item/bow_pulling_2"
    }
  ]
}
```

--------------------------------

### Remove Values from Data Maps (JSON)

Source: https://docs.neoforged.net/docs/1.20.4/datamaps/structure

Shows how to remove previously attached values from registry entries or tags using the 'remove' array in a data map JSON file. This is useful for excluding specific items or all items within a tag from having a data map value applied.

```json
{
    "remove": ["minecraft:apple", "minecraft:potato"]
}
```

--------------------------------

### Configure Raid Hero Gifts with neoforge:raid_hero_gifts Data Map

Source: https://docs.neoforged.net/docs/1.20.4/datamaps/neo_maps

The `neoforge:raid_hero_gifts` data map configures the loot table gifted to players by villagers after stopping a raid, replacing `GiveGiftToHero#GIFTS`. It is located at `neoforge/data_maps/villager_profession/raid_hero_gifts.json`. The structure specifies a 'loot_table' string for each villager profession.

```json
{
    "values": {
         "minecraft:armorer": {
            "loot_table": "minecraft:gameplay/hero_of_the_village/armorer_gift"
        }
    }
}
```

--------------------------------

### LootItemFunction Builder

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

Modifies the results of loot generation before output. Each function is associated with a registered LootItemFunctionType.

```java
LootItemFunction function = LootItemFunction.lootItemFunction(functionType)
    .build();
```

--------------------------------

### ScoreboardNameProvider (ScoreboardValue)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/loottables

A specific type of number provider that uses a scoreboard value to determine the number of rolls. It defines the name of the scoreboard to query.

```java
ScoreboardValue.Builder scoreboardProvider = ScoreboardValue.scoreboardValue(scoreboardName)
    .build();
```

--------------------------------

### Remove MobEffect from Entity in Java

Source: https://docs.neoforged.net/docs/1.20.4/items/mobeffects

Illustrates how to remove a specific MobEffect from a LivingEntity. Since only one instance of a MobEffect can exist per entity, specifying the MobEffect is sufficient for removal.

```java
entity.removeEffect(MobEffects.REGENERATION);

```

--------------------------------

### Generate Global Loot Modifiers with NeoForged DataGenerator (Java)

Source: https://docs.neoforged.net/docs/1.20.4/datagen/glm

This snippet demonstrates how to generate Global Loot Modifiers (GLMs) by subclassing `GlobalLootModifierProvider` and registering it with the `DataGenerator`. It shows how to add a custom modifier with specific conditions and items.

```java
@SubscribeEvent // on the mod event bus  
public static void gatherData(GatherDataEvent event) {  
    event.getGenerator().addProvider(  
        // Tell generator to run only when server data are generating  
        event.includeServer(),  
        output -> new MyGlobalLootModifierProvider(output, MOD_ID)  
    );
}

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

### Add Attribute Modifier to MobEffect in Java

Source: https://docs.neoforged.net/docs/1.20.4/items/mobeffects

Demonstrates adding an attribute modifier to a custom MobEffect. This allows the effect to modify entity attributes like attack damage. Requires a unique UUID, the attribute to modify, a value, and an operation type. The UUID must be a valid UUIDv4.

```java
public static final String MY_MOB_EFFECT_UUID = "01234567-89ab-cdef-0123-456789abcdef";
public static final Holder<MyMobEffect> MY_MOB_EFFECT = MOB_EFFECTS.register("my_mob_effect", () -> new MyMobEffect(...)
        .addAttributeModifier(Attribute.ATTACK_DAMAGE, MY_MOB_EFFECT_UUID, 2.0, AttributeModifier.Operation.ADD)
);
```

=== COMPLETE CONTENT === This response contains all available snippets from this library. No additional content exists. Do not make further requests.
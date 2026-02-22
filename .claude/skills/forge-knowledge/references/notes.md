### Mixin Setup: Gradle Build Configuration

Source: https://toapuro.github.io/modding-notes/content/mixin

Provides the necessary Gradle configurations to set up Mixin for a Minecraft modding project. This includes adding the MixinGradle plugin, the Mixin processor dependency, and configuring Mixin within the build.gradle file.

```gradle
buildscript {
    repositories {
        maven { url = 'https://repo.spongepowered.org/repository/maven-public/' }
        mavenCentral()
    }
    dependencies {
        classpath 'org.spongepowered:mixingradle:0.7-SNAPSHOT'
    }
}
```

```gradle
plugins {
    ...
}
apply plugin: 'org.spongepowered.mixin'

```

```gradle
dependencies {
    annotationProcessor 'org.spongepowered:mixin:0.8.5:processor'
}
```

```gradle
mixin {
    add sourceSets.main, "${mod_id}.refmap.json"

    config "${mod_id}.mixins.json"
}
```

--------------------------------

### Declare Modrinth Maven Dependency in Gradle

Source: https://toapuro.github.io/modding-notes/content/getting-started/dependency

Demonstrates how to declare a dependency using the Modrinth Maven format. The format is 'maven.modrinth:<projectID>:<version>', as shown with an example for JEI.

```gradle
dependencies {
    implementation fg.deobf("maven.modrinth:jei:15.20.0.129")
}
```

--------------------------------

### Accessing Fields and Invoking Methods via Mixin Interface

Source: https://toapuro.github.io/modding-notes/content/mixin/injections

Shows how to define an interface with @Accessor and @Invoker annotations to access fields and methods of a target class from outside the Mixin class. It includes examples of how to cast and use the accessor interface.

```java
@Mixin(Example.class, remap = false)
public interface ExampleAccessor {

    @Accessor("exampleValue")
    int getExampleValue();

    @Mutable
    @Accessor("exampleValue")
    void setExampleValue(int exampleValue);

    @Invoker("handle")
    void invokeHandle(int something);
}

// Usage

Example example = ...;

((ExampleAccessor)(Object)example).setExampleValue(10);

// Or

ExampleAccessor accessor = (ExampleAccessor) example;
accessor.setExampleValue(10);
```

--------------------------------

### Dynamic Item Model JSON Configuration

Source: https://toapuro.github.io/modding-notes/content/rendering/renderer/model

This JSON configuration defines different model variants ('small' and 'large') for an item. The 'loader' specifies a custom model loader, and each variant points to a specific parent model resource location. This setup is used in conjunction with code to dynamically select between these models.

```json
{
  "loader": "examplemod:example",
  "small": {
    "parent": "examplemod:item/example_small"
  },
  "large": {
    "parent": "examplemod:item/example_large"
  }
}
```

--------------------------------

### Specify Git Ignore Rules in .gitignore

Source: https://toapuro.github.io/modding-notes/content/getting-started/mod-files

The .gitignore file specifies intentionally untracked files that Git should ignore. This example shows how to exclude the 'run' directory, which is typically not needed in the repository.

```gitignore
# Ignore the run directory
run/
```

--------------------------------

### Modの初期化とアイテム登録の紐付け - ExampleMod (Java)

Source: https://toapuro.github.io/modding-notes/content/registries/basic

Modのメインクラスで、DeferredRegisterに登録されたアイテムをModイベントバスに紐付ける方法を示します。これにより、ゲーム起動時にアイテムが正しくレジストリに登録されます。Neoforge / 1.20.1 Forge (3.10以降) のコンストラクタで登録処理が行われます。

```java
public class ExampleMod {
    public static final String MODID = "examplemod";

    // Neoforge / 1.20.1 Forge (3.10以降) 
    public ExampleMod(FMLJavaModLoadingContext context) {
        IEventBus modBus = context.getModEventBus();

        // 登録
        ModItems.ITEMS.register(modBus);
    }

    // else
    @SuppressWarnings("removal")
    public ExampleMod() {
        this(FMLJavaModLoadingContext.get());
    }
}
```

--------------------------------

### Development Environment Run Configurations

Source: https://toapuro.github.io/modding-notes/content/advanced/gradle

Sets up configurations for running the Minecraft client and server during development. It defines working directories, logging levels, and how mods are included. Specific configurations for client, server, gameTestServer, and data generation tasks are provided.

```gradle
minecraft {
    runs {
        configureEach {
            workingDirectory project.file('run')

            property 'forge.logging.markers', 'REGISTRIES'

            property 'forge.logging.console.level', 'debug'

            mods {
                "${mod_id}" {
                    source sourceSets.main
                }
            }
        }

        client {
            property 'forge.enabledGameTestNamespaces', mod_id
        }

        server {
            property 'forge.enabledGameTestNamespaces', mod_id
            args '--nogui'
        }

        gameTestServer {
            property 'forge.enabledGameTestNamespaces', mod_id
        }

        data {
            workingDirectory project.file('run-data')

            args '--mod', mod_id, '--all', '--output', file('src/generated/resources/'), '--existing', file('src/main/resources/')
        }
    }
}

```

--------------------------------

### Gradle Dependency Configurations

Source: https://toapuro.github.io/modding-notes/content/getting-started/dependency

Demonstrates the basic structure for declaring dependencies in a Gradle build file. It shows how to use 'implementation' to include a dependency like JEI.

```gradle
dependencies {
    // JEI
    implementation fg.deobf("curse.maven:jei-238222:4712866")
}
```

--------------------------------

### Mod開発用Gradleタスク

Source: https://toapuro.github.io/modding-notes/content/getting-started/setup

IntelliJ IDEAのGradleツールウィンドウから実行できる、Mod開発に関連する主要なタスクを示しています。コードのビルドや、開発環境でのクライアント実行などが含まれます。

```text
Tasks->build->build (jarファイルへのビルド)
Tasks->forgegradle runs->runClient (開発環境でのクライアント実行)
```

--------------------------------

### Mod Class Initialization (Neoforge/Forge 1.20.1+)

Source: https://toapuro.github.io/modding-notes/content/getting-started/mod

Initializes the Mod class by accepting FMLJavaModLoadingContext in the constructor for Neoforge/Forge 1.20.1 and later. This is the recommended approach for newer development.

```java
@Mod(ExampleMod.MODID)
public class ExampleMod {
    public static final String MODID = "examplemod";

    public ExampleMod(FMLJavaModLoadingContext context) {
        IEventBus modBus = context.getModEventBus();
    }
}
```

--------------------------------

### gradle.properties の設定例

Source: https://toapuro.github.io/modding-notes/content/getting-started/setup

Modのメタデータ（ID、名前、ライセンス、グループID、作者、説明）を設定するための`gradle.properties`ファイルの記述例です。Mod開発プロジェクトの基本的な設定を行います。

```properties
minecraft_version_range=[1.20.1]
mod_id=modding_example
mod_name=ModdingExample
mod_license=MIT
mod_group_id=dev.toapuro
mod_authors=toapuro, another_author
mod_description=A example mod
```

--------------------------------

### Dynamic Item Model Implementation in Java

Source: https://toapuro.github.io/modding-notes/content/rendering/renderer/model

This Java class, ExampleModel, extends SimpleUnbakedGeometry to handle dynamic model switching for items. It deserializes 'small' and 'large' BlockModels and overrides the bake method to use a custom ExampleOverrides class. The ExampleOverrides class resolves the appropriate model based on the item's count.

```java
public class ExampleModel extends SimpleUnbakedGeometry<ExampleModel> {

    public static final IGeometryLoader<ExampleModel> LOADER = ExampleModel::deserialize;

    public final BlockModel smallModel;
    public final BlockModel largeModel;

    public ExampleModel(BlockModel smallModel, BlockModel largeModel) {
        this.smallModel = smallModel;
        this.largeModel = largeModel;
    }

    // 別クラスに移動しても良い
    public static ExampleModel deserialize(JsonObject json, JsonDeserializationContext context) {
        BlockModel smallModel = context.deserialize(GsonHelper.getAsJsonObject(json, "small"), BlockModel.class);
        BlockModel largeModel = context.deserialize(GsonHelper.getAsJsonObject(json, "large"), BlockModel.class);

        return new ExampleModel(smallModel, largeModel);
    }

    @Override
    public BakedModel bake(IGeometryBakingContext context, ModelBaker baker, Function<Material, TextureAtlasSprite> spriteGetter, ModelState modelState, ItemOverrides overrides, ResourceLocation modelLocation) {
        overrides = new ExampleOverrides();

        BakedModel bakedSmallModel = this.smallModel.bake(baker, this.smallModel, spriteGetter, modelState, modelLocation, context.useBlockLight());
        BakedModel bakedLargeModel = this.largeModel.bake(baker, this.largeModel, spriteGetter, modelState, modelLocation, context.useBlockLight());

        return new Baked(
                context.useAmbientOcclusion(),
                context.isGui3d(),
                context.useBlockLight(),
                spriteGetter.apply(context.getMaterial("particle")),
                overrides,
                bakedSmallModel,
                bakedLargeModel
        );
    }

    @Override
    public void resolveParents(Function<ResourceLocation, UnbakedModel> modelGetter, IGeometryBakingContext context) {
        this.smallModel.resolveParents(modelGetter);
        this.largeModel.resolveParents(modelGetter);
        super.resolveParents(modelGetter, context);
    }

    @Override
    protected void addQuads(IGeometryBakingContext iGeometryBakingContext, IModelBuilder<?> iModelBuilder, ModelBaker modelBaker, Function<Material, TextureAtlasSprite> function, net.minecraft.client.resources.model.ModelState modelState, ResourceLocation resourceLocation) {

    }

    public static class Baked implements IDynamicBakedModel {
        private final boolean isAmbientOcclusion;
        private final boolean isGui3d;
        private final boolean isSideLit;
        private final TextureAtlasSprite particle;
        private final ItemOverrides overrides;
        private final BakedModel smallModel;
        private final BakedModel largeModel;

        public Baked(boolean isAmbientOcclusion, boolean isGui3d, boolean isSideLit, TextureAtlasSprite particle, ItemOverrides overrides, BakedModel smallModel, BakedModel largeModel) {
            this.isAmbientOcclusion = isAmbientOcclusion;
            this.isGui3d = isGui3d;
            this.isSideLit = isSideLit;
            this.particle = particle;
            this.overrides = overrides;

            this.smallModel = smallModel;
            this.largeModel = largeModel;
        }

        public @NotNull List<BakedQuad> getQuads(@Nullable BlockState state, @Nullable Direction side, @NotNull RandomSource rand, @NotNull ModelData data, @Nullable RenderType renderType) {
            return smallModel.getQuads(state, side, rand, data, renderType);
        }

        @Override
        public boolean useAmbientOcclusion() {
            return this.isAmbientOcclusion;
        }

        @Override
        public boolean isGui3d() {
            return this.isGui3d;
        }

        @Override
        public boolean usesBlockLight() {
            return this.isSideLit;
        }

        @Override
        public boolean isCustomRenderer() {
            return false;
        }

        @Override
        public @NotNull TextureAtlasSprite getParticleIcon() {
            return this.particle;
        }

        @Override
        public @NotNull ItemOverrides getOverrides() {
            return overrides;
        }
    }

    public static class ExampleOverrides extends ItemOverrides {

        @Override
        public @Nullable BakedModel resolve(@NotNull BakedModel model, @NotNull ItemStack itemStack, @Nullable ClientLevel level, @Nullable LivingEntity livingEntity, int partialTicks) {
            int count = itemStack.getCount();

            if(model instanceof Baked baked) {

```

--------------------------------

### Configure Local JAR Repository in Gradle

Source: https://toapuro.github.io/modding-notes/content/getting-started/dependency

Sets up Gradle to load JAR files from a local 'libs' directory. This is useful for mods not available through online repositories.

```gradle
repositories {
    flatDir {
        dirs "libs"
    }
}
```

--------------------------------

### RegistryObjectの定義 - Java

Source: https://toapuro.github.io/modding-notes/content/registries/basic

DeferredRegisterが管理するレジストリエントリへの参照を保持するRegistryObjectの定義方法です。registerメソッドにアイテムIDとアイテムのインスタンスを生成するSupplierを渡すことで、登録と同時に参照が作成されます。

```java
public static final RegistryObject<Item> EXAMPLE_ITEM = ITEMS.register("example_item", () -> new Item(new Item.Properties()));

```

--------------------------------

### Register a Basic Item - Java

Source: https://toapuro.github.io/modding-notes/content/registries/item

Registers a new item to the game's registry. This is the fundamental step for adding any item. It requires a unique ID and an instance of the Item class.

```java
public class ModItems {
    public static final DeferredRegister<Item> ITEMS = DeferredRegister.create(ForgeRegistries.ITEMS, ExampleMod.MODID);

    public static final RegistryObject<Item> EXAMPLE_ITEM = ITEMS.register("example_item", () ->
            new Item(new Item.Properties())
    );
}
```

--------------------------------

### DeferredRegisterの初期化 - Java

Source: https://toapuro.github.io/modding-notes/content/registries/basic

DeferredRegisterを初期化する際のコードスニペットです。第一引数でレジストリ（ForgeRegistries.*, ForgeRegistries.Keys.*, Registries.*など）を指定し、第二引数でMODIDを指定します。これにより、遅延登録の準備が整います。

```java
public static final DeferredRegister<Item> ITEMS = DeferredRegister.create(
        // レジストリを指定
        ForgeRegistries.ITEMS,
        // MODID
        ExampleMod.MODID
);

```

--------------------------------

### Add Parchment Maven Repository to settings.gradle

Source: https://toapuro.github.io/modding-notes/content/advanced/mapping

This snippet shows how to add the ParchmentMC Maven repository to the `settings.gradle` file. This is a necessary step to allow Gradle to download the Parchment mapping artifacts. It should be added within the `pluginManagement` and `repositories` blocks.

```gradle
pluginManagement {
    repositories {
        maven { url = 'https://maven.parchmentmc.org' }
    }
}

```

--------------------------------

### Apply Rotation using Quaternion and PoseStack

Source: https://toapuro.github.io/modding-notes/content/rendering/api

Demonstrates how to apply rotations to objects using Quaternions with PoseStack. This allows for localized transformations, such as rotating a held item.

```java
// Y軸中心に+90度回転します
poseStack.mulPose(Axis.YP.rotationDegrees(90));
// Y軸中心に-90度回転します
poseStack.mulPose(Axis.Y.rotationDegrees(90));
```

--------------------------------

### Mod Class Initialization (Forge < 47.3.10)

Source: https://toapuro.github.io/modding-notes/content/getting-started/mod

Initializes the Mod class using the older method for Forge versions prior to 47.3.10. It retrieves the FMLJavaModLoadingContext using a static getter.

```java
@Mod(ExampleMod.MODID)
public class ExampleMod {
    public static final String MODID = "examplemod";

    @SuppressWarnings("removal")
    public ExampleMod() {
        IEventBus modBus = FMLJavaModLoadingContext.get().getModEventBus();
    }
}
```

--------------------------------

### Mod開発テンプレートの生成方法

Source: https://toapuro.github.io/modding-notes/content/getting-started/setup

NeoForge, Fabric, ForgeといったModローダーに対応した開発環境を生成する複数の方法を示しています。Modジェネレータの利用、テンプレートリポジトリのクローン、Forge MDKのダウンロード、IntelliJプラグイン経由での生成が含まれます。

```text
1. (NeoForge/Fabric) Mod ジェネレータの利用
   - NeoForge Mod Generator
   - Fabric Template
2. (NeoForge) テンプレートをクローン
   - GitHubリポジトリからZIPダウンロードまたはgit clone
3. (Forge) Forge MDKの利用
   - Forge MDKダウンロード・解凍
4. (NeoForge/Forge/Fabric) Intellijプラグイン経由で生成
   - IntelliJの新規プロジェクト作成画面からMinecraftを選択。
```

--------------------------------

### レジストリ内の全エントリの反復処理 - Java

Source: https://toapuro.github.io/modding-notes/content/registries/basic

レジストリに登録されたすべてのエントリに対して処理を行うためのコード例です。keySet()を使用してIDのセットを取得し反復処理するか、entrySet()を使用してキーと値のペアのセットを取得して反復処理します。

```java
for (ResourceLocation id : BuiltInRegistries.BLOCKS.keySet()) {
    // ...
}
for (Map.Entry<ResourceKey<Block>, Block> entry : BuiltInRegistries.BLOCKS.entrySet()) {
    // ...
}

```

--------------------------------

### Java JDK バージョンと Minecraft バージョンの対応

Source: https://toapuro.github.io/modding-notes/content/getting-started/setup

Minecraftのバージョンごとに推奨されるJava Development Kit (JDK) のバージョンを示したテーブルです。Mod開発環境のJDK選定の参考になります。

```text
MC バージョン | JDK バージョン
---|---
1.16.x | JDK 8
1.17.x | JDK 16
1.18.x | JDK 17
1.19.x | JDK 17
1.20.x | JDK 17
1.21.x | JDK 21
```

--------------------------------

### アイテムの登録 - ModItems (Java)

Source: https://toapuro.github.io/modding-notes/content/registries/basic

DeferredRegisterを使用してアイテムを登録する基本的な例です。ForgeRegistries.ITEMSをレジストリとして指定し、MODIDとアイテムIDを指定してアイテムを登録します。RegistryObject<Item>は登録されたアイテムへの参照を保持します。

```java
public class ModItems {
    public static final DeferredRegister<Item> ITEMS = DeferredRegister.create(ForgeRegistries.ITEMS, ExampleMod.MODID);

    // "example_item"というIDとして仮登録
    public static final RegistryObject<Item> EXAMPLE_ITEM = ITEMS.register("example_item", () -> new Item(new Item.Properties()));
}
```

--------------------------------

### RegisterEventを使用したブロック登録 - Java

Source: https://toapuro.github.io/modding-notes/content/registries/basic

RegisterEventを使用してブロックをレジストリに登録する例です。Forge 1.20.1 (47.3.10) 以降またはNeoforgeでサポートされているResourceLocation.fromNamespaceAndPathを使用します。未サポートの場合はnew ResourceLocationを使用します。

```java
@SubscribeEvent // Modイベントバスで登録
public void register(RegisterEvent event) {
    event.register(
        ForgeRegistries.Keys.BLOCKS,
        helper -> {
            helper.register(ResourceLocation.fromNamespaceAndPath(MODID, "example_block_1"), new Block(...));
            helper.register(ResourceLocation.fromNamespaceAndPath(MODID, "example_block_2"), new Block(...));
            helper.register(ResourceLocation.fromNamespaceAndPath(MODID, "example_block_3"), new Block(...));
            // ...
        }
    );
}

```

--------------------------------

### Method Descriptor Syntax in Mixin

Source: https://toapuro.github.io/modding-notes/content/mixin/injections

Method descriptors are crucial for precisely targeting methods and fields within Mixin. This section explains the syntax for class names (e.g., Ljava/lang/Object;), method signatures (e.g., add(II)I), and field descriptors (e.g., Ljava/lang/Boolean;TRUE:Ljava/lang/Boolean;). Understanding this syntax is essential for using @At with INVOKE and FIELD targets.

```java
Lio/github/toapuro/example/Example;add(II)I
Lio/github/toapuro/example/Example;concat(Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;
[I
Ljava/lang/Boolean;TRUE:Ljava/lang/Boolean;
```

--------------------------------

### カスタムレジストリの作成 - RegistryBuilder (Java)

Source: https://toapuro.github.io/modding-notes/content/registries/basic

RegistryBuilderを使用してカスタムレジストリを作成する例です。レジストリID、数値IDの範囲、クライアントとの同期設定、デフォルトキーなどを設定できます。このカスタムレジストリは、NewRegistryEventで登録されます。

```java
private static final ResourceLocation CUSTOM_REGISTRY_ID = ResourceLocation.fromNamespaceAndPath("yourmodid", "custom_id");
public static final RegistryBuilder<Spell> CUSTOM_REGISTRY = RegistryBuilder.<Spell>of(CUSTOM_REGISTRY_ID)
        // 数値IDの割り当てられる範囲
        .setIDRange(0, Integer.MAX_VALUE) // = .setMaxID(Integer.MAX_VALUE)

        // クライアントとの数値IDの同期を無効化
        // .disableSync()

        // 存在しない場合に置き換えられるエントリ。任意
        .setDefaultKey(ResourceLocation.fromNamespaceAndPath("yourmodid", "empty"));

@SubscribeEvent // Modバスに登録
public static void registerRegistries(NewRegistryEvent event) {
    event.register(CUSTOM_REGISTRY);
}

```

--------------------------------

### Apply Parchment ForgeGradle Plugin in build.gradle

Source: https://toapuro.github.io/modding-notes/content/advanced/mapping

This code snippet demonstrates how to apply the Parchment ForgeGradle plugin in the `build.gradle` file. This plugin integrates Parchment mappings into the Forge build process. It must be added after the `net.minecraftforge.gradle` plugin.

```gradle
plugins {
    id 'net.minecraftforge.gradle' version '[6.0.16,6.2)'
    id 'org.parchmentmc.librarian.forgegradle' version '1.+'
}

```

--------------------------------

### IntelliJ IDEA プラグインのインストール

Source: https://toapuro.github.io/modding-notes/content/getting-started/setup

Minecraft Mod開発に必要なプラグインをIntelliJ IDEAにインストールする手順です。Japanese Language Packで日本語化し、Minecraft Developmentプラグインで開発環境を整えます。

```text
1. Intellij IDEAのインストール
2. 日本語化 (任意)
   Pluginsタブから Japanese Language Pack を検索してインストール。
3. Minecraft Development プラグインをインストール
   Pluginsタブから Minecraft Development を検索してインストール。
```

--------------------------------

### カスタムRenderTypeの作成 - Java

Source: https://toapuro.github.io/modding-notes/content/rendering/render-options

バニラのRenderTypeに必要な設定がない場合に独自に作成する方法を示します。VertexFormat、頂点モード、バッファサイズ、そしてCompositeStateの設定を含みます。

```Java
private static final RenderType SOLID = RenderType.create(
    "solid",
    DefaultVertexFormat.BLOCK,
    VertexFormat.Mode.QUADS,
    2097152,
    true,
    false,
    RenderType.CompositeState.builder()
        .setLightmapState(LIGHTMAP)
        .setShaderState(RENDERTYPE_SOLID_SHADER)
        .setTextureState(BLOCK_SHEET_MIPPED)
        .createCompositeState(true)

);
```

--------------------------------

### Minecraft Mappings Configuration

Source: https://toapuro.github.io/modding-notes/content/advanced/gradle

Specifies the Minecraft development mappings to be used. Mappings provide human-readable names for classes, methods, and fields, simplifying the development process.

```gradle
minecraft {
    mappings channel: mapping_channel, version: mapping_version
}

```

--------------------------------

### Mixin: Injecting Code into Existing Java Methods

Source: https://toapuro.github.io/modding-notes/content/mixin

Demonstrates how to use Mixin to inject custom code into an existing Java method. It shows the original class, the Mixin class with an injection point, and the conceptual result after applying the Mixin. Requires the Mixin library.

```java
public class SomethingLogic {
    public void process() {
        System.out.println("Proceed");
    }
}
```

```java
// リマッピングオフでSomethingLogicにMixin
@Mixin(value = SomethingLogic.class, remap = false)
public class MixinSomethingLogic {

    // メソッドの最初に注入
    @Inject(method = "process()V", at = @At("HEAD"))
    private void onProcess(CallbackInfo ci) {
        System.out.println("Injected");
    }
}
```

```java
public class SomethingLogic {
    public void process() {
        onProcess(new CallbackInfo(...))
        System.out.println("Proceed");
    }

    private void onProcess(CallbackInfo ci) {
        System.out.println("Injected");
    }
}
```

--------------------------------

### Dependency Management with Repositories

Source: https://toapuro.github.io/modding-notes/content/advanced/gradle

Defines repositories and declares dependencies for the Minecraft mod. It includes setting up CurseMaven as a repository and adding Forge and JEI as dependencies. The `fg.deobf` method is used for deobfuscated dependencies.

```gradle
repositories {
    maven {
        url "https://cursemaven.com"
    }
}

dependencies {
    minecraft "net.minecraftforge:forge:${minecraft_version}-${forge_version}"

    implementation fg.deobf("curse.maven:jei-238222:7391695")
}

```

--------------------------------

### レジストリエントリの取得 - Java

Source: https://toapuro.github.io/modding-notes/content/registries/basic

特定のレジストリエントリをResourceLocationから取得する例です。BuiltInRegistriesクラスのgetメソッドを使用します。また、レジストリエントリからResourceLocationを取得したり、特定のIDが存在するかどうかを確認したりすることも可能です。

```java
// ResourceLocation -> Block
BuiltInRegistries.BLOCKS.get(ResourceLocation.fromNamespaceAndPath("minecraft", "stone"));

// Block -> ResourceLocation
BuiltInRegistries.BLOCKS.getKey(Blocks.STONE);

// 対象のエントリが存在するか
BuiltInRegistries.BLOCKS.containsKey(ResourceLocation.fromNamespaceAndPath("yourmod", "custom_block"))

```

--------------------------------

### Register Custom Renderer with Item (Java)

Source: https://toapuro.github.io/modding-notes/content/rendering/renderer/item

This Java code demonstrates how to register a custom item renderer with an item. It overrides the `initializeClient` method of the `Item` class and provides an implementation of `IClientItemExtensions` that returns the custom renderer instance. This ensures the custom renderer is used when the item is displayed.

```java
public class ExampleItem extends Item {
    public ExampleItem(Item.Properties properties) {
        super(properties);
    }

    @Override
    public void initializeClient(Consumer<IClientItemExtensions> consumer) {
        consumer.accept(
            new IClientItemExtensions() {
                private final BlockEntityWithoutLevelRenderer renderer = new ExampleItemRenderer(
                    Minecraft.getInstance().getBlockEntityRenderDispatcher(),
                    Minecraft.getInstance().getEntityModels()
                );

                @Override
                public BlockEntityWithoutLevelRenderer getCustomRenderer() {
                    return renderer;
                }
            }
        );
    }
}
```

--------------------------------

### Declare CurseMaven Dependencies in Gradle

Source: https://toapuro.github.io/modding-notes/content/getting-started/dependency

Shows how to declare dependencies using the CurseMaven format, specifying 'compileOnly' and 'runtimeOnly' for a mod like JEI. The format is 'curse.maven:<description>-<projectID>:<fileID>'.

```gradle
dependencies {
    // JEI の例 (Project ID: 238222, File ID: 4712866)
    compileOnly fg.deobf("curse.maven:jei-238222:4712866")
    runtimeOnly fg.deobf("curse.maven:jei-238222:4712866")
}
```

--------------------------------

### Gradle Plugin Configuration

Source: https://toapuro.github.io/modding-notes/content/advanced/gradle

Configures essential Gradle plugins for Eclipse, IntelliJ IDEA, Maven publishing, and ForgeGradle. ForgeGradle is crucial for automating tasks like downloading Minecraft sources, deobfuscation, and setting up development clients.

```gradle
plugins {
    id 'eclipse' // Eclipse IDE
    id 'idea' // Intellij IDEA
    id 'maven-publish' // Maven公開用
    id 'net.minecraftforge.gradle' version '[6.0,6.2)' // ForgeGradle
}

```

--------------------------------

### Accessing and Modifying Fields with @Shadow and @Mutable

Source: https://toapuro.github.io/modding-notes/content/mixin/injections

Demonstrates how to use the @Shadow annotation to access private fields within a Mixin class. It also shows how @Mutable allows modification of final fields, and warns against using final on @Shadow fields.

```java
@Shadow
private int exampleValue;

@Mutable
@Shadow @Final
private int finalExampleValue;
```

--------------------------------

### データパックレジストリの作成 - Java

Source: https://toapuro.github.io/modding-notes/content/registries/basic

データパックでエントリを追加できるレジストリを作成する例です。DataPackRegistryEvent.NewRegistryイベントを使用して、レジストリキーとエントリのCodecを指定して登録します。

```java
@SubscribeEvent // Modバスに登録
public static void registerDatapackRegistries(DataPackRegistryEvent.NewRegistry event) {
        event.dataPackRegistry(
                ResourceKey.createRegistryKey(CUSTOM_REGISTRY_ID),
                Spell.CODEC
        );
    }

```

--------------------------------

### Soft Override with Mixins in Java

Source: https://toapuro.github.io/modding-notes/content/mixin/tricks

Illustrates the concept of soft overrides using Mixins, which allows for a more robust inheritance system for Mixin methods. The `@SoftOverride` annotation is optional but recommended for validation. This technique is useful for avoiding conflicts when multiple mods modify the same methods.

```java
public class ExampleParent {
    protected void exampleMethod() {
        System.out.println("Original");
    }
}

public class Example extends ExampleParent {
}

@Mixin(ExampleParent.class)
public class ExampleParentMixin {

    @Inject(method = "exampleMethod", at = @At("HEAD"))
    protected void injectExampleMethod() {
    }

    @Unique
    protected void example$uniqueMethod() {
        System.out.println("Unique");
    }
}

@Mixin(Example.class)
public class ExampleMixin extends ExampleParentMixin {

    @Override
    @SoftOverride
    protected void injectExampleMethod() {
        System.out.println("Mixin");
    }

    @Override
    @SoftOverride
    protected void example$uniqueMethod() {
        System.out.println("Mixin");
    }
}
```

--------------------------------

### Generate Cube Model Quads with SimpleUnbakedGeometry

Source: https://toapuro.github.io/modding-notes/content/rendering/renderer/model

This Java code defines a custom `CubeModel` that extends `SimpleUnbakedGeometry` to automatically generate quads for a cube model. It includes a deserialization method to parse model data from JSON and an `addQuads` implementation to create the cube's faces using provided sprites and dimensions. This is useful for procedural model generation in Neoforge.

```java
public class CubeModel extends SimpleUnbakedGeometry<CubeModel> {

    public static final IGeometryLoader<CubeModel> LOADER = CubeModel::deserialize;

    // 別クラスに移動しても良い
    public static CubeModel deserialize(JsonObject json, JsonDeserializationContext context) {
        float size = GsonHelper.getAsFloat(json, "size");

        return new CubeModel(size);
    }

    private final float size;

    public CubeModel(float size) {
        this.size = size;
    }

    @Override
    protected void addQuads(IGeometryBakingContext context, IModelBuilder<?> modelBuilder, ModelBaker modelBaker, Function<Material, TextureAtlasSprite> spriteGetter, ModelState modelState, ResourceLocation modelLocation) {

        List<BlockElement> blockElements = new ArrayList<>();

        Map<Direction, BlockElementFace> faces = new EnumMap<>(Direction.class);

        Material baseLocation = context.getMaterial("base");
        TextureAtlasSprite baseSprite = spriteGetter.apply(baseLocation);
        SpriteContents contents = baseSprite.contents();

        for (Direction face : Direction.values()) {
            faces.put(
                    face,
                    new BlockElementFace(null, -1, baseLocation.texture().toString(), new BlockFaceUV(
                            new float[]{0f, 0f, contents.width(), contents.height()}, 0
                    ))
            );
        }

        blockElements.add(new BlockElement(
                new Vector3f(8f - size, 8f - size, 8f - size),
                new Vector3f(8f + size, 8f + size, 8f + size),
                faces,
                null,
                false
        ));

        UnbakedGeometryHelper.bakeElements(modelBuilder, blockElements, spriteGetter, modelState, modelLocation);
    }
}
```

--------------------------------

### Define Mod Properties in gradle.properties

Source: https://toapuro.github.io/modding-notes/content/getting-started/mod-files

The gradle.properties file is used to define configuration values for the build process, such as Mod ID, name, license, and version. These properties are often referenced in other build files like build.gradle.

```properties
minecraft_version=1.20.1
forge_version=47.4.10

mod_id=examplemod
mod_name=Example Mod
mod_license=All Rights Reserved
mod_version=1.0.0
```

--------------------------------

### Create Custom Creative Tab - Java

Source: https://toapuro.github.io/modding-notes/content/registries/item

Defines and registers a new custom creative tab. This involves using DeferredRegister for CreativeModeTab and configuring its title, icon, and the items it displays.

```java
public static final DeferredRegister<CreativeModeTab> CREATIVE_TABS =
        DeferredRegister.create(Registries.CREATIVE_MODE_TAB, ExampleMod.MODID);

public static final RegistryObject<CreativeModeTab> EXAMPLE_TAB = CREATIVE_TABS.register("example", () -> CreativeModeTab.builder()
  .title(Component.translatable("item_group." + ExampleMod.MODID + ".example"))
  .icon(() -> new ItemStack(ModItems.EXAMPLE_ITEM.get()))
  .displayItems((params, output) -> {
    output.accept(ModItems.EXAMPLE_ITEM.get());
    // ...
  })
  .build()
);

CREATIVE_TABS.register(modBus);
```

--------------------------------

### Add CurseMaven Repository to Gradle

Source: https://toapuro.github.io/modding-notes/content/getting-started/dependency

Configures the Gradle build to use the CurseMaven repository, which allows for easy integration of mods hosted on CurseForge as Maven dependencies.

```gradle
repositories {
    maven {
        url "https://cursemaven.com"
        content {
            includeGroup "curse.maven"
        }
    }
}
```

--------------------------------

### Mixin Inheritance in Java

Source: https://toapuro.github.io/modding-notes/content/mixin/tricks

Demonstrates how to use Mixins to inherit from a target class and implement its interfaces. This allows access to superclass methods and fields within the Mixin. Note that if the target is an abstract class, the Mixin must also be abstract and implement the abstract methods.

```java
@Mixin(Example.class)
public abstract class ExampleMixin extends ExampleParent implements ExampleInterface {

    public ExampleMixin(...) {
        super(...);
    }
}
```

--------------------------------

### Basic @Inject Usage in Mixin

Source: https://toapuro.github.io/modding-notes/content/mixin/injections

The @Inject annotation allows for inserting custom code at specific points within a target method. It requires specifying the target method using the 'method' parameter and the injection point using the @At annotation. The 'remap' parameter controls whether obfuscation mapping should be applied, and 'require' ensures a minimum number of matches for the injection point.

```java
@Inject(method = "methodName", at = @At("HEAD"), remap = false, require = 1)
```

--------------------------------

### Declare Local JAR Dependency in Gradle

Source: https://toapuro.github.io/modding-notes/content/getting-started/dependency

Shows how to declare a dependency on a local JAR file placed in the 'libs' folder. The format 'group:name:version' must be followed, with the filename matching 'name-version.jar'.

```gradle
dependencies {
    // libs/jei-1.20.1-forge-15.20.0.129.jar
    implementation fg.deobf("blank:jei-1.20.1-forge:15.20.0.129")
}
```

--------------------------------

### Specifying Injection Points with @At

Source: https://toapuro.github.io/modding-notes/content/mixin/injections

The @At annotation is used to specify the exact location within a method where code should be injected or modified. It accepts various values like HEAD, TAIL, RETURN, INVOKE, and FIELD, each targeting a different part of the method's execution flow. Optional parameters like 'remap', 'targets', 'priority', and 'ordinal' provide fine-grained control over the injection process.

```java
@At("HEAD")
@At(value = "INVOKE", target = "<descriptor>")
@At(value = "FIELD", target = "<descriptor>")
```

--------------------------------

### Configure Parchment Mappings in build.gradle

Source: https://toapuro.github.io/modding-notes/content/advanced/mapping

This snippet illustrates how to configure the Minecraft mappings channel and version within the `build.gradle` file for a Forge project. It switches the mapping channel from 'official' to 'parchment' and specifies a version compatible with Minecraft 1.20.1.

```gradle
minecraft {
    mappings channel: 'parchment', version: '2023.09.03-1.20.1'
}

```

--------------------------------

### Add Modrinth Maven Repository to Gradle

Source: https://toapuro.github.io/modding-notes/content/getting-started/dependency

Configures the Gradle build to use the Modrinth Maven repository for integrating mods from Modrinth. It includes an 'exclusiveContent' block to filter for 'maven.modrinth' group.

```gradle
repositories {
    exclusiveContent {
        forRepository {
            maven {
                name = "Modrinth"
                url = "https://api.modrinth.com/maven"
            }
        }
        forRepositories(fg.repository) // Only add this if you're using ForgeGradle, otherwise remove this line
        filter {
            includeGroup "maven.modrinth"
        }
    }
}
```

--------------------------------

### Custom Gradle Tasks for Modding

Source: https://toapuro.github.io/modding-notes/content/advanced/gradle

Defines custom Gradle tasks for processing resources, configuring the JAR manifest, setting up publishing, and ensuring UTF-8 encoding for Java compilation. The `processResources` task replaces template literals in `mods.toml` and `pack.mcmeta`, the `jar` task sets manifest attributes, and publishing is configured for a local Maven repository.

```gradle
/**
    mods.tomlにあるテンプレートリテラルを実際の値に置き換える処理
*/
tasks.named('processResources', ProcessResources).configure {
    var replaceProperties = [
            minecraft_version: minecraft_version, minecraft_version_range: minecraft_version_range,
            forge_version: forge_version, forge_version_range: forge_version_range,
            loader_version_range: loader_version_range,
            mod_id: mod_id, mod_name: mod_name, mod_license: mod_license, mod_version: mod_version,
            mod_authors: mod_authors, mod_description: mod_description,
    ]
    inputs.properties replaceProperties

    filesMatching(['META-INF/mods.toml', 'pack.mcmeta']) {
        expand replaceProperties + [project: project]
    }
}

/**
    Jarファイルのメタデータを設定
*/
tasks.named('jar', Jar).configure {
    manifest {
        attributes([
                'Specification-Title'     : mod_id,
                'Specification-Vendor'    : mod_authors,
                'Specification-Version'   : '1', // We are version 1 of ourselves
                'Implementation-Title'    : project.name,
                'Implementation-Version'  : project.jar.archiveVersion,
                'Implementation-Vendor'   : mod_authors,
                'Implementation-Timestamp': new Date().format("yyyy-MM-dd'T'HH:mm:ssZ")
        ])
    }

    finalizedBy 'reobfJar'
}

/**
    パブリッシング設定
*/
publishing {
    publications {
        register('mavenJava', MavenPublication) {
            artifact jar
        }
    }
    repositories {
        maven {
            url "file://${project.projectDir}/mcmodsrepo"
        }
    }
}

/**
    Javaコンパイル時のエンコーディングをUTF-8に設定
*/
tasks.withType(JavaCompile).configureEach {
    options.encoding = 'UTF-8'
}


```

--------------------------------

### Create Custom Item Renderer Class (Java)

Source: https://toapuro.github.io/modding-notes/content/rendering/renderer/item

This Java code defines a custom item renderer by extending `BlockEntityWithoutLevelRenderer`. It includes a constructor and an overridden `renderByItem` method for custom rendering logic. This class serves as the foundation for unique item visuals.

```java
class ExampleItemRenderer extends BlockEntityWithoutLevelRenderer {

    public ExampleItemRenderer(BlockEntityRenderDispatcher dispatcher, EntityModelSet modelSet) {
        super(dispatcher, modelSet);
    }

    @Override
    public void renderByItem(ItemStack itemStackIn, ItemDisplayContext type, PoseStack poseStack, MultiBufferSource bufferSource, int packedLight, int packedOverlay) {
        // レンダリング
    }
}
```

--------------------------------

### Configuring @Inject with Cancellable and Locals

Source: https://toapuro.github.io/modding-notes/content/mixin/injections

When using @Inject, the 'cancellable' parameter can be set to true to allow the injected code to cancel the original method's execution using CallbackInfo. The 'locals' parameter controls the capture of local variables, with options for no capture, soft failure, or hard failure.

```java
@Inject(method = "methodName", at = @At("HEAD"), cancellable = true, locals = LocalCapture.CAPTURE_FAILHARD)
```

--------------------------------

### Include Datagen Resources

Source: https://toapuro.github.io/modding-notes/content/advanced/gradle

Configures the main source set to include resources generated by the data generation process. This ensures that generated assets are available during the build.

```gradle
sourceSets.main.resources { srcDir 'src/generated/resources' }

```

--------------------------------

### Mixin Interface Implementation in Java

Source: https://toapuro.github.io/modding-notes/content/mixin/tricks

Explains how to implement custom interfaces using Mixins. Since the Mixin is merged into the target class, it can implement any interface. Using `@Unique` allows for adding and accessing arbitrary data within the class.

```java
public interface MyInterface {
    void example$exampleMethod();
    String example$getExampleValue();
}

@Mixin(Example.class)
public class ExampleMixin implements MyInterface {
    @Unique
    private String example$exampleValue = "Hello World";

    @Unique
    @Override
    public void example$exampleMethod() {
        System.out.println("Mixin");
    }

    @Unique
    @Override
    public String example$getExampleValue() {
        return example$exampleValue;
    }
}

// 使用法
Example example = ...;
MyInterface myInterface = (MyInterface) example;

myInterface.example$exampleMethod();
System.out.println(myInterface.example$getExampleValue());
```

--------------------------------

### Mixin Configuration: .mixins.json File

Source: https://toapuro.github.io/modding-notes/content/mixin

Defines the structure and content of the Mixin configuration file (.mixins.json). This file specifies required settings, compatibility level, package, and lists the Mixin classes to be applied, categorized for client, server, or both.

```json
{
  "required": true,
  "minVersion": "0.8",
  "package": "<groupId>.mixin",
  "compatibilityLevel": "JAVA_17",
  "mixins": [],
  "client": [],
  "server": [],
  "injectors": {
    "defaultRequire": 1
  }
}
```

```json
{
    "package": "io.github.toapuro.example.mixins",
    "mixins": [
        "ExampleMixin"
    ],
    "client": [],
    "server": []
}
```

--------------------------------

### Adjusting Injection Point with @Inject's shift

Source: https://toapuro.github.io/modding-notes/content/mixin/injections

The 'shift' parameter within the @At annotation, when used with @Inject, allows for fine-tuning the injection point relative to the specified location. Options include NONE (default), BEFORE, and AFTER, enabling injections immediately before or after the annotated point.

```java
@Inject(method = "methodName", at = @At(value = "INVOKE", shift = At.Shift.AFTER))
```

--------------------------------

### Configure Item Properties - Java

Source: https://toapuro.github.io/modding-notes/content/registries/item

Sets various properties for an item, such as stack size, durability, rarity, and repairability. These properties are defined using the Item.Properties class and its methods.

```java
public static final RegistryObject<Item> EXAMPLE_ITEM = ITEMS.register("example_item", () ->
        new Item(new Item.Properties().stacksTo(1).defaultDurability(1024).rarity(Rarity.UNCOMMON))
);
```

--------------------------------

### Declare Minecraft Forge Dependency in build.gradle

Source: https://toapuro.github.io/modding-notes/content/getting-started/mod-files

The build.gradle file defines the build process for the project. This snippet shows how to declare the Minecraft Forge dependency using variables defined in gradle.properties.

```gradle
dependencies {
    minecraft "net.minecraftforge:forge:${minecraft_version}-${forge_version}"
}
```

--------------------------------

### Using @Cancellable for CallbackInfo Manipulation

Source: https://toapuro.github.io/modding-notes/content/mixin/mixin-extras

The @Cancellable annotation provides a way to manipulate CallbackInfo, similar to @ModifyExpressionValue, even when the injection function does not explicitly take CallbackInfo as an argument. By adding `@Cancellable CallbackInfo ci` or `@Cancellable CallbackInfoReturnable<T> ci` as the last argument, you can control cancellation and return values.

```java
// Example usage with @Cancellable
@Inject(method = "someMethod", at = @At("HEAD"))
public void onMethodCall(@Cancellable CallbackInfo ci) {
    if (someCondition) {
        ci.cancel();
    }
}

@Inject(method = "anotherMethod", at = @At("RETURN"))
public void onMethodReturn(@Cancellable CallbackInfoReturnable<Boolean> cir) {
    if (anotherCondition) {
        cir.setReturnValue(false);
    }
}

```
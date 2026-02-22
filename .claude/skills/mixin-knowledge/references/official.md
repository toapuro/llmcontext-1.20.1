### Complete Mixin Gradle Closure Configuration Example

Source: https://github.com/spongepowered/mixin/wiki/Mixins-on-Minecraft-Forge

Illustrates a comprehensive `mixin` closure configuration in Gradle, covering refmap addition for source sets, config file inclusion for runs and jars, debug options for development, and annotation processor settings.

```groovy
mixin {
    // Refmaps for each SourceSet
    add sourceSets.main, 'mixins.mymod.refmap.json'

    // Configs to add to runs and jars
    config 'mixins.mymod.json'

    // Specify options for dev run configs
    debug.verbose = true
    debug.export = true
    dumpTargetOnFailure = true

    // Options for the Annotation Processor
    quiet
}
```

--------------------------------

### BeforeConstant expandZeroConditions Argument Usage Example

Source: https://github.com/spongepowered/mixin/wiki/Injection-Point-Reference

An example of how to specify multiple values for the 'expandZeroConditions' argument in the BeforeConstant injection point, using a comma-separated string of Condition types.

```Text
"expandZeroConditions=LESS_THAN_ZERO,GREATER_THAN_ZERO"
```

--------------------------------

### Java: Basic Bytecode Method Copy using ASM

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

This Java snippet demonstrates a simple, short example of copying a method from a source class to a destination class using the ASM library. It illustrates the ease of direct bytecode manipulation but highlights its inherent unsafety, as it performs no validation for conflicts or validity, requiring carefully crafted source methods.

```java
ClassNode source = new ClassNode();
ClassNode dest = new ClassNode();
new ClassReader(sourceBytes).accept(source);
new ClassReader(destBytes).accept(dest);
source.methods.stream().filter(m -> m.name.equals("foo")).forEach(dest.methods::add);
ClassWriter cw = new ClassWriter(0);
dest.accept(cw);
return cw.toByteArray();
```

--------------------------------

### Java Conditional Expression Compilation Example

Source: https://github.com/spongepowered/mixin/wiki/Injection-Point-Reference

Illustrates how a simple Java conditional expression involving zero might be compiled, showing the source code and a conceptual compiled form. This helps explain the need for the 'expandZeroConditions' argument in the BeforeConstant injection point.

```Java
if (x >= 0)
```

```Java
if (x.isGreaterThanOrEqualToZero())
```

--------------------------------

### Example Target Method for Mixin Injection

Source: https://github.com/spongepowered/mixin/wiki/Advanced-Mixin-Usage---Callback-Injectors

Defines a simple setPos method that takes two integer arguments. This method serves as a target for demonstrating how Mixin's @Inject can pass arguments from the target method to the handler.

```Java
/**
 * A target method, setPos
 */
public void setPos(int x, int y) {
    Point p = new Point(x, y);
    this.position = p;
    this.update();
}
```

--------------------------------

### Mixin Gradle Annotation Processor Configuration Example

Source: https://github.com/spongepowered/mixin/wiki/Mixins-on-Minecraft-Forge

Demonstrates how to configure Mixin's Annotation Processor (AP) options within the `mixin` closure in a Gradle build script, including disabling target validation, setting overwrite error levels, suppressing output, and adding custom mappings.

```groovy
mixin {
    // AP Settings
    disableTargetValidator = true
    overwriteErrorLevel = 'error'
    quiet
    extraMappings file("my_custom_srgs.tsrg")
}
```

--------------------------------

### Apply Mixin Constraint with Inclusive Range

Source: https://github.com/spongepowered/mixin/wiki/Introduction-to-Mixins---Overwriting-Methods

This Java example demonstrates defining a constraint that allows a range of values for a token. The BUILD(1230-1240) constraint ensures the 'BUILD' token's value is between 1230 and 1240, inclusive. This provides flexibility for mixins that can tolerate minor version differences.

```Java
@Overwrite(constraints = "BUILD(1230-1240)")
```

--------------------------------

### Add Mixin Annotation Processor Dependency to build.gradle

Source: https://github.com/spongepowered/mixin/wiki/Mixins-on-Minecraft-Forge

Adds the Mixin Annotation Processor (AP) as an `annotationProcessor` dependency to the project's `dependencies` block in `build.gradle`. The `processor` classifier ensures that all necessary upstream dependencies for the AP are included automatically, simplifying setup.

```Groovy
dependencies {
    // Specify the version of Minecraft to use. If this is any group other than 'net.minecraft', it is assumed
    // that the dep is a ForgeGradle 'patcher' dependency, and its patches will be applied.
    // The userdev artifact is a special name and will get all sorts of transformations applied to it.
    minecraft 'net.minecraftforge:forge:1.17.1-37.0.70'

    // Apply Mixin AP
    annotationProcessor 'org.spongepowered:mixin:0.8.5:processor'
}
```

--------------------------------

### Mixin Type Cast Optimization Example

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

Illustrates how Mixin handles type casts to a known target class, specifically addressing the Java compiler's limitation of direct casts to classes it 'knows' will fail. Mixin detects and optimizes these double-casts for efficiency, resulting in code equivalent to a direct call.

```Java
((TargetClass)(Object)this).somePublicMethod()
```

--------------------------------

### Java: Implement Cancellable Method Injection with Mixin @Inject

Source: https://github.com/spongepowered/mixin/wiki/Advanced-Mixin-Usage---Callback-Injectors

Demonstrates a cancellable injection using the Mixin framework. By setting `cancellable = true` in the `@Inject` annotation and calling `ci.cancel()` within the handler, the target method (`setPos`) can be made to return prematurely. This example shows custom logic for a specific condition (position (0,0)) and how to short-circuit the original method's execution, effectively creating a Short Circuit Injection. Calling `cancel()` on a non-cancellable `CallbackInfo` will throw an exception.

```Java
/**
 * Cancellable injection, note that we set the "cancellable"
 * flag to "true" in the injector annotation
 */
@Inject(method = "setPos", at = @At("HEAD"), cancellable = true)
protected void onSetPos(int x, int y, CallbackInfo ci) {
    // Check whether setting position to origin and do some custom logic
    if (x == 0 && y == 0) {
        // Some custom logic
        this.position = Point.ORIGIN;
        this.handleOriginPosition();
        
        // Call update() just like the original method would have
        this.update();
        
        // Mark the callback as cancelled
        ci.cancel();
    }
    
    // Execution proceeds as normal at this point, no custom handling
}
```

--------------------------------

### Mixin: Shadowing Fields and Methods for Invocation

Source: https://github.com/spongepowered/mixin/wiki/Introduction-to-Mixins---Understanding-Mixin-Architecture

This Java code extends the previous example by demonstrating how to use the @Shadow annotation for both fields (level) and methods (update()). It shows how a mixin can declare a virtual method to invoke a private or protected method from its target class (EntityPlayer), allowing for more complex interactions with the target's internal state and behavior. The shadowed method is declared abstract or with an empty body if private.

```Java
@Mixin(EntityPlayer.class)
public abstract class MixinEntityPlayer
    extends Entity
    implements LivingThing, Leveller {
    
    @Shadow
    private int level;
    
    @Shadow
    private void update() {}
    
    @Override
    public void setLevel(int newLevel) {
        // Set the level value
        this.level = newLevel;
        
        // Invoke the shadowed method to update the object state
        this.update();
    }
}
```

--------------------------------

### Activating Mixin Configurations

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

Explains how Mixin configurations are processed and activated within the transformer, ensuring priority order.

```APIDOC
MixinConfig:
  Description: Configurations that survive preparation are added to the transformer's active collection and re-sorted for priority.
```

--------------------------------

### API Documentation for Mixin Injection Process Classes

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

This documentation outlines the core classes and their responsibilities during the Mixin pre-injection application stage, focusing on how injection information is parsed, targets are located, and references are mapped.

```APIDOC
Class: InjectionInfo
  Purpose: Parses injection opcodes into structs, instances the correct Injector, handles injection-type-specific parsing, extracts information from injector annotations, and locates candidate target methods.
  Source: https://github.com/SpongePowered/Mixin/blob/master/src/main/java/org/spongepowered/asm/mixin/injection/struct/InjectionInfo.java

Class: Injector
  Purpose: Performs the actual code manipulation during the injection process.
  Source: https://github.com/SpongePowered/Mixin/blob/master/src/main/java/org/spongepowered/asm/mixin/injection/code/Injector.java

Class: MethodSlice
  Purpose: Represents a defined slice within a method, parsed and held by MethodSlices. Can be retrieved by ID for targeted searching.
  Source: https://github.com/SpongePowered/Mixin/blob/master/src/main/java/org/spongepowered/asm/mixin/injection/code/MethodSlice.java

Class: MethodSlices
  Purpose: A holder for parsed MethodSlice instances, managing method slices for injection targeting.
  Source: https://github.com/SpongePowered/Mixin/blob/master/src/main/java/org/spongepowered/asm/mixin/injection/code/MethodSlices.java

Class: MemberInfo
  Purpose: Structs used to parse string references from annotations. These are then used as discriminators to locate matching methods within a class.
  Source: https://github.com/SpongePowered/Mixin/blob/master/src/main/java/org/spongepowered/asm/mixin/injection/struct/MemberInfo.java

Class: ReferenceMapper
  Purpose: Utilized during MemberInfo parsing to convert compiled-in strings to their obfuscated counterparts using a compile-time generated refMap.
  Source: https://github.com/SpongePowered/Mixin/blob/master/src/main/java/org/spongepowered/asm/mixin/refmap/ReferenceMapper.java

Class: InjectionPoint
  Purpose: Instances parsed from annotations that are later used to match specific opcodes within the discovered target methods, defining the exact location for injection.
  Source: https://github.com/SpongePowered/Mixin/blob/master/src/main/java/org/spongepowered/asm/mixin/injection/InjectionPoint.java
```

--------------------------------

### Mixin Main Application Stage Process Overview

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

Describes the comprehensive process of the Mixin framework's main application stage, detailing how mixin code is merged into a target class. This includes handling generic signatures, interfaces, class attributes, annotations, fields, and methods, with specific attention to method transformation and special considerations like supermixins and type casts.

```APIDOC
Main Application Stage:
  Purpose: Processes the majority of mixin application logic, merging prepared mixin code into the target class.
  Tasks:
    - Merging and applying generic signature:
      - Blends declared signature elements of mixin with target class.
      - Uses ClassSignature structs and ClassInfo metaobjects.
    - Merging interfaces:
      - Adds 'implements' clause for each interface declared on mixins.
    - Merging class attributes:
      - Includes class version and source file (e.g., Java 6/8 compatibility).
    - Merging annotations (class level):
      - @Mixin annotation is discarded.
      - Most other annotations are merged onto the target.
    - Merging fields:
      - Most fields are added directly.
      - Shadow fields are discarded, but their annotations are merged.
    - Merging methods (most involved step):
      - Requires specialisation before merging.
      - Respects contractual obligations established by mixin.
      - Method Transformation:
        - Handled by MixinTargetContext.
        - Walks through opcodes, changing references to mixin class (method/field accesses) into references to the new target class.
        - Uses ClassInfo for lookups of members outside the target class (may cause side-loading).
        - Applies renames to actual calls.
      - Type Casts and Supermixins:
        - References to supermixin within mixin code are transformed to the target of that mixin in the context of the current target.
        - Example: MixinZ accessing field F in MixinX, when applied to target C, transforms to A.F if A is superclass of MixinZ in context of C.
        - Detects and removes double-casts like ((TargetClass)(Object)this).somePublicMethod() for efficiency.
      - Method Applicator:
        - Methods generally displace their targets.
        - @Final: Prevents further overwrite operations.
        - @Overwrite: Subsequent overwrites log a warning.
        - Synthetic bridge methods: Compared for equivalence; merged if match, error if not.
```

--------------------------------

### Mixin Configuration File Schema

Source: https://github.com/spongepowered/mixin/wiki/Introduction-to-Mixins---The-Mixin-Environment

Defines the structure and available properties for a Mixin configuration file, including core mixin set definitions and optional advanced settings.

```APIDOC
Mixin Configuration File:
  package: string
    description: Defines the parent package for this group of mixins. The package and all subpackages will be excluded from the LaunchClassLoader at run time.
  mixins: array<string>
    description: Defines the list of mixin "classes" within the parent package to apply for this configuration. Each mixin "class" is specified relative to the parent package; sub-packages are allowed. Entries apply to both client and dedicated server.
  client: array<string>
    description: Defines the list of mixins to apply only on the client side.
  server: array<string>
    description: Defines the list of mixins to apply only on the dedicated server side.
  refmap: string (optional)
    description: Defines the name of the reference map filename for this set.
  priority: integer (optional)
    description: Defines the priority of this mixin set relative to other configurations.
  plugin: string (optional)
    description: The name of an optional companion plugin class for the mixin configuration which can tweak the mixin configuration programmatically at run time.
  required: boolean (optional)
    description: Defines whether the mixin set is required or not. If a single mixin failing to apply should be considered a failure state for the entire game, then set to true.
  minVersion: string (optional)
    description: Should be set if this mixin set relies on some mixin functionality which was added in a particular version. Can be omitted for version-agnostic mixin sets.
  setSourceFile: boolean (optional)
    description: Causes the mixin processor to overwrite the source file property in target classes with the source file of the mixin class. Useful for debugging mixins.
  verbose: boolean (optional)
    description: Promotes all DEBUG-level log messages to INFO level for this mixin set. Can also be enabled globally via the mixin.debug.verbose system property.
```

--------------------------------

### Demonstrate Usage of Mixin-Applied Interface

Source: https://github.com/spongepowered/mixin/wiki/Introduction-to-Mixins---Understanding-Mixin-Architecture

This Java code demonstrates how to utilize an interface (`LivingThing`) applied to `EntityPlayer` via a Mixin. It shows that the target class can now be cast to the new interface, enabling new method calls and interactions.

```Java
public void method() {
    EntityPlayer player = new EntityPlayer();

    // With the mixin applied, this cast succeeds
    LivingThing living = (LivingThing)player;

    // And because the cast succeeded, we can pass our object
    // to other things that require a reference to LivingThing
    // such as the method below
    if (this.isAlive(living)) {
        // hooray
    }
}

public boolean isAlive(LivingThing living) {
    // We can call getHealth() just fine, because the method
    // exists and is accessible via the LivingThing interface
    int health = living.getHealth();
    return health > 0;
}
```

--------------------------------

### Mixin Framework Core Classes and Their Interactions

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

This section details the roles and interactions of key classes within the Mixin framework's injection process, including InjectionInfo, Target, Injector, TargetNode, and InjectionNode, explaining how they collaborate to identify, wrap, and manage target instructions for bytecode manipulation.

```APIDOC
InjectionInfo:
  Purpose: Manages the overall injection process for a specific injector.
  Methods:
    - visits queued targets
    - stores InjectionNode objects
Target:
  Purpose: A method wrapper that tracks changes made by injectors and manages injection nodes.
  Source: Obtained from TargetClassContext.
  Properties:
    - keeps track of max-stack and max-locals changes
  Methods:
    - creates InjectionNode for each nominated node
    - stores InjectionNode objects
Injector:
  Purpose: Executes injection points on target methods.
  Methods:
    find(target: Target, injection_points: List[InjectionPoint]):
      - Runs injection points on the target.
      - Wraps each discovered ASM AbstractInsnNode in a TargetNode.
TargetNode:
  Purpose: Wraps an ASM AbstractInsnNode, allowing it to be decorated with metadata.
  Properties:
    - wraps AbstractInsnNode
    - decorated with nominating injection points
InjectionNode:
  Purpose: Represents a unique handle to an underlying instruction, tracking its replacement status.
  Source: Created by Target.
  Properties:
    - tracks when the instruction is replaced by an injector
TargetClassContext:
  Purpose: Provides context for target classes, used to obtain Target wrappers.
AbstractInsnNode:
  Purpose: Represents an instruction node in ASM bytecode.
  Usage: Wrapped by TargetNode.
```

--------------------------------

### MethodHead Injection Point

Source: https://github.com/spongepowered/mixin/wiki/Injection-Point-Reference

Documents the MethodHead injection point. Short name: HEAD, Full name: org.spongepowered.asm.mixin.injection.points.MethodHead. It has no options or arguments. This injection point always returns the first instruction in the candidate list, which is the first instruction in the method or slice. It is guaranteed to select only 1 instruction.

```APIDOC
InjectionPoint: MethodHead
  Short name: HEAD
  Full name: org.spongepowered.asm.mixin.injection.points.MethodHead
  Options: None
  Arguments: None
  Description: This injection point always returns the first instruction in the candidate list. When selecting within a method, this is always the first instruction in the method. When selecting in a slice, this is always the first instruction in the slice.
  Notes: This injection point is guaranteed to select only 1 instruction.
```

--------------------------------

### Mixin Configuration Properties Reference

Source: https://github.com/spongepowered/mixin/wiki/Mixin-Java-System-Properties

A comprehensive reference of configuration properties for the Mixin framework, detailing their purpose and impact on mixin processing.

```APIDOC
mixin.debug.dumpBytecode: Enables dumping of incoming (un-mixed-in) class bytecode to disk upon InvalidMixinException or other runtime failures, allowing inspection with javap or a Java Disassembler to determine the cause of mismatch.
```

```APIDOC
mixin.checks: Enables all mixin check operations.
```

```APIDOC
mixin.checks.interfaces: Enables Interface Implementation Audit Mode. Outputs an audit report for every applied mixin, summarizing unimplemented interface methods that would cause an AbstractMethodError. Report is sent to STDERR and flatfiles in .mixin.out directory.
```

```APIDOC
mixin.checks.interfaces.strict: When mixin.checks.interfaces is enabled, applies implementation checks to abstract target classes (default: true). Setting to false skips abstract targets in the report.
```

```APIDOC
mixin.ignoreConstraints: Disables constraint checking, demoting violations from fatal errors to warnings. Useful for development or testing out-of-band targets.
```

```APIDOC
mixin.hotSwap: Enables the hot-swap agent.
```

```APIDOC
mixin.env: Parent for environment-related settings. Not a configurable setting itself (always false).
```

```APIDOC
mixin.env.obf: Forces refmap obfuscation type when required (always false).
```

```APIDOC
mixin.env.disableRefMap: Disables refmap when required.
```

```APIDOC
mixin.env.remapRefMap: Enables runtime remapping of existing refMaps. Requires mixin.env.refMapRemappingFile and mixin.env.refMapRemappingEnv. Automatically enabled if loading via GradleStart.
```

```APIDOC
mixin.env.refMapRemappingFile: Specifies the SRG file name for mappings when mixin.env.remapRefMap is enabled. Mappings must have 'searge' source type and target current development environment, or mixin.env.refMapRemappingEnv must specify the source type.
```

```APIDOC
mixin.env.refMapRemappingEnv: Overrides the default 'searge' source environment when using mixin.env.refMapRemappingFile. The specified environment type must exist in the original refmap.
```

```APIDOC
mixin.env.ignoreRequired: Globally ignores the 'required' attribute of all configurations.
```

```APIDOC
mixin.env.compatLevel: Sets the default compatibility level for Mixin operations.
```

```APIDOC
mixin.env.shiftByViolation: Defines behavior (currently 'warn') when the maximum At.by value is exceeded in a mixin. May be promoted to 'error' in future versions.
```

--------------------------------

### BeforeFinalReturn Injection Point

Source: https://github.com/spongepowered/mixin/wiki/Injection-Point-Reference

Documents the BeforeFinalReturn injection point. Short name: TAIL, Full name: org.spongepowered.asm.mixin.injection.points.BeforeFinalReturn. It has no options or arguments. This injection point returns the final RETURN opcode in the target method. Note that the last RETURN opcode may not correspond to the notional 'bottom' of a method in the original Java source due to conditional expressions.

```APIDOC
InjectionPoint: BeforeFinalReturn
  Short name: TAIL
  Full name: org.spongepowered.asm.mixin.injection.points.BeforeFinalReturn
  Options: None
  Arguments: None
  Description: This injection point returns the final RETURN opcode in the target method.
  Notes: Note that the last RETURN opcode may not correspond to the notional "bottom" of a method in the original Java source, since conditional expressions can cause the bytecode emitted to differ significantly in order from the original Java.
```

--------------------------------

### Mixin Gradle Annotation Processor Options

Source: https://github.com/spongepowered/mixin/wiki/Mixins-on-Minecraft-Forge

Defines the available configuration options for the Mixin Annotation Processor within the `mixin` closure in Gradle, including their types and descriptions.

```APIDOC
Mixin Annotation Processor Options:
  disableTargetValidator: boolean
    description: Disables the target validator (validates that the mixin targets are sane (eg. superclass exists within target hierarchy))
  disableOverwriteChecker: boolean
    description: Disables the overwrite checker which ensures @Overwrite method javadoc contains @author and @reason tags
  overwriteErrorLevel: String
    description: Sets the error level for the overwrite checker (defaults to warning, can be set to error)
  quiet: boolean (flag)
    description: Suppresses the banner message and informational output from the AP
  extraMappings: String (file path)
    description: Specifies the name of an additional (custom) TRSG mapping file to feed to the AP, this can be used for mapping entries not specified in the main mapping file
```

--------------------------------

### Java Method Signature and Bytecode Descriptors

Source: https://github.com/spongepowered/mixin/wiki/Introduction-to-Mixins---Understanding-Mixin-Architecture

This snippet illustrates the definition of a Java method's signature, its conceptual representation, and its compact bytecode descriptor format. The first code block shows a standard Java method declaration. The second shows its conceptual signature format (parameters and return type). The third shows the compact bytecode descriptor, which is essential for working with Injectors.

```Java
public ThingType getThingAtLocation(double scale, int x, int y, int z, boolean squash) {
```

```Java
(double,int,int,int,boolean)com.mypackage.ThingType
```

```Bytecode
(DIIIZ)Lcom/mypackage/ThingType;
```

--------------------------------

### Implement Mixin ITokenProvider for Custom Constraints

Source: https://github.com/spongepowered/mixin/wiki/Introduction-to-Mixins---Overwriting-Methods

This Java code demonstrates how to implement the ITokenProvider interface to supply custom token values to the Mixin environment. The getToken method returns the application's build number for the 'BUILD' token, returning null for unsupported tokens. This provider must be registered with MixinEnvironment during bootstrapping.

```Java
public class MyTokenProvider implements ITokenProvider {
    public Integer getToken(String token) {
        if ("BUILD".equals(token)) {
            return TargetApplication.getInstance().getBuildNumber();
        }
        return null;
    }
}
```

--------------------------------

### BeforeConstant Injection Point API Reference

Source: https://github.com/spongepowered/mixin/wiki/Injection-Point-Reference

Detailed API specification for the BeforeConstant injection point, including its short name, full class name, available options, and a comprehensive list of arguments for matching different constant types within method bodies. Special attention is given to the 'expandZeroConditions' argument, which handles zero literals in conditional expressions.

```APIDOC
Injection Point: BeforeConstant
Short Name: CONSTANT
Full Name: org.spongepowered.asm.mixin.injection.points.BeforeConstant

Options:
  ordinal: int
    Description: The ordinal index of the NEW opcode to select (or all if omitted).

Arguments:
  nullValue: boolean
    Description: Set to true to match null literals in the method body.
  intValue: int
    Description: Set to an integer literal to match in the method body. Note that it may be necessary to also set the expandZeroConditions argument if you wish to match literal zeroes which form part of a conditional expression.
  floatValue: float
    Description: Set to a float literal to match in the method body.
  longValue: long
    Description: Set to a long literal to match in the method body.
  doubleValue: double
    Description: Set to a double literal to match in the method body.
  stringValue: String
    Description: Set to a literal String value to match in the method body.
  class: String
    Description: Set to a fully-qualified class name to match a Class literal in the method body.
  log: boolean
    Description: Set to true to emit logging information when applying this injection point query.
  expandZeroConditions: String
    Description: Specifies one or more values for this argument based on the type of expression you wish to expand. Values are Condition types separated by commas or spaces. This handles special cases where zero is used in conditional expressions and Java compiles them inversely. It implies an intValue of 0 unless explicitly defined.
```

--------------------------------

### Mixin @Inject Handler Method with Target Arguments and CallbackInfo

Source: https://github.com/spongepowered/mixin/wiki/Advanced-Mixin-Usage---Callback-Injectors

Illustrates how to pass arguments from the target method (setPos) to the handler method (onSetPos). The handler method's signature includes the target method's arguments (x, y) before the CallbackInfo, allowing the handler to access these values.

```Java
/**
 * Handler method, onSetPos. Note the two int variables x and y
 * which appear before the callbackinfo
 */
@Inject(method = "setPos", at = @At("HEAD"))
protected void onSetPos(int x, int y, CallbackInfo ci) {
    System.out.printf("Position is being set to (%d, %d)\n", x, y);
}
```

--------------------------------

### Complete build.gradle Plugins Block with MixinGradle

Source: https://github.com/spongepowered/mixin/wiki/Mixins-on-Minecraft-Forge

Illustrates the full `plugins` block in `build.gradle` after incorporating the MixinGradle plugin. This block typically includes other Forge-related plugins like Eclipse, Maven Publish, and ForgeGradle itself, defining the project's build environment.

```Groovy
plugins {
    id 'eclipse'
    id 'maven-publish'
    id 'net.minecraftforge.gradle' version '5.1.+'
    id 'org.spongepowered.mixin' version '0.7.+'
}
```

--------------------------------

### Mixin Constraint String Specifiers Reference

Source: https://github.com/spongepowered/mixin/wiki/Introduction-to-Mixins---Overwriting-Methods

This section provides a comprehensive reference for the various string specifiers used within Mixin constraints. These specifiers define how token values are evaluated against conditions, allowing for exact matches, ranges, and comparisons. Understanding these specifiers is crucial for precisely controlling when mixins are applied.

```APIDOC
Constraint String:
  (): Token value must be present, any value
  (1234): Token value must be exactly 1234
  (1234+), (1234-), (1234>): 1234 or greater
  (<1234): Less than 1234
  (<=1234): Less than or equal to 1234 (equivalent to 1234<)
  (>1234): Greater than 1234
  (>=1234): Greater than or equal to 1234 (equivalent to 1234>)
  (1234-1300): Value must be between 1234 and 1300 (inclusive)
  (1234+10): Value must be between 1234 and 1234+10 (1234-1244 inclusive)
```

--------------------------------

### Basic Mixin @Inject Annotation for Method Head Injection

Source: https://github.com/spongepowered/mixin/wiki/Advanced-Mixin-Usage---Callback-Injectors

Demonstrates the basic usage of the @Inject annotation to inject a handler method (onUpdate) at the HEAD of the target update method. This shows how to non-destructively add logic at the beginning of a method.

```Java
@Inject(method = "update", at = @At("HEAD"))
protected void onUpdate() {
    Observer.instance.foo(this);
}
```

--------------------------------

### Apply New Interface to Target Class with Mixin

Source: https://github.com/spongepowered/mixin/wiki/Introduction-to-Mixins---Understanding-Mixin-Architecture

This snippet shows how to apply a new interface, `LivingThing`, to the `EntityPlayer` target class at runtime by simply declaring it on the Mixin class. This allows for monkey-patching new behaviors onto existing classes.

```Java
@Mixin(EntityPlayer.class)
public abstract class MixinEntityPlayer
    extends Entity
    implements LivingThing {
}
```

--------------------------------

### Initial Intrinsic Proxy Method Attempt (Problematic)

Source: https://github.com/spongepowered/mixin/wiki/Introduction-to-Mixins---Overwriting-Methods

This Java code snippet demonstrates an initial attempt at creating an intrinsic proxy method using @Shadow and a soft implementation. It illustrates the conflict that arises after mixin application and the self-referential call leading to a stack overflow.

```Java
@Mixin(Foo.class)
@Implements(@Interface(iface = Indentifyable.class, prefix = "id$"))
public abstract class MixinFoo {

    @Shadow
    public abstract int getID();

    /**
     * This method will become our intrinsic proxy method, it
     * calls the original (shadowed) version of the accessor.
     */
    public int id$getID() {
        // Call original accessor
        return this.getID();
    }
}
```

--------------------------------

### APIDOC: InjectionNode Class

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

Documents the InjectionNode class, used by injectors to marshal metadata to each other. It can be decorated with replacement instructions and allows for dynamic decisions by later injectors, such as handling priority semantics for @Redirect.

```APIDOC
InjectionNode:
  Purpose: Represents a node in the injection process, used to marshal metadata between injectors.
  Details: Can be decorated with replacement instructions. Allows injectors to dynamically decide based on prior replacements or metadata (e.g., priority).
  Source: https://github.com/SpongePowered/Mixin/blob/master/src/main/java/org/spongepowered/asm/mixin/injection/struct/InjectionNodes.java#L53
```

--------------------------------

### Configure Main SourceSet Refmap and Mixin Config

Source: https://github.com/spongepowered/mixin/wiki/Mixins-on-Minecraft-Forge

This Groovy snippet shows how to configure the MixinGradle `mixin` extension to specify the reference map name for the `main` source set and the primary mixin configuration file. The `add` method links a source set to its corresponding reference map, while the `config` method registers the main mixin configuration, which will be injected into run configurations and added to obfuscated jar manifests.

```groovy
mixin {
    // MixinGradle Settings
    add sourceSets.main, 'mixins.mymod.refmap.json'
    config 'mixins.mymod.json'
}
```

--------------------------------

### Define MixinGradle Configuration Block

Source: https://github.com/spongepowered/mixin/wiki/Mixins-on-Minecraft-Forge

This Groovy snippet illustrates the basic structure for defining the `mixin` extension block in a Gradle build script. This block serves as the central place to configure various options for the MixinGradle plugin. It is recommended to place this configuration after the `sourceSets` or `dependencies` blocks for better organization.

```groovy
mixin {
    // MixinGradle Settings
}
```

--------------------------------

### Mixin Java System Properties Reference

Source: https://github.com/spongepowered/mixin/wiki/Mixin-Java-System-Properties

A comprehensive list of Java System Properties used to configure Mixin's runtime behavior, including debugging, bytecode export, verification, and strictness settings. Each property can be set to 'true' to enable its corresponding feature.

```APIDOC
Property: mixin.debug
  Description: Enables all mixin debug options
Property: mixin.debug.export
  Description: The export debug option causes the mixin processor to emit post-mixin bytecode to disk for all mixin targets. The bytecode data are output to a typical package/class structure under the .mixin.out directory under your run directory. Having the fernflower jar on your runtime classpath will also cause these class files to be decompiled.
Property: mixin.debug.export.filter
  Description: Export filter, if omitted allows all transformed classes to be exported. If specified, acts as a filter for class names to export and only matching classes will be exported. This is useful when using Fernflower as exporting can be otherwise very slow. The following wildcards are allowed:
    *: Matches one or more characters except dot (.)
    **: Matches any number of characters
    ?: Matches exactly one character
Property: mixin.debug.export.decompile
  Description: Set to false for fernflower to be disabled even if it is found on the classpath.
Property: mixin.debug.export.decompile.async
  Description: Run fernflower in a separate thread. In general this will allow export to impact startup time much less (decompiling normally adds about 20% to load times) with the trade-off that crashes may lead to undecompiled exports.
Property: mixin.debug.verify
  Description: The verify option runs ASM's CheckClassAdapter on the post-mixin bytecode in order to check that mixin transformations have been applied correctly. This option is only intended for use when working on the Mixin library itself and it is not recommended to enable it during general debugging of mixins themselves.
Property: mixin.debug.verbose
  Description: The verbose option promotes all DEBUG-level logging messages generated by the mixin processor to INFO level so they are emitted to the console at runtime. This is a useful option to enabled when developing with mixins as it allows more interactive monitoring of the mixin application process.
Property: mixin.debug.countInjections
  Description: Elevates failed injections to an error condition; see Inject.expect for details.
Property: mixin.debug.strict
  Description: Enables strict checks.
Property: mixin.debug.strict.unique
  Description: If false (default), Unique public methods merely raise a warning when encountered and are not merged into the target. If true, an exception is thrown instead.
Property: mixin.debug.strict.targets
  Description: Enable strict checking for mixin targets.
Property: mixin.debug.profiler
  Description: Enable the performance profiler for all mixin operations (normally it is only enabled during mixin prepare operations).
Property: mixin.dumpTargetOnFailure
  Description: 
```

--------------------------------

### BeforeStringInvoke Injection Point API

Source: https://github.com/spongepowered/mixin/wiki/Injection-Point-Reference

Similar to BeforeInvoke, this specialized injection point targets invoke instructions, but specifically matches methods that accept a single String argument and return void, where the string itself is a literal. It's primarily used for methods like Profiler::startSection.

```APIDOC
Full Name: org.spongepowered.asm.mixin.injection.points.BeforeStringInvoke
Short Name: INVOKE_STRING

Options:
  target: string
    Description: the invocation to target, must be fully-qualified if remapped
  ordinal: integer
    Description: the ordinal index of the matching invocation to select (selects all if omitted)

Arguments:
  ldc: string
    Description: constant value to match, see description
  log: boolean
    Description: produces verbose output in the console as the injector scans a method, useful for diagnosing injectors which are behaving incorrectly or in unexpected ways

Description: Like [BeforeInvoke](#invoke), this injection point searches for *invoke* instructions within the target instruction list and returns instructions which match the criteria. This specialised version will only match methods which accept a single `String` argument and return `void`, the string itself must be a literal string and is matched as part of the query process. The primary purpose of this query is to match specific invocations of `Profiler::startSection` and similar methods which accept a literal string. The string constant to match should be specified using the named argument `ldc`.

Notes: This query can only be used to match invocations where the argument is passed as a `String` literal. See notes in [BeforeInvoke](#invoke) regarding the application order of `target` and `ordinal`.
```

--------------------------------

### Handling Inner Classes in Mixins

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

Describes the delicate process of handling inner classes within mixins, including the reconstruction of synthetic scaffolding and the role of the InnerClassGenerator in conforming class references and synthesizing bytecode. Also mentions the optimization for pure synthetic inner classes handled by MixinPostProcessor.

```APIDOC
InnerClassGenerator:
  Description: Synthesizes bytecode for conformed inner classes by reading original bytecode and transforming mixin references to target classes. Reconstructs synthetic scaffolding and creates unique copies of inner classes for each target.
  Purpose: To handle inner classes in mixins by conforming their references to the target class and generating unique bytecode for each target.
MixinPostProcessor:
  Description: Handles pure synthetic inner classes (e.g., for switch lookups) by passing their bytecode through as-is, reducing processing burden for static synthetic classes.
```

--------------------------------

### Injecting into All Constructors with Wildcard Target

Source: https://github.com/spongepowered/mixin/wiki/Advanced-Mixin-Usage---Callback-Injectors

Demonstrates how to use a wildcard (`*`) with the method name `<init>` to target all constructors of a class. This allows a single injector to apply to multiple overloaded constructors, useful for observer-type initializations. Note that argument capture is not possible with wildcard targets.

```Java
@Inject(method = "<init>*", at = @At("RETURN"))
private void onConstructed(CallbackInfo ci) {
    // do initialisation stuff
}
```

--------------------------------

### Mixin Pre-defined Injection Points Reference

Source: https://github.com/spongepowered/mixin/wiki/Advanced-Mixin-Usage---Callback-Injectors

This API documentation outlines the various pre-defined injection points in Mixin, specifying the type of code they identify and their injection behavior. These points allow for precise bytecode manipulation.

```APIDOC
INVOKE:
  Description: Finds a method call and injects before it
FIELD:
  Description: Finds a field read or write and injects before it
NEW:
  Description: Finds a NEW opcode (object creation) and injects before it
JUMP:
  Description: Finds a jump opcode (of any type) and injects before it
INVOKE_STRING:
  Description: Finds a call to a method which takes a single String and returns void which accepts a constant string as an argument. This can be used primarily to find calls to Profiler.startSection( nameOfSection )
INVOKE_ASSIGN:
  Description: Finds a method call which returns a value and injects immediately after the value is assigned to a local variable. Note this is the only Injection Point which injects after its target
```

--------------------------------

### Enable Mixin Debugging Options in MixinGradle

Source: https://github.com/spongepowered/mixin/wiki/Mixins-on-Minecraft-Forge

This Groovy snippet demonstrates how to enable common debugging options for Mixin within the `mixin` extension block. It shows how to set `debug.verbose` to `true` for detailed logging and `debug.export` to `true` to export transformed classes, which are highly recommended during development for troubleshooting. These settings are applied conveniently within the main MixinGradle configuration.

```groovy
mixin {
    // MixinGradle Settings
    add sourceSets.main, 'mixins.mymod.refmap.json'
    config 'mixins.mymod.json'

    debug.verbose = true
    debug.export = true
}
```

--------------------------------

### APIDOC: MixinInfo Structure and Purpose

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

Documents the MixinInfo struct, which is central to the Mixin parsing process. It describes its role in loading and initially parsing mixins, and how it acts as a container for all generated metadata, including references to State, ClassInfo, and SubType.

```APIDOC
MixinInfo:
  Description: Handles the initial parsing of a mixin and acts as storage for all generated metadata.
  Lifecycle:
    - Loaded from disk during the prepare stage.
    - Proceeds to gather required information if the mixin bytecode is successfully loaded.
  Internal Components:
    - State: A struct holding mixin bytecode and unvalidated details.
    - ClassInfo: Used for accessing and storing class metadata.
    - SubType: Determines the specific type of the mixin.
```

--------------------------------

### Add Mixin Annotation Processor Dependencies for Multiple SourceSets

Source: https://github.com/spongepowered/mixin/wiki/Mixins-on-Minecraft-Forge

This Groovy snippet demonstrates how to add the Mixin Annotation Processor (AP) dependency to a Gradle build file. It shows how to include the AP for the main source set and additional source sets like `client` and `api` by specifying their respective annotation processor configurations. This ensures that Mixin processing is applied across all relevant source sets in a multi-module or multi-source project.

```groovy
dependencies {
    // Specify the version of Minecraft to use. If this is any group other than 'net.minecraft', it is assumed
    // that the dep is a ForgeGradle 'patcher' dependency, and its patches will be applied.
    // The userdev artifact is a special name and will get all sorts of transformations applied to it.
    minecraft 'net.minecraftforge:forge:1.17.1-37.0.70'

    annotationProcessor 'org.spongepowered:mixin:0.8.5:processor'
    clientAnnotationProcessor 'org.spongepowered:mixin:0.8.5:processor'
    apiAnnotationProcessor 'org.spongepowered:mixin:0.8.5:processor'
}
```

--------------------------------

### BeforeInvoke Injection Point API

Source: https://github.com/spongepowered/mixin/wiki/Injection-Point-Reference

The BeforeInvoke injection point searches for invoke instructions within the target instruction list and returns instructions that match specified criteria. It supports targeting specific invocations and selecting by ordinal index. It also provides a logging argument for debugging.

```APIDOC
Full Name: org.spongepowered.asm.mixin.injection.points.BeforeInvoke
Short Name: INVOKE

Options:
  target: string
    Description: the invocation to target, must be fully-qualified if remapped
  ordinal: integer
    Description: the ordinal index of the matching invocation to select (selects all if omitted)

Arguments:
  log: boolean
    Description: produces verbose output in the console as the injector scans a method, useful for diagnosing injectors which are behaving incorrectly or in unexpected ways

Description: This injection point searches for *invoke* instructions within the target instruction list and returns instructions which match the criteria. `target` should be a parsable [MemberInfo](http://jenkins.liteloader.com/job/Mixin/javadoc/index.html?org/spongepowered/asm/mixin/injection/struct/MemberInfo.html) which specifies an invocation to search for (omitting `target` will return all invoke instructions in the target). The `ordinal` specifies the index of the invocation to select, if omitted then all matching invocations are returned.

Notes: The `ordinal` value selects *within the instructions matched by `target`*, so if there are 4 invoke instructions in the target method, but only 2 match the value specified in target, then specifying `ordinal=1` selects the second instruction matching `target` not the second invocation in the method. In other words, `target` is always applied before `ordinal`.
```

--------------------------------

### APIDOC: Target Class

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

Documents the Target class, which is crucial for marshalling instruction replacements or additions made by injectors. It decorates the InjectionNode with any replaced instructions.

```APIDOC
Target:
  Purpose: Marshals instruction replacements or additions made by injectors.
  Details: Decorates the InjectionNode with its replacement instruction if replaced.
  Source: https://github.com/SpongePowered/Mixin/blob/master/src/main/java/org/spongepowered/asm/mixin/injection/struct/Target.java
```

--------------------------------

### APIDOC: AccessorInfo Class

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

Documents the AccessorInfo class, which holds parsed information for @Accessor methods. This information is used to construct the appropriate generator that fabricates the accessor method and adds it to the target class.

```APIDOC
AccessorInfo:
  Purpose: Holds parsed information for @Accessor methods.
  Details: Used to construct a generator that fabricates the accessor method and adds it to the target class.
  Source: https://github.com/SpongePowered/Mixin/blob/master/src/main/java/org/spongepowered/asm/mixin/gen/AccessorInfo.java
```

--------------------------------

### Configure Refmaps and Mixin Configs for Multiple SourceSets

Source: https://github.com/spongepowered/mixin/wiki/Mixins-on-Minecraft-Forge

This Groovy snippet expands on the MixinGradle configuration to include reference map and mixin configuration settings for multiple source sets, specifically `main`, `client`, and `api`. Each source set is assigned its own unique reference map JSON file, and corresponding mixin configuration JSON files are registered. This ensures proper processing and configuration for projects with complex source set structures.

```groovy
mixin {
    add sourceSets.main, 'mixins.mymod.refmap.json'
    add sourceSets.client, 'mixins.mymod.client.refmap.json'
    add sourceSets.api, 'mixins.mymod.api.refmap.json'

    config 'mixins.mymod.json'
    config 'mixins.mymod.client.json'
    config 'mixins.mymod.api.json'
}
```

--------------------------------

### Conditional Logic in Mixin Handler Using Target Arguments

Source: https://github.com/spongepowered/mixin/wiki/Advanced-Mixin-Usage---Callback-Injectors

Demonstrates a practical application of passing target method arguments to a handler. The handler method onSetPos uses the x and y coordinates to perform conditional logic, such as checking if the position is being set to the origin.

```Java
/**
 * Handler method, onSetPos
 */
@Inject(method = "setPos", at = @At("HEAD"))
protected void onSetPos(int x, int y, CallbackInfo ci) {
    // Check if setting position to origin
    if (x == 0 && y == 0) {
        this.handleOriginPosition();
    }
}
```

--------------------------------

### APIDOC: InjectionPoint Class

Source: https://github.com/spongepowered/mixin/wiki/Flippin'-Mixins,-how-do-they-work?

Documents the InjectionPoint class, from which metadata can be propagated. This metadata can be used by injectors for various purposes, such as influencing injection behavior.

```APIDOC
InjectionPoint:
  Purpose: Defines a point in the target method where an injection can occur.
  Details: Metadata from the InjectionPoint itself can be propagated to injectors.
  Source: https://github.com/SpongePowered/Mixin/blob/master/src/main/java/org/spongepowered/asm/mixin/injection/InjectionPoint.java
```

--------------------------------

### Enforcing Minimum Injection Success with `require`

Source: https://github.com/spongepowered/mixin/wiki/Advanced-Mixin-Usage---Callback-Injectors

Illustrates the use of the `require` argument in `@Inject` annotations to specify the minimum number of times an injection point must match. If fewer matches occur, a runtime error is raised, ensuring critical injections succeed. This can also be configured globally.

```Java
@Inject(method = "foo", at = @At(value = "INVOKE", target = "someMethod"), require = 2)
private void onInvokeSomeMethodInFoo(CallbackInfo ci) {
    ...
}
```
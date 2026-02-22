## Environment

* **Minecraft Version**: 1.20.1
* **Mod Loader**: Forge (ForgeGradle 6.x)
* **Mappings**: Parchment layered on top of Official (Mojmap)
* **Mixin**: SpongePowered Mixin 0.8.5 with MixinGradle 0.7.+
* **Languages**: Java 17 and optionally Kotlin

## Code Style Guidelines

Apply these rules only when appropriate. Do not over-apply them.

### General

* Prefer early returns.
* Keep methods small and focused.
* Write Javadoc only for public APIs.
* For internal logic, use brief English inline comments only when necessary.
* If Kotlin is used, restrict Java usage to Mixins only.

### Java

* Structure code clearly.
* Split responsibilities instead of implementing everything in a single method or class.

### Kotlin

* It is acceptable to wrap Minecraft APIs using extension functions to improve readability.

## Forge Guidelines

### Event System

* When using `@Mod.EventBusSubscriber`, explicitly declare the `bus` (`FORGE` or `MOD`).
* Keep event handlers small and delegate logic to separate methods.

## Mixin Guidelines

### Core Policy

* Use Mixins only when strictly necessary.
* Always consider whether a Forge event can replace the need for a Mixin.
* If Kotlin is available, use extension functions to access custom fields or functions instead of exposing them directly.

### Remap Handling

* Default remap behavior flows from `@Mixin` → injection annotations (e.g., `@Inject`) → `@At`.
* If most target methods are non-vanilla, set `remap = false` at the `@Mixin` level.

### Accessor / Invoker

* For vanilla classes, consider using Access Transformers as an alternative.
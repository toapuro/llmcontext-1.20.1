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

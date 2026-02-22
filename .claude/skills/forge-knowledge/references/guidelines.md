## Forge Guidelines

### Event System

* When using `@Mod.EventBusSubscriber`, explicitly declare the `bus` (`FORGE` or `MOD`).
* Keep event handlers small and delegate logic to separate methods.

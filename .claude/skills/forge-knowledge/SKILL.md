---
name: forge-knowledge
description: Forge 1.20.1 Knowledge
---

# Forge 1.20.1

## Event Bus

- Always explicitly specify `bus` in `@Mod.EventBusSubscriber`. Omitting it defaults to `FORGE` bus — silently breaks MOD bus events.

## Registry

- `RegistryObject.get()` called during static initialization = crash. Only call after game startup.

## Capability

- Forgetting `invalidateCaps()` / `lazyOptional.invalidate()` causes memory leaks. Always implement it.

## References

Access in numerical order and stop once the required information is obtained. If further information is needed, continue retrieving until the end.

1. `@reference/official.md`: [small] Official documentation(Registry, Block, Item, Entity...)
2. `@reference/neoforge.md`: [medium] Neoforge 1.20.4 documentation. Since there are changes, use them as references.
3. `@reference/notes.md`: [medium] Unofficial documentation(Rendering, Mixin, Mapping...)
4. `@reference/notes-full.md`: [large] Japanese full documentation
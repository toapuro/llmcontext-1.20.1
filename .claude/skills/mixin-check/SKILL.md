---
name: mixin-check
description: Use this to check your mixins after writing them
disable-model-invocation: true
---

Check all items on the following checklist

* [ ] No conflicts with major mods
* [ ] Correct `remap` configuration
* [ ] Injection is minimal and precise
* [ ] The method names defined with `@Invoker` or `@Accessor` do not exist in the target class
* [ ] Does not use `@Overwrite`, `@Redirect`, or `@ModifyConstant`
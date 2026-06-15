# CookieCore

Library mod for ephys' mods.

## Config Generation

In the constructor of your mod, add `ConfigSynchronizer.synchronizeConfig();` (`be.ephys.cookiecore.config.ConfigSynchronizer`) to launch config synchronization for your mod.

Then in any class, you can declare static configuration fields:

```java
class MyFeature {
  @Config(name = "myfeature.enabled", description = "Enable 'MyFeature'")
  @Config.BooleanDefault(value = true)
  public static ModConfigSpec.BooleanValue enabled;
}
```

Fields annotated with `@Config` + `@Config.<type>Default` will be set to an instance of `ModConfigSpec.ConfigValue` that you can use.

One of the `@Config.<type>Default` annotations *must* be specified.

### Supported config types

**string:**

```java
class MyFeature {
  @Config(name = "namespace.subnamespace.key")
  @Config.StringDefault("default value")
  public static ModConfigSpec.ConfigValue<String> myConfigValue;
}
```

**string list:**

*important*: Due to limitations in both the TOML parser, and Java annotations, the `StringListDefault` accepts an ARRAY of strings,
    but the `ModConfigSpec.ConfigValue<>` must use a `List` as its generic!

```java
class MyFeature {
  @Config(name = "namespace.subnamespace.key")
  @Config.StringListDefault({"value1", "value2"})
  public static ModConfigSpec.ConfigValue<List<String>> myConfigValue;
}
```

**boolean:**

```java
class MyFeature {
  @Config(name = "myfeature.enabled", description = "Enable 'MyFeature'")
  @Config.BooleanDefault(value = true)
  public static ModConfigSpec.BooleanValue enabled;
}
```

**int:**

```java
class MyFeature {
  @Config(name = "range_computation.base_range", description = "What is the beacon base range?")
  @Config.IntDefault(10)
  public static ModConfigSpec.IntValue baseRange;
}
```

**long:**

```java
class MyFeature {
  @Config(name = "range_computation.base_range", description = "What is the beacon base range?")
  @Config.LongDefault(10)
  public static ModConfigSpec.LongValue baseRange;
}
```

**double:**

```java
class MyFeature {
  @Config(name = "range_computation.base_range", description = "What is the beacon base range?")
  @Config.DoubleDefault(10.5)
  public static ModConfigSpec.DoubleValue baseRange;
}
```

**enum:**

```java
class MyFeature {
  @Config(name = "range_computation.vertical_range_type")
  @Config.EnumDefault(value = "FullHeight", enumType = BeaconVerticalRangeType.class)
  // BeaconVerticalRangeType is an enum
  public static ModConfigSpec.EnumValue<BeaconVerticalRangeType> verticalRangeType;
}
```

### Escape hatch

If you need to build part of the config object yourself, you can use `@OnBuildConfig` on a *static* method.

```java
class MyFeature {
  public static ModConfigSpec.IntValue configValue;
  
  @OnBuildConfig()
  public static onBuildConfig(ModConfigSpec.Builder rootBuilder) {
    configValue = rootBuilder.defineInRange("my_config_value", 5, 0, 10);
  }
}
```

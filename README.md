# Mocktale

> A mocking library for **Hytale** that lets you unit test plugins without spinning up a live server. Write tests first, ship plugins faster.

![Java 21+](https://img.shields.io/badge/Java-25%2B-brightgreen) ![Hytale API](https://img.shields.io/badge/Hytale-API-blue) ![pre-release](https://img.shields.io/badge/status-pre--release-orange)

---

## What is Mocktale?

Hytale's ECS architecture makes integration testing natural, but **unit testing individual components, systems, and commands** requires a live game context that's slow and brittle to stand up.

Mocktale provides lightweight fakes and builder APIs for Hytale's core primitives so you can validate plugin logic in plain JUnit 5 tests without the need to spin up a server to test your logic.

---

## What you can mock

| Target | Status | Description |
|---|---|---|
| **Components** | ✅ WIP | Create and attach fake ECS components to mock entities |
| **Systems** | ✅ WIP | Fully mock Systems, Store operations as well as test your own systems |
| **Commands** | ✅ WIP | Dispatch commands against a mock sender and assert on output, feedback messages, and side effects |
| **World context** | 🔜 Soon | Stub out chunk and world lookups so spatial queries return predictable data |
| **Events** | ✅ WIP | Fire and capture events to assert that your listeners respond correctly |
---

## Getting started

Add Mocktale to your `build.gradle.kts`:

```kotlin
repositories {
    maven("https://repo.hytheria.gg/snapshots")
}

dependencies {
    testImplementation("gg.hytheria:mocktale:0.1.0-SNAPSHOT")
    testImplementation("org.junit.jupiter:junit-jupiter:5.11.0")
}
```

---

## Examples

These are just API ideas and might change heavily

### Mocking a component

```java
@Test
void healthComponent_clampsToMax() {
    MockEntity entity = Mocktale.entity()
        .withComponent(YourCustomComponent::new, c -> {
            c.setX(100);
            c.setY(80);
        })
        .build();

    entity.getComponent(HealthComponent.class).someOperation("hello");

    Assertions.assertEquals(
        100,
        entity.getComponent(HealthComponent.class).getX()
    );
}
```

### Mocking a system tick

```java
@Test
void regenSystem_restoresHealthEachTick() {
    MockEntity entity = Mocktale.entity()
        .withComponent(HealthComponent::new, c -> c.setCurrent(50))
        .withComponent(RegenComponent::new, c -> c.setRate(5))
        .build();

    MockStore store = Mocktale.store()
        .withEntity(entity)
        .withSystem(new TestSystem())
        .build();

    store.tick(1f)

    Assertions.assertEquals(55,
        store.getComponent(HealthComponent.class).getX()
    );
}
```
---

## Roadmap

TODO

---

## Contributing

Contributions are welcome. Please open an issue before submitting a PR for anything non-trivial so we can align on approach first.

---

## Links

- [hytheria.gg](https://hytheria.gg)
- [HytaleModding.dev](https://hytalemodding.dev)

---

MIT licence

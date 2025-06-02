# Dependency Injection

The project uses [**Koin**](https://insert-koin.io/) for dependency injection (DI), allowing you to manage and inject dependencies in a clean way.

## Structure

The DI setup is split into three parts to handle platform-specific and shared configurations.

### Shared DI configuration

The common DI setup (used by both Android and iOS) is defined in:

```bash
shared/src/commonMain/kotlin/{your/package/name}/_internal/di/SharedModule.kt
```

This directory contains all the modules and bindings used across your shared code — such as repositories, use cases, and utilities.

!!! tip

    When you add new shared dependencies (e.g., repositories, use cases, or services), make sure to register them in this file.

### Android-specific DI

Android-only dependencies (e.g., platform services or context-aware bindings) are declared in:

```bash
shared/src/androidMain/kotlin/{your/package/name}/_internal/di/SharedAndroidDI.kt
```

This file imports the shared configuration and extends it with Android-specific declarations.

!!! tip

    If you create new Android-only classes (e.g. `Context`-based services), register them in this file.

### iOS-specific DI

iOS-only dependencies are handled similarly, in:

```bash
shared/src/iosMain/kotlin/{your/package/name}/_internal/di/SharedIosDI.kt
```

It also includes the shared setup and adds iOS-specific dependencies where needed.

!!! tip

    If you create new iOS-only classes (e.g. platform-specific bridges), be sure to bind them here.

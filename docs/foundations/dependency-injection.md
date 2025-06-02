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

!!! note

    If you create new shared classes (e.g. a new use case or service) that need to be injected, make sure to add their bindings here.

### Android-specific DI

Android-only dependencies (e.g., platform services or context-aware bindings) are declared in:

```bash
shared/src/androidMain/kotlin/{your/package/name}/_internal/di/SharedAndroidDI.kt
```

This file imports the shared configuration and extends it with Android-specific declarations.

!!! note

    If you create new Android-only classes (e.g. `Context`-based services), register them in this file.

### iOS-specific DI

iOS-only dependencies are handled similarly, in:

```bash
shared/src/iosMain/kotlin/{your/package/name}/_internal/di/SharedIosDI.kt
```

It also includes the shared setup and adds iOS-specific dependencies where needed.

!!! note

    If you create new iOS-only classes (e.g. platform-specific bridges), be sure to bind them here.

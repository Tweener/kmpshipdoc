# Local database

KMPShip uses [Room](https://developer.android.com/kotlin/multiplatform/room) for its local database solution on both Android and iOS. This enables you to persist and query structured data using a familiar, multiplatform approach.

For more details and advanced use cases, refer to the [official Room for Kotlin Multiplatform documentation](https://developer.android.com/kotlin/multiplatform/room).

The Room-based implementation lives in the `data` layer at:

```
com.example.myapp.data.source.room
```

---

## Structure overview

The local database setup is organized into the following main components:

### Models

These are data classes representing database tables. Each model is annotated with `@Entity` and corresponds to a table.

### DAOs

DAOs (Data Access Objects) expose SQL operations for a specific model. For example:

* `RoomUserModel` → `RoomUserDAO`

DAOs use standard SQL annotations (`@Query`, `@Insert`, `@Delete`, etc.) to define the operations.

### DataSources

DataSources serve as aggregators of one or more DAOs. They provide high-level CRUD access to data and isolate database access logic.

### Mappers

Mappers convert between domain entities (from the `domain` layer) and database models (in the `data` layer).

This layered design keeps the business logic clean and decoupled from the persistence logic.

---

## MyProjectRoomDatabase

`MyProjectRoomDatabase` is the main Room database class. It acts as the entry point to your local database configuration. You can rename this class to match your own project naming convention if needed.

Here, you must:

* Declare all models (`@Entity` classes)
* Bind all corresponding DAOs

Example:

```kotlin
@Database(
    entities = [RoomUserModel::class, RoomOtherClassModel::class],
    version = 1,
    exportSchema = false
)
abstract class MyProjectRoomDatabase : RoomDatabase() {
    abstract fun roomUserDao(): RoomUserDAO
    abstract fun roomOtherClassDao(): RoomOtherClassDao
}
```

---

## Example Models Provided

KMPShip includes two example models:

* `RoomExamplePartialModel`
* `RoomOtherClassModel`

Each comes with its own DAO and datasource, demonstrating best practices for multiplatform Room integration.

---

!!! success "Local database setup complete"

    You can now use the Room database in your KMPShip project to store and retrieve data on both Android and iOS platforms.

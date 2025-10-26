# Inventory-App
# CS 360 Mobile App: Inventory Tracker

This project is a mobile inventory management application for Android, built as part of the CS 360 Mobile Architecture and Programming course. It provides a secure, persistent, and efficient tool for warehouse staff or small business owners to manage stock levels in real-time.

---

## Project Overview

### App Requirements and Goals

The primary goal of this application is to provide a functional and secure tool for tracking warehouse inventory. The core requirements included:

* **Secure Authentication:** A database-backed login system for users to create accounts and log in.
* **Persistent Database:** A local SQLite database (using Room) to store user credentials and inventory items, ensuring data is saved even when the app is closed.
* **Full CRUD Functionality:** The ability for users to **C**reate (add), **R**ead (view), **U**pdate (change quantity), and **D**elete items from the inventory.
* **Proactive Alerts:** An automated system to notify the user via SMS when any item's stock quantity is reduced to zero.

This app was designed to address the needs of warehouse associates and inventory managers who require a fast, mobile, and accurate way to manage stock and prevent costly stockouts.

---

### UI and User-Centered Design

To support the user's needs, three primary screens were developed:

1.  **Login/Registration Screen:** A clean, focused screen for secure authentication.
2.  **Main Inventory List Screen:** A `RecyclerView` displaying all items in a `CardView` grid, providing an at-a-glance overview. A `FloatingActionButton` (FAB) provides a clear and standard way to add new items.
3.  **Add/Edit Item Screen:** A multi-purpose form for either creating a new item or editing an existing one (by passing the item's ID via an `Intent`).

My UI design kept users in mind by prioritizing **speed and clarity**. The design is user-centered because it uses familiar Android patterns (like the FAB and `RecyclerView`) that require no special training. The flow is logical: log in, see the list, and tap to add or edit. This design is successful because it minimizes the number of taps required for a user to perform their most common task: updating an item's quantity.

---

### Coding Approach and Strategy

My approach was to build a modern, robust Android application using industry-standard architecture.

* **Persistence Strategy:** I used the **Room Persistence Library** as an abstraction layer over SQLite. This involved creating `Entity` classes (`User`, `InventoryItem`), `DAOs` (Data Access Objects) to define database queries, and a singleton `AppDatabase` class to manage access.
* **Concurrency:** To prevent freezing the UI, all database operations (inserts, queries, deletes) were performed on a background thread using a dedicated `ExecutorService` (`databaseWriteExecutor`).
* **UI Updates:** The main screen uses `LiveData` to observe the inventory list from the database. This allows the `RecyclerView` to update automatically and efficiently whenever data is added, changed, or deleted, ensuring the UI is always in sync with the database.

These strategies (Room, background threads, and `LiveData`) are foundational to modern Android development. They can and should be applied to any future app that requires a responsive UI and persistent data.

---

### Functional Testing

The code was tested manually and iteratively using the **Android Emulator**. The testing process included:

* **Authentication:** Testing login with both valid and invalid credentials and creating a new user.
* **CRUD Operations:** Walking through the full user flow of adding a new item, seeing it appear on the list, tapping it to edit the quantity, and finally, deleting it.
* **Permission Handling:** Testing the SMS permission request by both **Allowing** and **Denying** it to ensure the app continued to function in both scenarios.
* **Edge Case Testing:** Specifically testing the zero-stock alert by updating an item's quantity to `0` and verifying that the permission/SMS logic was triggered.

This process is vital because it's the only way to find crashes and logical errors. For example, my testing revealed a critical crash when tapping the "add" button. This was because the `AddItemActivity` was not declared in the `AndroidManifest.xml`. Testing immediately exposed this bug, which would have otherwise been a app-breaking failure.

---

### Innovation and Problem-Solving

The most significant challenge was implementing the **SMS notification permission** in a user-friendly way. Instead of asking for a sensitive permission like `SEND_SMS` when the app first launches (which users often deny), I innovated by making the request **contextual**.

The app only requests the permission at the exact moment the user's action requires it: when they save an item and its quantity becomes zero. At this moment, it's clear to the user *why* the permission is needed. The app also had to be built to handle denial gracefully, allowing all other features to work perfectly even if the user chooses not to grant the permission.

---

### Component of Success

I was particularly successful in demonstrating my knowledge and skills in the **database and persistence architecture**.

Implementing the **Room Persistence Library** correctly involved creating multiple interconnected components:
1.  **Entities:** Defining the data schema for `User` and `InventoryItem`.
2.  **DAOs:** Writing clean, abstract query interfaces.
3.  **Database Class:** Setting up the singleton `AppDatabase` class.
4.  **Concurrency:** Integrating an `ExecutorService` to ensure all database calls were off the main UI thread.
5.  **Reactive UI:** Connecting the database to the UI using `LiveData` and an `InventoryAdapter`.

This component demonstrates a full-stack understanding of modern data persistence, from the SQL-level queries in the DAO to the asynchronous threading and reactive updates that power the user interface.

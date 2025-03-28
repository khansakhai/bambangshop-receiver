# BambangShop Receiver App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create SubscriberRequest model struct.`
    -   [X] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [X] Commit: `Implement add function in Notification repository.`
    -   [X] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [X] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [X] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X] Commit: `Implement receive_notification function in Notification service.`
    -   [X] Commit: `Implement receive function in Notification controller.`
    -   [X] Commit: `Implement list_messages function in Notification service.`
    -   [X] Commit: `Implement list function in Notification controller.`
    -   [X] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

>In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

Dalam tutorial ini, saya menggunakan `RwLock<>` untuk menyinkronkan akses ke `Vec<Notification>` karena saya ingin memungkinkan banyak thread membaca data secara bersamaan, tetapi hanya satu thread yang boleh menulis saat ada perubahan. Ini lebih efisien untuk kasus di mana aktivitas baca lebih sering daripada tulis. Saya tidak menggunakan `Mutex<>` karena `Mutex` hanya mengizinkan satu thread untuk mengakses data, baik itu untuk membaca maupun menulis, yang menurut saya bisa memperlambat proses jika ada banyak pembacaan data.

>In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

Saya menggunakan *library* eksternal `lazy_static` untuk membuat `Vec` atau `DashMap` sebagai variabel *static* karena di Rust, variabel *static* harus memenuhi aturan *ownership* dan *thread-safety* yang sangat ketat. Berbeda dengan Java, Rust tidak mengizinkan kita mengubah isi variabel *static* langsung tanpa pengaman karena ingin memastikan tidak terjadi *race condition*. Menurut saya, ini adalah cara Rust untuk menjaga agar program aman dijalankan secara paralel, dan itulah kenapa saya perlu membungkusnya dengan struktur seperti `RwLock<>` agar tetap bisa dimutasi dengan aman.

#### Reflection Subscriber-2

>Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

Saya belum mengeksplorasi bagian lain seperti `src/lib.rs` karena saya ingin fokus terlebih dahulu mengikuti alur yang sudah ditentukan dalam tutorial agar semua fungsi utama dapat berjalan dengan baik. Menurut saya, lebih baik memastikan setiap bagian yang dijelaskan benar-benar berhasil diimplementasikan sebelum mencoba hal-hal di luar petunjuk. Dengan begitu, saya bisa lebih mudah memahami alur sistem tanpa harus kebingungan jika terjadi error yang tidak saya ketahui asalnya. Namun ke depannya, saya tertarik untuk melihat file seperti `lib.rs` agar bisa memahami bagaimana struktur proyek secara keseluruhan diatur, terutama jika ingin melakukan pengembangan lebih lanjut.

>Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

Setelah mencoba menjalankan beberapa instance dari Receiver app, saya merasa Observer pattern benar-benar memudahkan proses menambahkan subscriber baru. Saya hanya perlu menjalankan instance baru dan melakukan subscribe, maka sistem langsung bisa mengirimkan notifikasi ke subscriber tersebut tanpa mengubah kode utama. Namun, jika saya ingin menambahkan lebih dari satu instance Main app, menurut saya sistemnya perlu lebih banyak penyesuaian karena akan muncul banyak sumber publisher, dan itu bisa menimbulkan konflik kalau tidak ditangani dengan baik.

>Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

Saya sudah mencoba fitur testing dan dokumentasi di Postman dan menurut saya keduanya sangat membantu. Testing memudahkan dalam memverifikasi respons dari setiap endpoint secara otomatis, sehingga proses pengujian menjadi lebih efisien. Sementara itu, dokumentasi memperjelas penggunaan API karena setiap request bisa disertai deskripsi dan contoh input. Dalam pengujian sistem notifikasi, kedua fitur ini sangat mendukung dalam memastikan bahwa notifikasi hanya dikirim ke subscriber yang sesuai, terutama saat menangani berbagai tipe produk di beberapa instance receiver.
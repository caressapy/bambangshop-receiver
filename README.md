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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

Dalam tutorial ini, kita menggunakan **RwLock<>** untuk mengelola akses terhadap vektor **Notification** agar tetap sinkron dalam lingkungan multi-threaded. **RwLock<>** memungkinkan banyak thread untuk membaca data secara bersamaan, sementara hanya satu thread yang boleh menulis dalam satu waktu. Jika kita menggunakan **Mutex<>**, setiap thread harus memperoleh lock sebelum membaca atau menulis, yang berarti bahkan operasi pembacaan pun bisa terhambat oleh thread lain yang sedang mengakses data. Hal ini dapat menyebabkan bottleneck yang mengurangi efisiensi sistem. Dengan **RwLock<>**, kita bisa memastikan bahwa proses pembacaan dapat dilakukan tanpa hambatan selama tidak ada thread lain yang sedang menulis.  

Selain itu, dalam tutorial ini, kita menggunakan **lazy_static** untuk mendefinisikan **Vec** dan **DashMap** sebagai variabel **static**. Tidak seperti di Java, Rust tidak mengizinkan mutasi langsung terhadap variabel **static** karena berpotensi menyebabkan **race condition** dalam lingkungan multi-threaded. Rust mengutamakan **memory safety** dan menghindari kondisi balapan dengan menerapkan aturan kepemilikan yang ketat. Oleh karena itu, kita menggunakan **lazy_static** untuk memastikan bahwa variabel **static** dapat diinisialisasi secara aman dan digunakan dalam konteks multi-threading tanpa menyebabkan masalah konkurensi.

#### Reflection Subscriber-2

Dalam eksplorasi saya terhadap aplikasi publisher dan receiver, saya belum mengeksplorasi file src/lib.rs secara mendalam. Saya lebih fokus mengikuti langkah-langkah dalam tutorial dan belum merasa perlu untuk melihat detail tambahan di dalamnya. Namun, saya menyadari bahwa file ini kemungkinan memiliki peran penting dalam konfigurasi dan pengaturan aplikasi secara keseluruhan.

Setelah menyelesaikan tutorial dan mencoba menguji sistem notifikasi dengan menjalankan beberapa instance Receiver, saya menyadari bahwa pola Observer sangat mempermudah dalam menambahkan subscriber baru. Hal ini karena setiap Subscriber hanya perlu dikonfigurasi dengan port yang berbeda, tanpa perlu melakukan perubahan kompleks lainnya pada Publisher. Begitu port dikonfigurasi dengan benar, Subscriber dapat langsung melakukan subscribe ke product_type tertentu tanpa harus mengubah konfigurasi lebih lanjut pada aplikasi Publisher.

Namun, jika ingin menambahkan lebih dari satu instance dari aplikasi Main, terdapat beberapa tantangan tambahan. Setiap instance baru dari Main app harus ditambahkan ke konfigurasi Receiver agar dapat dikenali. Selain itu, Subscriber juga harus melakukan proses subscribe secara manual ke domain instance baru dari Main app untuk mulai menerima notifikasi. Hal ini disebabkan karena pada versi ini, tiap Main app tidak berbagi data satu sama lain. Oleh karena itu, untuk memperbaiki sistem ini, perlu ada modifikasi pada mekanisme subscribe dan unsubscribe di Receiver app, sehingga setiap kali fungsi tersebut dipanggil, permintaan subscribe atau unsubscribe dikirimkan ke semua instance Main app, bukan hanya satu.

Saya belum mencoba membuat Test saya sendiri di Postman, tetapi dari koleksi Test yang tersedia, saya melihat bahwa fitur ini cukup mirip dengan unit testing yang umum digunakan dalam pengembangan perangkat lunak. Namun, saya mengalami kendala dengan konfigurasi port di file rocket.toml, di mana secara default port yang digunakan adalah 8001, padahal aplikasi ini merupakan Receiver yang seharusnya berjalan di port 8000. Saya masih bingung apakah port ini perlu diganti atau tidak agar sistem dapat berjalan dengan benar. Menurut saya, fitur ini sangat membantu dalam pengerjaan proyek kelompok, karena memungkinkan pengujian API endpoint dilakukan dengan lebih mudah tanpa perlu menulis kode tambahan. Dengan adanya fitur ini, tim saya dapat memastikan bahwa setiap endpoint bekerja dengan baik sebelum diimplementasikan secara penuh dalam aplikasi.

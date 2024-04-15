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

1. Dalam tutorial ini, kami menggunakan **RwLock<>** untuk menyinkronkan penggunaan **Vec** dari **Notifikasi**. Mengapa ini diperlukan dalam kasus ini, dan mengapa kita tidak menggunakan **Mutex<>** sebagai gantinya?
    - **RwLock<Vec>** sangat penting untuk mengatur penggunaan vektor yang berisi notifikasi di antara *thread*. RwLock memungkinkan beberapa *reader* untuk mengakses data secara bersamaan, sementara penulisan hanya dapat dilakukan oleh satu *writer* pada satu waktu.
    - Ini sangat krusial dalam konteks *multi-threading* untuk mencegah kondisi perlombaan dan memastikan konsistensi data.
    - Pemilihan RwLock dibuat karena memungkinkan banyak operasi baca yang terjadi serentak, yang cocok ketika operasi baca lebih dominan daripada operasi tulis.
    - Sebaliknya, Mutex membatasi akses ke satu *thread* pada satu waktu, yang dapat memperlambat kinerja ketika banyak operasi baca yang perlu dilakukan secara bersamaan.

2. Dalam tutorial ini, kami menggunakan pustaka eksternal **lazy_static** untuk mendefinisikan **Vec** dan **DashMap** sebagai variabel **"static"**. Dibandingkan dengan Java di mana kita dapat mengubah isi variabel **static** melalui fungsi **static**, mengapa Rust tidak mengizinkan kita melakukan hal yang sama?
    - Di Rust, perubahan pada variabel **static** tidak diizinkan karena bertentangan dengan prinsip-prinsip *ownership* dan *borrowing*. Ini berkaitan dengan konsep *mutable aliasing*, di mana Rust sangat memprioritaskan keamanan memori dan mencegah kesalahan data dengan aturan ketat mengenai referensi yang dapat diubah dan *shared mutability*.
    - Di Java, perubahan pada variabel **static** melalui fungsi **static** diizinkan karena pendekatan Java yang lebih fleksibel terhadap keamanan memori, menggunakan sinkronisasi eksplisit seperti *synchronized block* atau variabel *volatile* untuk menjaga keamanan *thread*.
    - Rust memberikan prioritas pada keamanan memori dengan menggunakan konsep *borrowing* dan primitif konkurensi seperti RwLock dan Mutex untuk mencapai konkurensi yang aman dan efisien, berbeda dengan Java yang lebih sering menggunakan metode langsung untuk mengelola keamanan *thread*.

#### Reflection Subscriber-2

1. Apakah Anda telah menjelajahi bagian kode di luar langkah-langkah tutorial, seperti **src/lib.rs**? Jika belum, jelaskan mengapa Anda tidak melakukannya. Jika sudah, jelaskan apa yang telah Anda pelajari dari bagian kode lain tersebut.
    - Saya menemukan bahwa penggunaan **lazy_static** untuk inisialisasi variabel global sangat membantu dalam memahami cara menginisialisasi variabel global secara efisien dalam Rust. Variabel global hanya diinisialisasi ketika pertama kali dibutuhkan, yang mengurangi beban saat program dijalankan.
    - Dari implementasi konfigurasi aplikasi menggunakan file `.env`, saya belajar bahwa pengaturan seperti port, koneksi database, atau token API bisa disimpan secara terpusat dan diakses dengan mudah.
    - Saya juga belajar tentang manfaat menggunakan tipe `Result<T,E>` dalam mengelola kesalahan di aplikasi Rust. Tipe Result membedakan antara nilai sukses (Ok) dan kesalahan (Err), memungkinkan penanganan kesalahan yang lebih terorganisir.
    - Penggunaan `Custom<Json>` untuk respons kesalahan memberi saya wawasan tentang cara meningkatkan kejelasan dan kegunaan informasi kesalahan yang disampaikan kepada pengguna.

2. Karena Anda telah menyelesaikan tutorial dan telah mencoba menguji sistem notifikasi Anda dengan memunculkan beberapa instans Receiver, jelaskan bagaimana Pola Observer memudahkan Anda untuk menambahkan lebih banyak pelanggan. Bagaimana jika Anda memunculkan lebih dari satu instans dari aplikasi utama, apakah masih akan mudah untuk ditambahkan ke sistem?
    - Pola Observer memudahkan penambahan pelanggan baru ke dalam sistem notifikasi. Dengan menyimpan daftar observer, kita bisa menambahkan pelanggan baru tanpa perlu mengubah struktur dasar aplikasi. Pola Observer memungkinkan pengiriman notifikasi ke semua pelanggan yang terdaftar secara efisien.
    - Namun, saya menemukan tantangan ketika mencoba menggunakan lebih dari satu instans dari aplikasi utama. Setiap instans memiliki Pola Observer-nya sendiri, sehingga jika ingin berbagi notifikasi antar instans, diperlukan mekanisme penyimpanan data bersama yang bisa diakses oleh semua instans. Ini bisa menjadi tantangan dalam memastikan sinkronisasi dan konsistensi antar instans aplikasi, terutama dalam pertukaran informasi atau notifikasi.

3. Apakah Anda telah mencoba membuat Tes Anda sendiri, atau meningkatkan dokumentasi pada koleksi **Postman** Anda? Jika Anda telah mencoba fitur-fitur tersebut, beritahu kami apakah itu berguna untuk pekerjaan Anda (baik itu pekerjaan tutorial Anda atau Proyek Kelompok Anda).
    - Meskipun saya belum membuat tes atau meningkatkan dokumentasi di Postman, saya percaya bahwa kedua fitur tersebut akan sangat berguna untuk proyek kelompok saya di masa depan.
    - Dalam proyek kelompok, kemampuan untuk membuat tes yang komprehensif akan sangat penting untuk memastikan bahwa aplikasi kami berfungsi dengan baik. Peningkatan dokumentasi di Postman juga akan memudahkan kerja sama tim dalam pengembangan dan pengujian aplikasi.


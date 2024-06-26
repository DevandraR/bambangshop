# BambangShop Publisher App
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
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough ? <br>
Jawab : Dalam Observer pattern, antarmuka Subscriber mendefinisikan kontrak bagi subscriber untuk menerima pembaruan dari subjek. Menggunakan antarmuka memungkinkan fleksibilitas, enkapsulasi, dan kemudahan pengujian. Namun, jika hanya satu jenis subscriber yang diperlukan, Model tunggal tanpa antarmuka cukup. Keputusan untuk menggunakannya tergantung pada kompleksitas dan kebutuhan aplikasi.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case ? <br>
Jawab : Penggunaan Vec sederhana dan mudah diimplementasikan, tetapi pencarian keunikan dalam Vec memerlukan iterasi melalui seluruh daftar, yang dapat menjadi tidak efisien saat ukuran daftar bertambah. DashMap, di sisi lain, memberikan akses yang cepat dan konstan untuk operasi pencarian, membuatnya efisien untuk memeriksa keunikan. Namun, DashMap mungkin memperkenalkan overhead tambahan dan tidak secara pasti mempertahankan urutan penyisipan. Jadi menggunakan DashMap menurut saya diperlukan karena kelebihan yang ditawarkannya ini.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread-safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead ? <br>
Jawab : DashMap menyediakan dukungan bawaan untuk akses concurrent yang memudahkan implementasi dan pemeliharaan struktur data yang aman dari sisi thread, namun memperkenalkan ketergantungan eksternal dan pembelajaran tambahan terkait dengan penggunaan library eksternal. Di sisi lain, pola Singleton memberikan kontrol penuh atas implementasi tanpa dependensi eksternal, tetapi memerlukan perhatian ekstra terkait keamanan thread dan bisa menjadi lebih kompleks dalam implementasinya. Jadi, menurut saya kita masih memerlukan DashMap karena kemudahan implementasinya, dan juga menggunakan library yang sudah ada akan memudahkan koding kedepannya.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model ? <br>
Jawab : Memisahkan "Service" dan "Repository" dari Model dalam pola MVC penting karena mematuhi prinsip-prinsip desain seperti Single Responsibility Principle dan Separation of Concerns. Ini memungkinkan fokus yang jelas pada tanggung jawab masing-masing komponen, meningkatkan modularitas, testability, dan fleksibilitas aplikasi. Dengan demikian, implementasi yang terpisah mempermudah pemeliharaan dan pengembangan aplikasi secara keseluruhan.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model ? <br>
Jawab : Jika hanya Model yang digunakan tanpa memisahkan Service dan Repository, interaksi antara model-model (Program, Subscriber, Notification) akan saling terikat di dalam Model. Hal ini dapat meningkatkan kompleksitas kode, ketergantungan yang erat antar model, dan duplikasi kode. Pengujian juga bisa menjadi lebih sulit karena keterkaitan yang rumit antar model. Memisahkan tanggung jawab ke lapisan Service dan Repository akan meningkatkan organisasi, pemeliharaan, dan pengujian aplikasi secara keseluruhan.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. Maybe you want to also list which features in Postman you are interested in or feel like it’s helpful to help your Group Project or any of your future software engineering projects. <br>
Jawab : Saya sudah menggunakan Postman beberapa kali, dan saya sangat terbantu oleh alat ini karena saya bisa langsung melihat http response dari setiap method http. Fitur pada Postman yang menurut saya sangat berguna pada saat ini dan masa depan adalah sistem workspace yang dapat mengelompokkan berdasarkan method http, ini sangat membantu karena membantu monitoring pada saat pengetesan aplikasi.

#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use ? <br>
Jawab : Pada tutorial ini, digunakan Push model karena server secara aktif memberikan data pada subscriber. Jadi setiap permintaan data dari subscriber, publisher secara aktif memberikan response kepada subscriber tanpa adanya permintaan tambahan.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull) <br>
Jawab : Dalam tutorial ini, menggunakan variasi Pull model dari Observer Pattern dapat menghasilkan keuntungan penggunaan sumber daya yang efisien dan kontrol yang lebih baik bagi subscribers. Namun, hal ini juga dapat menyebabkan keterlambatan dalam menerima data, kompleksitas dalam manajemen langganan, dan peningkatan beban pada server.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process. <br>
Jawab : Jika tidak menggunakan multi-threading dalam proses notifikasi, program kemungkinan akan memiliki banyak kekurangan. Tanpa multi-threading, proses notifikasi akan dieksekusi secara berurutan dalam thread utama, yang dapat menyebabkan perilaku blocking di mana setiap tugas notifikasi akan memblokir thread utama hingga selesai. Akibatnya, keterbatasan keseimbangan terjadi karena program hanya dapat menangani satu tugas notifikasi pada satu waktu, membatasi keseimbangan dan skalabilitas. Selain itu, potensi deadlock juga meningkat karena dalam lingkungan single-threaded, sebuah tugas notifikasi yang memblokir atau masuk ke dalam keadaan deadlock dapat menyebabkan seluruh program menjadi tidak responsif atau hanging. Responsif antarmuka pengguna juga dapat menjadi terbatas, karena thread utama diblokir saat menangani notifikasi, mengakibatkan pengalaman pengguna yang buruk, terutama dalam aplikasi interaktif.

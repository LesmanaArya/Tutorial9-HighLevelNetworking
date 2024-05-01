## Tutorial 9 Reflection

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
    - Perbedaan utama dari unary, server streaming, dan bi-directional streaming menurut saya adalah di bagian prosedur komunikasinya. 
        - **Unary** memiliki mekanisme dimana client hanya mengirimkan suatu request tunggal saja kepada server dan server akan mengembalikan response secara tunggal juga sehingga komunikasi yang terjadi antarkeduanya adalah single request dan single response. Unary cocok digunakan untuk skenario dimana client tidak membutuhkan response secara terus-menerus dalam jumlah yang banyak. Contoh skenarionya adalah ketika submit form atau sekedar mengecek suatu homepage.

        - **Server streaming** memiliki mekanisme yang mirip seperti Unary dimana dimana client hanya mengirimkan single request kepada server, tetapi response yang diberikan oleh server adalah stream of messages atau kumpulan message yang diberikan secara terus-menerus hingga komunikasi dinyatakan selesai, misalkkan terjadi error atau salah satu dari mereka memutuskan untuk memberhentikannya. Contoh skenarionya adalah ketika melakukan upload file seperti video atau foto.

        - **Bi-directional streaming** memiliki mekanisme dimana client dan server sama-sama bertukar stream of messages antar sesamanya. Kedua stream of messages yang diberikan saling independen sehingga client dapat terus mengirimkan message kepada server sekaligus menerima messages dari server tanpa perlu menunggu response dari server terlebih dahulu. Contoh skenarionya adalah streaming live video atau di game multiplayer online

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
    - **Authentication** : Dapat mempertimbangkan penggunaan TLS atau Transport Layer Security untuk memastikan bahwa komunikasi antar client dan server sudah aman atau terenkripsi. Mekanisme lain yang bisa dipertimbangkan adalah token based authentication seperti JWT untuk mengamankan proses autentikasi client. Hal lain yang dapat dilakukan adalah pastikan hanya client sudah terautentikasi dengan aman saja yang diberikan akses
    - **Authorization** : Pastikan bahwa hanya client tertentu saja yang diberikan autorisasi untuk melakukan suatu hal tertentu. Dapat juga dilakukan dengan menerapkan Role Based Access Control dimana client akan dibagi menjadi role-role tertentu yang masing-masing memiliki autorisasi masing-masing
    - **Data Encryption** : Dapat menerapkan enkripsi dalam mengirim data antar client dan server, dapat juga diterapkan dalam store data. Selalu lakukan maintenance terhadap enkripsi data di aplikasi untuk memastikan bahwa keamanan enkripsi masih berjalan dengan baik

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
    - Tantangan utama dalam bidirectional streaming adalah memastikan bahwa koneksi yang digunakan untuk komunikasi antara client dan server selalu berjalan dengan baik. Beberapa hal yang berpotensi menjadi tantangan dalam melakukan hal tersebut adalah me-manage sinkronisasi antar keduanya, manajemen resource sistem seperti misal koneksi atau spesifikasi hardware terutama jika ingin melakukan bidirectional streaming dalam waktu yang lama, serta mempertimbangkan performa yang dihasilkan apalagi jika banyak message yang masuk secara paralel

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
    - Keuntungan dari menggunakan **tokio_stream::wrappers::ReceiverStream** menurut saya adalah integrasinya dengan module Tokio lainnya yang sangat membantu dalam asynchronus programming dalam Rust. Selain itu, fleksibilitas yang dimiliki oleh ReceiverStream memungkinkan kita dalam menerima berbagai tipe data yang masuk. Kerugian dari menggunakan **tokio_stream::wrappers::ReceiverStream** adalah di kompleksitasnya. Hal ini tidak ekslusif pada tokio saja tetapi dalam asynchronus programming secara umum karena membutuhkan effort lebih dalam hal misalnya error handling atau concurrency. Selain itu, mempelajari Tokio sendiri bukanlah hal yang mudah apalagi bagi developer  yang sebelumnya jarang berinteraksi pada bagian asynchronus programming

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
    - Mengorganisir modul-modul terkait gRPC yang ada berdasarkan fungsionalitas atau konsepnya. Dapat juga menerapkan SOLID principle khususnya pada bagian SRP  untuk modularity dan ISP untuk reusability. 

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
    - Implementasi error handling seperti ketika jumlah pembayaran kurang atau kegagalan lain dalam memenuhi syarat transaksi. Lakukan pengiriman gRCP kode yang sesuai setiap kali kita memproses pembayaran. Hal lain yang dapat ditambahkan mungkin yang terkait dengan security seperti validasi dan autorisasi pembayaran client

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
    - Penggunaan gRPC dapat meningkatkan efisiensi dan maintanability dalam distributed system. Penggunaan gRPC sendiri tidak ekslusif atau terbatas dalam platform terentu saja, sehingga sangat fleksibel dalam digunakan. Protocol komunikasi gRPC juga memungkinkan pertukaran komunikasi dengan efisien antar client dan server, khususnya ketika digunakan pada skenario real time dalam streaming atau bi-directional communication

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
    - HTTP/2 memungkinkan untuk melakukan multiplexing atau mengirimkan multiple response dan request menggunakan single connection. HTTP/2 juga dilengkapi dengan header compression yang dapat meng-improve latency dan bandwith. Di sisi lain, HTTP/2 memiliki kompleksitas yang lebih tinggi dibanding HTTP/1.1 terutama di bagian protocol dan operational. HTTP/2 pun karena memiliki lebih banyak fitur dibanding HTTP/1.1, akan memakan resource yang lebih banyak terutama ketika heavy load

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
    - REST API memiliki mekanisme komunikasi yang berbeda terutama pada bagian snychronus communication serta client side nya. Komunikasi pada REST API dilakukan secara synchronus sedangkan gRPC bi-directional dapat dilakukan secara asinkronus. Komunkasi dari sisi client juga pada REST API biasanya diinisiasi terlebih dahulu oleh client untuk nanti diproses oleh server, sedangkan pada bi-directional streaming, client dan server sama-sama bisa mengirimkan komunikasi secara berbarengan dan real time. Overall, bi-directional gRPC secara umum dapat memberikan komunikasi yang lebih efisien dibanding REST API

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
    - Schema-based approach oleh gRCP dapat memberikan keuntungan seperti peningkatan efisiensi, safety, dan versioning. Schema-based juga dapat memperjelas komunikasi antara client dan server karena message yang diberikan haruslah sesuai dengan definisi skemanya. Tetapu di sisi lain, Schema-less JSON di REST API dapat memberikan fleksibilitas yang lebih baik karena tidak bergantung pada skema tertentu. JSON pun secara kode lebih mudah untuk dimengerti dan di-inspect oleh developer
# Pacman Project

## Group 3
5025211015&emsp;Muhammad Daffa 'Ashdaqfillah <br/>
5025211051&emsp;Hanun Shaka Puspa <br/>
5025211113&emsp;Revanantyo Dwigantara <br/>

## Alur Kerja Program
Di awal pengembangan game ini, digunakan environment Unity dengan bahasa C# dan Anaconda untuk pemanggilan API python sebagai implementasi algoritma _path finding_ GBFS dan A*. Namun ditemui beberapa kendala dalam pengembangannya. Sehingga kemudian dilakukan migrasi ke program Python dengan module _Pygame_. Repository ini berisi _submodule_ untuk kedua implemetasi tersebut.

## Pacman Game dengan C# dan API Python
Diimplementasikan pada program [berikut](./PACMAN-KB-MIDTERM/) <br/>

### GBFS
Sebelum memasuki fungsi Python untuk menghitung heuristik dalam program Python, dilakukan pemanggilan fungsi Python pada kode C# yang mengirimkan argumen arah dan posisi hantu serta pemain.

Dalam kode Python, dilakukan pengecekan heuristik dengan menggunakan jarak lurus dari sumbu x dan y antara posisi hantu dan pemain menggunakan rumus Pythagoras. Pengecekan hanya dilakukan untuk langkah-langkah yang mungkin dilakukan oleh hantu (Atas, Bawah, Kanan, Kiri).

Fungsi ini mengembalikan hasil arah dengan jarak terpendek dari arah yang tersedia bagi hantu menuju pemain.

### A*
Pada fungsi algoritma pencarian A* (A Star), dilakukan pengecekan dengan menggunakan heuristik yang sama seperti pada kode GBFS, namun dengan penambahan biaya (cost) pada setiap perpindahan langkah.

Jumlah perpindahan langkah diketahui dengan mengambil pola dari TILE yang didefinisikan dalam permainan.

Akhirnya, program akan mengembalikan nilai arah dari hantu yang memiliki total biaya paling sedikit.

### Kendala
#### Lagging akibat pemanggilan program Python dalam C#
Penggunaan program Python dalam kode C# menyebabkan kendala dalam fungsi permainan, yaitu lagging. Hal ini terjadi karena pemanggilan fungsi tersebut setiap kali hantu memutuskan tujuan untuk bergerak. Dengan demikian, pemanggilan program Python berjalan secara terus-menerus (jika diasumsikan 24 frame/detik), sehingga akan dipanggil 24 kali dalam satu detik. Namun, proses pemanggilan program Python memerlukan sumber daya komputer, yang dapat menyebabkan lagging dalam permainan.

Solusi sementara untuk mengurangi lagging yang disebabkan oleh pemanggilan program Python setiap saat adalah dengan membatasi hantu untuk memutuskan tujuan bergerak hanya setiap 2 detik sekali. Hal ini dapat mengurangi jumlah pemanggilan program yang berkelanjutan. Namun, konsekuensinya adalah bahwa dalam jangka waktu kurang dari 2 detik, hantu akan menggunakan pergerakan terakhir dan jika bertemu dengan tembok, hantu akan bergerak secara acak (random movement).

#### Implementasi algoritma A* tidak berjalan dengan baik
Implementasi A* dalam program ini tidak berjalan dengan baik karena dalam pemetaan TILE_MAP, tidak mungkin menentukan fungsi TILE_MAP untuk mengatur pergerakan hantu sesuai dengan arah yang tersedia. Hal ini dikarenakan tidak ada pemetaan yang dapat dikonversi menjadi bentuk biner dari Unity ke dalam program Python.

## Pacman Game dengan Single-Language Python
Diimplementasikan pada program [berikut](./PACMAN-PYTHON/)

### Kendala
#### Posisi objek pada tilemap tidak selalu tepat
Posisi pemain dan hantu pada tilemap dalam Python diinput berdasarkan panjang frame, yaitu 900x950. Namun, arah pergerakan hantu dan pemain dibatasi menjadi 30x32 tile. Oleh karena itu, agar pergerakan tetap smooth dan teratur, digunakan pergerakan per satu tile frame.

Hal ini mengakibatkan kendala pada pergerakan arah hantu dan pemain yang terbatas pada peta (30x32). Dalam beberapa kondisi, objek dapat tersangkut dan tidak mengikuti arah tile yang seharusnya. Karena dalam perhitungan GBFS, arah ditentukan setiap 1 frame. Oleh karena itu, kondisi ini sedikit mempersulit pemain dan hantu untuk mendapatkan arah GBFS yang tepat.

## Kesimpulan
Kedua implementasi tersebut memiliki kekurangan dan kelebihan masing-masing. Untuk implementasi C# - Python yang lebih cepat, dibutuhkan pencarian mekanisme pemanggilan API yang lebih cepat. Untuk implementasi single-language python, dibutuhkan _refactor_ untuk mengatasi keterbatasan kalkulasi posisi dengan tilemap yang lebih baik.
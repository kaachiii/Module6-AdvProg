- Nama : Ischika Afrilla
- NPM : 2306227955

## Reflection
### Commit 1 Reflection notes
1. What is inside the `handle_connection` method?

Method `handle_connection` berfungsi untuk menangani koneksi yang masuk melalui `TcpStream`. Pertama, `stream` dibungkus dengan `BufReader` agar dapat membaca data secara lebih efisien. Selanjutnya, permintaan HTTP dari klien dibaca baris demi baris menggunakan iterator `.lines()`, dan setiap baris hasilnya diambil dengan `.map(|result| result.unwrap())`. Proses pembacaan berlangsung hingga menemukan baris kosong menggunakan `.take_while(|line| !line.is_empty())`, karena dalam format HTTP, header permintaan diakhiri dengan baris kosong. Setelah semua baris dikumpulkan ke dalam sebuah `Vec<String>`, isi permintaan HTTP dicetak ke dalam terminal dengan println!. Namun, method ini hanya membaca dan mencetak permintaan tanpa memberikan respons kembali kepada klien.

### Commit 2 Reflection notes
1. What does the additional lines of code in handle_connection do?

Tambahan baris kode dalam `handle_connection` membuat fungsi ini tidak hanya membaca permintaan HTTP dari klien tetapi juga mengirimkan respons kembali. Setelah membaca permintaan, fungsi menyiapkan respons dengan menentukan status HTTP "200 OK", membaca isi file `hello.html`, dan menghitung panjangnya untuk disertakan dalam header. Kemudian, respons diformat sesuai standar HTTP, terdiri dari status baris, header `Content-Length`, dan isi file sebagai body respons. Terakhir, respons dikirim ke klien menggunakan `stream.write_all(response.as_bytes()).unwrap();`, yang memastikan data dikirim dalam bentuk byte. Sekarang, server sudah dapat menampilkan halaman web berdasarkan file HTML yang tersedia.

![Commit 2 screen capture](/assets/images/commit2.png)

### Commit 3 reflection notes
1.  How to split between response and why the refactoring is needed?

Saya memisahkan logika penanganan koneksi dan pembuatan respons dengan menggunakan fungsi `handle_response`, sehingga kode lebih terstruktur dan mudah dikelola. Dalam `handle_connection`, permintaan HTTP dibaca dan disimpan dalam `http_request`. Jika tidak ada permintaan yang diterima, fungsi langsung berhenti. Baris pertama dari permintaan HTTP kemudian diambil untuk menentukan respons yang akan dikirim. Fungsi `handle_response` digunakan untuk memproses baris permintaan tersebut dengan membandingkannya terhadap `"GET / HTTP/1.1"`. Jika cocok, server mengirimkan `hello.html` dengan status **200 OK**. Jika tidak, server mengirimkan `404.html` dengan status **404 NOT FOUND**. Setelah menentukan file yang sesuai, `handle_response` membaca isi file, menghitung panjangnya, lalu membentuk respons HTTP yang dikembalikan sebagai `String` ke `handle_connection`, respons tersebut kemudian dikirimkan ke klien melalui `stream.write_all(response.as_bytes()).unwrap();`.

![Commit 3 screen capture](/assets/images/commit3.png)

### Commit 4 reflection notes
Saya melakukan refactor kembali pada bagian `handle_connection` dan `handle_response`. Selain itu, saya juga menambahkan `thread, time::Duration` pada bagian `use std::` agar server dapat menangani permintaan secara lebih efisien dengan menunda eksekusi sementara atau memungkinkan penggunaan multi-threading untuk menangani beberapa koneksi secara bersamaan. Saya juga menambahkan durasi sleep pada `handle_response`, khususnya pada permintaan `"GET /sleep HTTP/1.1"`, untuk mensimulasikan delay selama 10 detik sebelum mengembalikan respons sehingga saya dapat menguji bagaimana server menangani permintaan yang membutuhkan waktu pemrosesan lebih lama.
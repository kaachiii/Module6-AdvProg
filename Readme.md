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
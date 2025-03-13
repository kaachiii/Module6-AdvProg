- Nama : Ischika Afrilla
- NPM : 2306227955

## Reflection
### Commit 1 Reflection notes
1. What is inside the `handle_connection` method?

Method `handle_connection` berfungsi untuk menangani koneksi yang masuk melalui `TcpStream`. Pertama, `stream` dibungkus dengan `BufReader` agar dapat membaca data secara lebih efisien. Selanjutnya, permintaan HTTP dari klien dibaca baris demi baris menggunakan iterator `.lines()`, dan setiap baris hasilnya diambil dengan `.map(|result| result.unwrap())`. Proses pembacaan berlangsung hingga menemukan baris kosong menggunakan `.take_while(|line| !line.is_empty())`, karena dalam format HTTP, header permintaan diakhiri dengan baris kosong. Setelah semua baris dikumpulkan ke dalam sebuah `Vec<String>`, isi permintaan HTTP dicetak ke dalam terminal dengan println!. Namun, method ini hanya membaca dan mencetak permintaan tanpa memberikan respons kembali kepada klien.
## (1) Commit 1 : Penjelasan Fungsi `handle_connection`

Fungsi `handle_connection` digunakan untuk menangani permintaan dari klien, yaitu koneksi TCP yang diterima melalui `TcpListener`.

```rust
fn handle_connection(mut stream: TcpStream) {
```
Kode di atas menunjukkan bahwa fungsi ini menerima parameter `TcpStream`, yang merepresentasikan koneksi aktif antara server dan klien. Kata kunci `mut` menunjukkan bahwa `stream` akan dimodifikasi di dalam fungsi.

```rust
let buf_reader = BufReader::new(&mut stream);
```
Di sini, `BufReader` digunakan untuk membaca data dari `stream` secara lebih efisien, baris demi baris. Selain itu, `BufReader` membungkus `TcpStream`, sehingga mempermudah pembacaan permintaan HTTP.

```rust
let http_request: Vec<_> = buf_reader 
    .lines()
    .map(|result| result.unwrap()) 
    .take_while(|line| !line.is_empty()) 
    .collect();
```
- `buf_reader.lines()` mengembalikan iterator untuk membaca koneksi satu per satu dalam bentuk baris.  
- `.map(|result| result.unwrap())` mengekstrak nilai `String` dari `Result<String, Error>`.  
- `.take_while(|line| !line.is_empty())` berhenti membaca saat menemukan baris kosong.  
- `.collect()` mengumpulkan semua baris yang telah dibaca ke dalam `Vec<String>`.  

```rust
println!("Request: {:#?}", http_request);
```
Setelah itu, isi permintaan HTTP dicetak dalam format debug untuk mempermudah proses debugging.
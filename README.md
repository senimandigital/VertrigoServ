# VertrigoServ
Ada banyak sekali pilihan paket distribusi untuk Apache Webserver dan semuanya memiliki keunggulan masing-masing, tapi kami tetap menggunakan paket distribusi Vertrigo hanya karena alasan kenyamaan dalam penggunaan. Salah satu alasan kami menggunakan Vertrigo adalah: Paket distribusi ini sudah terbukti bersifat portabel.

Webserver yang bersifat portabel memiliki arti dan manfaat yang sangat penting terutama sekali bagi kita yang berprofesi sebagai programmer atau Software Developer, salah satu manfaatnya adalah kita tidak perlu khawatir kehilangan database apabila sewaktu-waktu system operasi mengalami Crash dan tidak lagi dapat di Booting.

### /vertrigo.ini
Adalah file konfigurasi yang ada di directory root, melalui file konfigurasi ini kita bisa menentukan apakah secara default Apache dan MySQL dijalankan. Kita perlu Vertrigo hanya menjalankan MySQL server, adalah ketika akan membackup Database dengan Engine InnoDB. Dengan adanya fasilitas hanya run MySQL Server kita bisa menjalankan beberapa MySQL Server sekaligus didalam sebuah komputer/dibawah system operasi.

### /Mysql/my.ini
Adalah file konfigurasi yang ada di directory mysql, disini kita bisa mengganti: lokasi penyimpanan database dan bisa juga mengganti konfigurasi port MySQL yang akan dijalankan.

### /Apache/conf/vertrigo.conf
Adalah file konfigurasi yang ada di directory /Apache/conf , disini kita bisa mengganti/menambahkan pengaturan virtual host. Kami biasanya tidak mengedit file ini secara terus menerus, karena Apache sendiri sudah bisa melakukan include file konfigurasi. Karena adanya dukungan ini, maka jauh lebih bijaksana untuk mengincludekan sebuah folder konfigurasi yang baru, contoh:
```
Include "D:\Online\Dropbox\_Konfigurasi\Apache Virtual Hosts for VertrigoServ Enabled"
```
Dengan menginjeksi script seperti contoh diatas, maka: file konfigurasi didalam folder "Apache Virtual Hosts for VertrigoServ Enabled" otomatis akan di eksekusi oleh Apache saat startup. Karena Apache hanya mengeksekusi script konfigurasi saat startup, maka apabila kita melakukan perubahan konfigurasi kita perlu merestart Apache terlebih dahulu agar script konfigurasi yang baru bisa dijalankan.

## Kesimpulan
Selain ketiga file diatas, tidak terlalu banyak hal mendasar yang perlu dikuasai dengan baik lagi. Meski demikian ada beberapa hal yang juga penting untuk diketahui.

### /Apache/logs/error.log
File ini akan dengan cepat mencapai kapasitas besar dengan ukuran bisa mencapai puluhan Giga Byte(GB) apabila kita memiliki kesalahan dalam pengaturan Virtual Host, Kesalahan yang umum terjadi adalah:
1. Kita membuat konfigurasi virtual host, tetapi folder tidak tersedia/belum dibuat pada harddisk.
2. Kita menulis kode program yang tidak sempurna, misal kita memakai sebuah variabel tanpa mendefinisikan-nya. Meskipun aplikasi tetap bisa berjalan dengan normal, tapi Apache akan melaporkan itu sebagai error dan menyimpan laporan-nya didalam file error.log ini dan laporan itu akan ditambahkan setiap kali request. Yang arti-nya website dengan satu user bisa menghasilkan file log mencapai ukuran 1 GB hanya dalam waktu satu bulan, bayangkan jika user website sudah menjacapai 1.000 atau bahkan 10.000 pengguna mungkin file log ini akan mencapai ukuran 1 GB hanya dalam beberapa jam saja. Jika VPS dengan harddisk berukuran 20GB maka butuh waktu kurang dari 1 hari untuk membuat server sepenuh-nya Down.

### Tabu dalam pemrograman
1. Menggunakan variabel Non Global: Dalam pemrogramman sangat dianjurkan menggunakan variabel Global yang sudah tersedia seperti:
```
$_ENV, $_GET, $_POST, $_SERVER, $_SESSION
```
  Tujuannya tentu saja untuk menghindari Apache dalam membengkak-kan ukuran file logs.

2. Memutar-mutar nilai variabel, contoh:
```
$title   = $_POST['title'];
$content = $_POST['content'];
```
Kesalahan seperti ini sering dilakukan oleh pemula, melempar-lempar nilai variabel seperti ini sangat tidak diperlukan. Selain menggandakan penggunaan memory langkah seperti ini hanya memperpanjang jumlah baris kode program, disaat bersaman juga mengurangi jumlah pengguna maksimal yang dapat ngeakses aplikasi sekaligus.

Jika terus mempertahankan penuilisan kode program seperti ini, jangan heran jika aplikasi anda dihargai jauh dibawah nilai UMR(Upah Minimum Regional). Dan memang seharusnya seperti itu, karena aplikasi yang ditulis dengan gaya seperti ini tidak mampu mengejar setengah performa dari aplikasi yang ditulis dengan algoritma dan mekanisme yang optimal.

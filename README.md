# UTS_Pemograman-Web2
## Artikel Konversi gambar browser dengan assembly: cepat, ringan, dan tanpa server 
| UTS  |  Pemrograman Web 2   |
|-------|--------- |
| Nama   | Endang Sirait |
| Nim  | 312310588 |
| Kelas | TI.23.A6 |
| **Mata Kuliah**    |     Pemrograman Web 2    |
| **Dosen Pengampu** |Agung Nugroho, S.Kom., M.Kom  |
| **Tautan Artikel** |https://endangsrt.blogspot.com/2025/05/konversi-gambar-di-browser-dengan.html |

# Pendahuluan

Saat ini, banyak orang menggunakan aplikasi web untuk berbagai keperluan, termasuk mengedit atau mengubah file gambar. Namun, proses seperti mengubah format gambar (misalnya dari JPG ke PNG) biasanya membutuhkan koneksi internet dan proses upload ke server, yang bisa memakan waktu dan mengganggu privasi pengguna.
WebAssembly (Wasm) hadir sebagai solusi untuk masalah ini. Dengan teknologi ini, kita bisa membuat aplikasi web yang mampu memproses gambar langsung di browser, tanpa perlu mengirim data ke server. Artinya, proses jadi lebih cepat, ringan, dan aman.
Melalui artikel ini, kita akan membahas bagaimana cara membuat aplikasi konversi gambar sederhana berbasis WebAssembly, yang bisa digunakan langsung di browser tanpa koneksi internet.
Memahami Konsep: WebAssembly dan Konversi Gambar

# Apa itu WebAssembly?
WebAssembly adalah teknologi yang memungkinkan kita menjalankan kode dengan performa tinggi langsung di browser. Biasanya, aplikasi web hanya menggunakan JavaScript, tapi dengan Wasm, kita bisa menjalankan kode dari bahasa seperti C++, Rust, atau lainnya yang sudah dikompilasi menjadi WebAssembly.
Hasilnya, aplikasi jadi jauh lebih cepat — terutama untuk tugas berat seperti pengolahan gambar, video, atau data besar.
Mengapa Cocok untuk Konversi Gambar?
Mengubah format gambar membutuhkan proses yang cukup berat:
•	Membaca dan memproses file gambar
•	Mengubah format dan menyimpannya kembali
•	Menjaga kualitas dan ukuran file tetap optimal
Dengan bantuan WebAssembly, kita bisa menggunakan pustaka pemrosesan gambar yang biasa digunakan di desktop, seperti libvips atau imagemagick, langsung di browser. Keuntungannya:
•	Lebih cepat, karena semuanya diproses di perangkat pengguna
•	Lebih aman, karena tidak ada data yang dikirim ke server
•	Bisa offline, cocok untuk aplikasi yang ringan dan praktis
# Kelebihan
1. Cepat dan Efisien : Proses konversi dilakukan langsung di browser tanpa perlu upload file. Performa hampir setara dengan aplikasi desktop karena menggunakan kode native (C/C++/Rust).
2. Privasi Terjaga : Tidak ada data atau file yang dikirim ke server. Sangat cocok untuk dokumen sensitif seperti KTP, ijazah, atau pas foto.
3. Tanpa Instalasi : Tidak perlu mengunduh atau menginstal software apapun. Bisa digunakan dari berbagai perangkat (laptop, tablet, bahkan HP).
4. Dukungan Offline : Aplikasi bisa dibuat berjalan tanpa internet jika menggunakan service worker atau dibuka sekali sebelumnya.
5. Portabilitas : WebAssembly bisa berjalan di semua browser modern (Chrome, Firefox, Safari, Edge, dll).
# Kekurangan
1. Ukuran File Awal Besar : Library yang dikompilasi ke WebAssembly (misal imagemagick.wasm) kadang cukup besar (bisa > 1 MB). Ini bisa memperlambat loading awal aplikasi.
2. Terbatas pada Fitur Umum : Tidak semua fitur dari library gambar bisa digunakan secara langsung di browser. Fungsi kompleks (seperti batch processing, filter lanjutan) bisa sulit diimplementasikan.
3. Pemrosesan Bergantung pada Perangkat : Karena berjalan di browser, performa bergantung pada spesifikasi perangkat pengguna. Di HP atau laptop lama, bisa lebih lambat.
4. Kesulitan Debugging : Debugging WebAssembly lebih sulit dibandingkan JavaScript biasa. Tools untuk melihat dan memperbaiki bug masih terbatas.
5. Keterbatasan Akses Sistem : Tidak bisa langsung akses file sistem atau folder (hanya via input browser). Fitur seperti "drag-and-drop folder" atau "akses kamera" perlu ditangani terpisah dengan JavaScript.

Kode HTML dan JavaScript untuk Konversi Gambar
Berikut adalah contoh implementasi yang saya buat untuk mengonversi gambar di browser menggunakan WebAssembly:
```html```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Konversi Gambar di Browser</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
        }

        h1 {
            color: #333;
            margin-bottom: 20px;
        }

        .container {
            text-align: center;
            background-color: #fff;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            width: 90%;
            max-width: 600px;
        }

        #inputImage {
            margin: 15px 0;
            padding: 10px;
            font-size: 16px;
            border-radius: 5px;
            border: 1px solid #ddd;
        }

        #convertBtn {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-bottom: 20px;
        }

        #convertBtn:hover {
            background-color: #45a049;
        }

        #outputImage {
            margin-top: 20px;
            max-width: 100%;
            max-height: 400px;
            border: 1px solid #ddd;
            border-radius: 8px;
        }

        p {
            font-size: 14px;
            color: #888;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Konversi Gambar di Browser dengan JavaScript</h1>
        <input type="file" id="inputImage" />
        <button id="convertBtn">Konversi Gambar</button>
        <br />
        <img id="outputImage" src="" alt="Gambar Hasil Konversi" />
        <p>Gambar yang telah dikonversi akan ditampilkan di sini.</p>
    </div>
    <script src="convert.js"></script>
</body>
</html>

```javascript```
document.getElementById('convertBtn').addEventListener('click', function() {
    const inputImage = document.getElementById('inputImage').files[0];
    if (!inputImage) {
        alert('Silakan pilih gambar terlebih dahulu!');
        return;
    }

    const reader = new FileReader();
    
    reader.onload = function(event) {
        const img = new Image();
        img.onload = function() {
            // Membuat elemen canvas untuk menggambar gambar
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');
            
            // Menentukan ukuran canvas sama dengan ukuran gambar
            canvas.width = img.width;
            canvas.height = img.height;
            
            // Menggambar gambar ke dalam canvas
            ctx.drawImage(img, 0, 0);

            // Mengonversi gambar ke format PNG
            const imageData = canvas.toDataURL('image/png'); // Gambar menjadi base64 PNG

            // Menampilkan gambar hasil konversi
            const outputImage = document.getElementById('outputImage');
            outputImage.src = imageData;

            // Menyesuaikan ukuran gambar hasil konversi agar lebih kecil jika diperlukan
            outputImage.style.maxWidth = '100%';  // Agar tidak melebihi lebar container
            outputImage.style.maxHeight = '400px'; // Membatasi tinggi gambar
        };
        
        // Memuat gambar dari FileReader
        img.src = event.target.result;
    };

    // Membaca gambar yang diupload sebagai DataURL
    reader.readAsDataURL(inputImage);
});

Hasil Eksperimen
Dari eksperimen yang dilakukan, hasil konversi gambar dapat diperoleh dalam hitungan detik setelah gambar dipilih. Proses ini dilakukan sepenuhnya di browser tanpa mengirimkan data gambar ke server. Kecepatan konversi sangat bergantung pada ukuran gambar dan spesifikasi perangkat yang digunakan.
Keuntungan Penggunaan JavaScript untuk Konversi Gambar
1.	Privasi Terjaga: Tidak ada data yang dikirim ke server, menjaga privasi gambar pengguna.
2.	Tanpa Server: Semua pengolahan gambar dilakukan di browser, mengurangi ketergantungan pada server dan meningkatkan performa.
3.	Tanpa Koneksi Internet
Dengan menggunakan WebAssembly, aplikasi ini dapat bekerja sepenuhnya offline setelah pertama kali dimuat, yang menjadikannya pilihan yang sangat berguna untuk aplikasi berbasis browser yang membutuhkan privasi dan efisiensi.
4.	Efisiensi dan Kecepatan: Proses pengolahan gambar lebih cepat karena dilakukan secara lokal, tanpa mengunggah gambar terlebih dahulu.
5.	Sederhana dan Mudah: Implementasi menggunakan JavaScript murni dan Canvas API membuat aplikasi mudah dibuat tanpa pustaka eksternal.



# Kode HTML dan JavaScript untuk Konversi Gambar
Berikut adalah contoh implementasi yang saya buat untuk mengonversi gambar di browser menggunakan WebAssembly:
```html```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Konversi Gambar di Browser</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
        }

        h1 {
            color: #333;
            margin-bottom: 20px;
        }

        .container {
            text-align: center;
            background-color: #fff;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            width: 90%;
            max-width: 600px;
        }

        #inputImage {
            margin: 15px 0;
            padding: 10px;
            font-size: 16px;
            border-radius: 5px;
            border: 1px solid #ddd;
        }

        #convertBtn {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-bottom: 20px;
        }

        #convertBtn:hover {
            background-color: #45a049;
        }

        #outputImage {
            margin-top: 20px;
            max-width: 100%;
            max-height: 400px;
            border: 1px solid #ddd;
            border-radius: 8px;
        }

        p {
            font-size: 14px;
            color: #888;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Konversi Gambar di Browser dengan JavaScript</h1>
        <input type="file" id="inputImage" />
        <button id="convertBtn">Konversi Gambar</button>
        <br />
        <img id="outputImage" src="" alt="Gambar Hasil Konversi" />
        <p>Gambar yang telah dikonversi akan ditampilkan di sini.</p>
    </div>
    <script src="convert.js"></script>
</body>
</html>

```javascript```
document.getElementById('convertBtn').addEventListener('click', function() {
    const inputImage = document.getElementById('inputImage').files[0];
    if (!inputImage) {
        alert('Silakan pilih gambar terlebih dahulu!');
        return;
    }

    const reader = new FileReader();
    
    reader.onload = function(event) {
        const img = new Image();
        img.onload = function() {
            // Membuat elemen canvas untuk menggambar gambar
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');
            
            // Menentukan ukuran canvas sama dengan ukuran gambar
            canvas.width = img.width;
            canvas.height = img.height;
            
            // Menggambar gambar ke dalam canvas
            ctx.drawImage(img, 0, 0);

            // Mengonversi gambar ke format PNG
            const imageData = canvas.toDataURL('image/png'); // Gambar menjadi base64 PNG

            // Menampilkan gambar hasil konversi
            const outputImage = document.getElementById('outputImage');
            outputImage.src = imageData;

            // Menyesuaikan ukuran gambar hasil konversi agar lebih kecil jika diperlukan
            outputImage.style.maxWidth = '100%';  // Agar tidak melebihi lebar container
            outputImage.style.maxHeight = '400px'; // Membatasi tinggi gambar
        };
        
        // Memuat gambar dari FileReader
        img.src = event.target.result;
    };

    // Membaca gambar yang diupload sebagai DataURL
    reader.readAsDataURL(inputImage);
});

# Hasil Eksperimen
Dari eksperimen yang dilakukan, hasil konversi gambar dapat diperoleh dalam hitungan detik setelah gambar dipilih. Proses ini dilakukan sepenuhnya di browser tanpa mengirimkan data gambar ke server. Kecepatan konversi sangat bergantung pada ukuran gambar dan spesifikasi perangkat yang digunakan.
Keuntungan Penggunaan JavaScript untuk Konversi Gambar
1.	Privasi Terjaga: Tidak ada data yang dikirim ke server, menjaga privasi gambar pengguna.
2.	Tanpa Server: Semua pengolahan gambar dilakukan di browser, mengurangi ketergantungan pada server dan meningkatkan performa.
3.	Tanpa Koneksi Internet
Dengan menggunakan WebAssembly, aplikasi ini dapat bekerja sepenuhnya offline setelah pertama kali dimuat, yang menjadikannya pilihan yang sangat berguna untuk aplikasi berbasis browser yang membutuhkan privasi dan efisiensi.
4.	Efisiensi dan Kecepatan: Proses pengolahan gambar lebih cepat karena dilakukan secara lokal, tanpa mengunggah gambar terlebih dahulu.
5.	Sederhana dan Mudah: Implementasi menggunakan JavaScript murni dan Canvas API membuat aplikasi mudah dibuat tanpa pustaka eksternal.

![WhatsApp Image 2025-05-04 at 00 24 50_a13baacd](https://github.com/user-attachments/assets/25de2892-60d0-4501-b84b-d2743d4e18d1)
![WhatsApp Image 2025-05-04 at 00 25 08_ea3215a8](https://github.com/user-attachments/assets/be0ed0ae-a000-488e-9041-92b761e6b0c3)
![WhatsApp Image 2025-05-04 at 00 25 36_be49fc3f](https://github.com/user-attachments/assets/f44b0e5c-d748-4c1b-97d3-4711170a8c82)
![WhatsApp Image 2025-05-04 at 00 04 31_30c6b1ae](https://github.com/user-attachments/assets/42ce4674-d184-4b2e-a133-285b4bd33d43)





# Kesimpulan 
Dengan semakin berkembangnya teknologi WebAssembly, kita bisa membuat aplikasi konversi gambar yang cepat, ringan, dan tidak bergantung pada server. Proses konversi gambar yang dilakukan langsung di browser tidak hanya menghemat waktu, tetapi juga menjaga privasi pengguna karena data tidak perlu dikirim ke server. Dengan segala kelebihan yang ditawarkan, WebAssembly menjadi pilihan yang tepat untuk membangun aplikasi web pengolahan gambar yang efisien dan mudah diakses.
Penerapan teknologi ini membuka peluang baru dalam menciptakan aplikasi yang lebih responsif dan ramah pengguna, sekaligus meningkatkan keamanan dan efisiensi. Dengan dukungan browser modern, siapa pun kini dapat menikmati aplikasi pengolahan gambar yang cepat dan praktis, tanpa perlu khawatir tentang privasi dan koneksi internet yang tidak stabil.

# Referensi
1.	Mozilla Developer Network (MDN) - WebAssembly
2.	Emscripten Documentation - Emscripten
3.	stb_image - stb_image library
4.	Rendering Medical Images using WebAssembly https://pdfs.semanticscholar.org/d6a3/dde8d43b6a63b4f957ab4cd19bd9750db63b.pdf



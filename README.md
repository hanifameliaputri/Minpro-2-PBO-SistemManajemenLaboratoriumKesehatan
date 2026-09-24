# Minpro-2-PBO-SistemManajemenLaboratoriumKesehatan

**Nama:** Hanif Amelia Putri  
**Kelas:** B  
**NIM:** 2509116075  


---


## 1. Deskripsi Singkat Program

Program **Sistem Manajemen Laboratorium Kesehatan** merupakan program berbasis Java yang digunakan untuk mengelola data pasien, petugas laboratorium, pemeriksaan, dan hasil pemeriksaan. Program menyediakan beberapa menu utama, yaitu pendaftaran pemeriksaan, pengelolaan pasien, pengelolaan petugas, pengelolaan pemeriksaan, serta pengelolaan hasil pemeriksaan.


Fitur utama aplikasi:

- **Pendaftaran Pemeriksaan**: mendaftarkan pasien (baru atau yang sudah terdaftar) ke suatu jenis pemeriksaan yang ditangani oleh petugas tertentu.
- **Kelola Pasien**: tambah, lihat semua, cari, dan hapus data pasien.
- **Kelola Petugas**: tambah **Analis** atau **Dokter** dan lihat semua petugas.
- **Kelola Pemeriksaan**: tambah, lihat semua, cari, ubah, dan hapus jenis pemeriksaan beserta biayanya.
- **Kelola Hasil Pemeriksaan**: input hasil pemeriksaan, lihat semua hasil, dan lihat riwayat hasil per pasien.
- **Validasi input** agar program tidak berhenti akibat kesalahan pengetikan pengguna.
- **Dummy data** awal sehingga data langsung tersedia saat program pertama kali dijalankan.

Seluruh data disimpan sementara selama program berjalan menggunakan `ArrayList`.

---

## 2. Penjelasan Alur Program

### Alur Umum

```text
[Start] Main.main()
        |
        v
new LaboratoriumController()
  |-- membuat 4 ArrayList (Pasien, Petugas, Pemeriksaan, HasilPemeriksaan)
  |-- mengatur counter ID awal (nextId... = 1)
  |-- membuat objek LaboratoriumView
  '-- isiDataAwal()  -> mengisi dummy data
        |
        v
controller.jalankanProgram()
        |
        v
+------------------------------------+
|     TAMPIL MENU UTAMA (while)      | <--------------------+
+------------------------------------+                      |
| 1. Pendaftaran Pemeriksaan         |                      |
| 2. Kelola Pasien                   |                      |
| 3. Kelola Petugas                  |                      |
| 4. Kelola Pemeriksaan              |                      |
| 5. Kelola Hasil Pemeriksaan        |                      |
| 6. Keluar                          |                      |
+------------------------------------+                      |
        |                                                   |
        |--- Pilih 1-5 -> Proses menu terkait --------------+
        |
        '--- Pilih 6   -> "Program selesai" -> [Program Berhenti]
```

1. **Program dimulai dari `Main.java`.** Method `main()` membuat objek `LaboratoriumController` lalu memanggil `jalankanProgram()`.
2. **Konstruktor Controller** menyiapkan empat `ArrayList`, counter ID otomatis, objek `LaboratoriumView`, lalu memanggil `isiDataAwal()` untuk memasukkan dummy data.
3. **`jalankanProgram()`** membuka `Scanner` dan menjalankan perulangan `while` yang terus menampilkan menu utama sampai pengguna memilih menu 6. Input menu dibaca dengan `bacaInt()` sehingga huruf atau input kosong tidak membuat program error. Pilihan di luar 1-6 menampilkan pesan `Pilihan tidak tersedia.`
4. **Setiap sub-menu** (menu 2 sampai 5) memiliki perulangan sendiri dan baru kembali ke menu utama ketika pengguna memilih opsi *Kembali*.

### Alur Tiap Menu

**Menu 1 - Pendaftaran Pemeriksaan**

```text
Pilih pasien
  |-- 1. Pasien Baru       -> input nama, umur, jenis kelamin, keluhan -> tambahPasien() (ID otomatis: P1, P2, ...)
  '-- 2. Pasien Terdaftar  -> tampil semua pasien -> input ID Pasien
                              '-- tidak ditemukan -> pesan error -> kembali ke menu utama
        |
        v
Tampil daftar pemeriksaan -> input ID Pemeriksaan
  '-- tidak ditemukan -> pesan error -> kembali ke menu utama
        |
        v
Tampil daftar petugas -> input ID Petugas
  '-- tidak ditemukan -> pesan error -> kembali ke menu utama
        |
        v
Tampil KONFIRMASI PENDAFTARAN (ID/nama pasien, pemeriksaan, biaya, petugas, status)
        |
        v
Simpan ke pasienTerdaftar, pemeriksaanTerdaftar, petugasTerdaftar
```

Data pendaftaran terakhir disimpan pada tiga atribut Controller (`pasienTerdaftar`, `pemeriksaanTerdaftar`, `petugasTerdaftar`) dan dipakai kembali oleh menu 5 saat menginput hasil.

**Menu 2 - Kelola Pasien**: tambah pasien, lihat semua pasien, cari pasien berdasarkan ID (ditampilkan lengkap dengan umur, jenis kelamin, dan keluhan), dan hapus pasien.

**Menu 3 - Kelola Petugas**: tambah Analis, tambah Dokter, dan lihat semua petugas. Objek `Analis` dan `Dokter` disimpan dalam satu `ArrayList<Petugas>`.

**Menu 4 - Kelola Pemeriksaan**: tambah, lihat semua, cari, ubah (nama dan biaya), dan hapus pemeriksaan.

**Menu 5 - Kelola Hasil Pemeriksaan**

```text
1. Input Hasil
     |-- Belum pernah melakukan pendaftaran (menu 1)?
     |     -> tampil pesan "Silakan lakukan menu 1. Pendaftaran Pemeriksaan terlebih dahulu"
     '-- Sudah -> tampil semua pasien -> input ID Pasien -> input hasil -> input status (Normal / Tidak Normal)
           -> tambahHasil() memeriksa ID pasien, pemeriksaan, dan petugas
           -> jika valid, hasil disimpan dengan ID otomatis (H1, H2, ...)
2. Lihat Semua Hasil
3. Lihat Riwayat Hasil per Pasien (input ID Pasien)
```

Jenis pemeriksaan dan petugas pada hasil diambil dari pendaftaran terakhir (menu 1), sedangkan ID pasien diinput ulang oleh pengguna.

**Menu 6 - Keluar**: menampilkan pesan penutup dan menghentikan perulangan program.

---

## 3. Penerapan Encapsulation dan Inheritance

### A. Encapsulation

Encapsulation diterapkan dengan **menyembunyikan atribut** di dalam class dan hanya membolehkan akses melalui method resmi (**getter** dan **setter**).

**1. Atribut bersifat `private`**

Seluruh atribut pada package `model` berstatus `private`:

| Class | Atribut `private` |
|---|---|
| `Pasien` | `id`, `nama`, `umur`, `jenisKelamin`, `keluhan` |
| `Petugas` | `id`, `nama`, `umur`, `jenisKelamin` |
| `Analis` | `spesialisasiBidang` |
| `Dokter` | `nomorSTR` |
| `Pemeriksaan` | `idPemeriksaan`, `namaPemeriksaan`, `biaya` |
| `HasilPemeriksaan` | `idHasil`, `idPasien`, `idPemeriksaan`, `idPetugas`, `hasil`, `status` |


<img height="200" alt="image" src="https://github.com/user-attachments/assets/abf510b6-16aa-44ea-ba62-d0f15ee1a935" />


Atribut tersebut tidak dapat diakses langsung dari luar class (misalnya dari Controller atau View). Controller dan View harus memakai getter, contohnya `pasien.getNama()` atau `pemeriksaan.getBiaya()`.

**2. Getter dan setter bersifat `public`**


<img height="200" alt="image" src="https://github.com/user-attachments/assets/39fc70a5-ef99-41bb-ad16-69195176263d" />


**3. Setter berfungsi sebagai validasi**

Setter tidak hanya mengisi nilai, tetapi juga menyaring data sehingga object selalu berisi data yang valid.

| Setter | Aturan validasi |
|---|---|
| `setNama()`, `setJenisKelamin()`, `setKeluhan()`, `setSpesialisasiBidang()`, `setNomorSTR()`, `setNamaPemeriksaan()`, `setHasil()`, `setStatus()` | Tidak boleh `null` atau kosong |
| `setUmur()` | Harus lebih dari 0 |
| `setBiaya()` | Tidak boleh negatif |


<img height="200" alt="image" src="https://github.com/user-attachments/assets/28c6e143-284d-4e99-8d5b-f7bde0d036bf" />


**4. Konstruktor memakai setter**

Agar validasi juga berlaku saat object dibuat, konstruktor memanggil setter, bukan mengisi atribut secara langsung:


<img height="200" alt="image" src="https://github.com/user-attachments/assets/45ccf3cc-1254-4052-a3a0-c9070cebdd71" />




**5. Encapsulation pada Controller dan class `final`**

Daftar data di Controller juga dibuat `private final` (`daftarPasien`, `daftarPetugas`, `daftarPemeriksaan`, `daftarHasil`), sehingga hanya bisa dimodifikasi lewat method Controller seperti `tambahPasien()` dan `hapusPasien()`. Class `HasilPemeriksaan` dideklarasikan `final` agar tidak dapat diturunkan.

---

### B. Inheritance

Inheritance diterapkan pada **1 superclass** (`Petugas`) dan **2 subclass** (`Analis` dan `Dokter`).

```text
                +---------------------------+
                |          Petugas          |   <- Superclass
                +---------------------------+
                | - id                      |
                | - nama                    |
                | - umur                    |
                | - jenisKelamin            |
                +---------------------------+
                | + tampilkanInfo()         |
                | + tampilkanInfo(boolean)  |
                +---------------------------+
                              ^
                              | extends
              +---------------+---------------+
              |                               |
+---------------------------+   +---------------------------+
|          Analis           |   |          Dokter           |   <- Subclass
+---------------------------+   +---------------------------+
| - spesialisasiBidang      |   | - nomorSTR                |
+---------------------------+   +---------------------------+
```

1. **Superclass `Petugas`** menyimpan data dan perilaku yang dimiliki semua petugas: `id`, `nama`, `umur`, `jenisKelamin`, getter/setter, serta method `tampilkanInfo()`.
2. **Subclass `Analis`** mewarisi `Petugas` dan menambahkan atribut khusus `spesialisasiBidang` (contoh: Hematologi).
3. **Subclass `Dokter`** mewarisi `Petugas` dan menambahkan atribut khusus `nomorSTR` (nomor Surat Tanda Registrasi).

Pewarisan dituliskan dengan keyword `extends`:


<img height="200" alt="image" src="https://github.com/user-attachments/assets/03720126-602d-48f5-954f-f791ca6e409f" />

dan

<img height="200" alt="image" src="https://github.com/user-attachments/assets/bfb0f14c-cdca-4c02-bd08-92148dc6002a" />



Konstruktor subclass memanggil konstruktor superclass dengan `super(...)`, lalu mengisi atribut miliknya sendiri lewat setter:



<img height="200" alt="image" src="https://github.com/user-attachments/assets/a03aa779-3f7e-4204-81e5-53bf3126b198" />



**Manfaat pewarisan pada program ini:**

- Kode `id`, `nama`, `umur`, `jenisKelamin` cukup ditulis sekali di `Petugas`. `Analis` dan `Dokter` langsung memakainya. Contohnya `analis.getId()` di Controller adalah method yang diwarisi dari `Petugas`.
- Karena `Analis` dan `Dokter` adalah `Petugas`, keduanya bisa disimpan dalam satu `ArrayList<Petugas>`.

**Hubungan dengan encapsulation:** atribut `id` dan `nama` bersifat `private` di `Petugas`, sehingga subclass tidak mengaksesnya langsung. Subclass memakai `super(...)` dan `super.tampilkanInfo()` untuk mendapatkan data tersebut.

> Class `Pasien` berdiri sendiri (tidak mewarisi `Petugas`) karena pasien bukan petugas dan memiliki atribut `keluhan`.

---

## 4. Penerapan Overriding dan Cara Kerjanya

**Overriding** adalah ketika subclass menulis ulang method milik superclass dengan **nama, parameter, dan tipe kembalian yang sama** agar perilakunya sesuai kebutuhan subclass. Pada program ini, overriding diterapkan pada method `tampilkanInfo()` dan `tampilkanInfo(boolean detail)`.

### Letak Overriding

| Method di superclass | Class yang meng-override | Lokasi |
|---|---|---|
| `Petugas.tampilkanInfo()` | `Analis` | `model/Analis.java` (baris 34-37) |
| `Petugas.tampilkanInfo()` | `Dokter` | `model/Dokter.java` (baris 33-36) |
| `Petugas.tampilkanInfo(boolean detail)` | `Analis` | `model/Analis.java` (baris 39-45) |
| `Petugas.tampilkanInfo(boolean detail)` | `Dokter` | `model/Dokter.java` (baris 38-44) |

Method yang di-override berada di `model/Petugas.java`:



<img height="200" alt="image" src="https://github.com/user-attachments/assets/0909ad61-27c7-4451-9c2b-b08b3eb8ae3f" />



Versi di subclass `Analis`:



<img height="200" alt="image" src="https://github.com/user-attachments/assets/2c564272-3fd7-45d7-ad83-8d3c5b102f11" />


Versi di subclass `Dokter`:



<img  height="200" alt="image" src="https://github.com/user-attachments/assets/7fa10d70-ea65-4840-a56c-c1950d970415" />


### Cara Kerja Overriding

Overriding dipakai ketika pengguna membuka **Kelola Petugas -> Lihat Semua Petugas** (juga saat memilih petugas pada menu Pendaftaran Pemeriksaan). Kedua fitur tersebut memanggil `tampilkanSemuaPetugas()` di `LaboratoriumController`:

```java
// LaboratoriumController.java
private final ArrayList<Petugas> daftarPetugas;   // berisi Analis dan Dokter

public void tampilkanSemuaPetugas() {
    ...
    for (Petugas p : daftarPetugas) {
        view.tampilkanBaris(p.tampilkanInfo(true));
    }
}
```



Langkah kerjanya:

1. Variabel `p` bertipe **`Petugas`**, tetapi object sebenarnya di dalam list bisa berupa `Analis` atau `Dokter`.
2. Program memanggil `p.tampilkanInfo(true)`. Saat runtime, Java melihat **tipe object sebenarnya** (bukan tipe variabelnya) lalu menjalankan versi method milik class tersebut. Mekanisme ini disebut *dynamic method dispatch*.
3. Jika object adalah `Analis`, versi `Analis.tampilkanInfo(boolean)` yang berjalan. Karena `detail = true`, method memanggil `super.tampilkanInfo(true)` (mengambil data umum dari `Petugas`: ID, nama, umur, jenis kelamin), lalu menambahkan `Peran: Analis` dan `Spesialisasi/Bidang`.
4. Jika object adalah `Dokter`, versi `Dokter.tampilkanInfo(boolean)` yang berjalan, dengan tambahan `Peran: Dokter` dan `No. STR`.
5. Hasilnya, satu perintah yang sama menghasilkan tampilan berbeda sesuai jenis petugasnya.

Contoh output program:



<img height="200" alt="image" src="https://github.com/user-attachments/assets/8047bdb4-73d2-463c-b700-ba107440f0b6" />


**Peran `super`:** Setiap subclass memakai `super.tampilkanInfo(...)` agar tidak menulis ulang bagian yang sudah ada di `Petugas`. Subclass hanya menambahkan informasi khususnya.

**Versi ringkas (`detail = false`):** Jika `tampilkanInfo(false)` dipanggil pada object `Analis` atau `Dokter`, method akan meneruskan ke `tampilkanInfo()` milik subclass itu sendiri. Hasilnya adalah versi ringkas dengan peran, misalnya:

```text
ID: PT1 | Nama: Asti Putri | Peran: Analis | Spesialisasi/Bidang: Hematologi
```

**Tanpa overriding**, semua petugas hanya akan tampil dengan data umum dari `Petugas` tanpa peran, spesialisasi, atau nomor STR.

**Catatan:** Anotasi `@Override` membuat compiler memeriksa bahwa method benar-benar menimpa method milik superclass. Jika nama atau parameternya salah, program tidak dapat dikompilasi.

---

## 5. Nilai Tambah

### A. Arsitektur MVC

Program dipisah menjadi beberapa package agar tanggung jawab tiap bagian jelas.

```text
LaboratoriumKesehatan/src/main/java/
|
|-- Main/
|   '-- Main.java                     <- Titik awal program
|
|-- controller/                       <- [CONTROLLER]
|   '-- LaboratoriumController.java   (alur menu, pengelola ArrayList, pencarian, validasi input)
|
|-- model/                            <- [MODEL]
|   |-- Pasien.java
|   |-- Petugas.java                  (superclass)
|   |-- Analis.java                   (subclass Petugas)
|   |-- Dokter.java                   (subclass Petugas)
|   |-- Pemeriksaan.java
|   '-- HasilPemeriksaan.java
|
'-- view/                             <- [VIEW]
    '-- LaboratoriumView.java         (menu, judul, pesan, dan konfirmasi pendaftaran)
```

<img height="400" alt="image" src="https://github.com/user-attachments/assets/df901a28-d166-452e-a1f6-5fcad20a0fed" />


| Package | Peran |
|---|---|
| `Main` | Membuat `LaboratoriumController` dan menjalankan `jalankanProgram()`. |
| `controller` | Menerima input pengguna, mengelola `ArrayList`, dan memanggil View untuk menampilkan hasil. |
| `model` | Menyimpan struktur data, encapsulation, dan hierarki pewarisan. |
| `view` | Khusus menampilkan menu dan pesan ke terminal. |

### B. Polymorphism dan Overloading

**Polymorphism.** `Analis` dan `Dokter` disimpan dalam satu `ArrayList<Petugas>`:

```java
daftarPetugas.add(analis); // Analis disimpan sebagai Petugas
daftarPetugas.add(dokter); // Dokter disimpan sebagai Petugas
```

Saat data ditampilkan, method yang berjalan menyesuaikan object aslinya (lihat [bagian 4](#4-penerapan-overriding-dan-cara-kerjanya)).

**Overloading.** Method dengan nama sama tetapi parameter berbeda dalam satu class:

| Class | Method overload |
|---|---|
| `Petugas` | `tampilkanInfo()` dan `tampilkanInfo(boolean detail)` |
| `Pasien` | `tampilkanInfo()` dan `tampilkanInfo(boolean detail)` |
| `LaboratoriumController` | `bacaInt(Scanner)` dan `bacaInt(Scanner, int min, int max)` |

> Perbedaan keduanya: **overloading** terjadi dalam satu class dengan parameter berbeda, sedangkan **overriding** terjadi antara superclass dan subclass dengan parameter yang sama.

### C. Validasi Input

Validasi dilakukan di dua lapis: pada **method baca input di Controller** dan pada **setter di Model**.

| Method | Fungsi |
|---|---|
| `bacaInt()` | Memastikan input berupa angka bulat (`try-catch NumberFormatException`) dan berada dalam rentang tertentu |
| `bacaUmur()` | Umur harus antara 1 sampai 120 |
| `bacaDouble()` | Memastikan input berupa angka dan tidak negatif (untuk biaya) |
| `bacaNama()` | Nama tidak boleh kosong dan hanya boleh berisi huruf, spasi, dan titik |
| `bacaJenisKelamin()` | Hanya menerima `Laki-laki` atau `Perempuan` |
| `bacaStatus()` | Hanya menerima `Normal` atau `Tidak Normal` |
| `bacaTeksTidakKosong()` | Teks tidak boleh kosong |

Jika input salah, program meminta pengguna memasukkan ulang tanpa berhenti.

### D. Dummy Data

Saat program pertama kali dijalankan, `isiDataAwal()` mengisi data berikut agar menu *Lihat* langsung menampilkan isi:

| Jenis Data | ID | Isi |
|---|---|---|
| Pasien | `P1` | Aulia Ashylla P, 19 tahun, Perempuan, keluhan demam dan batuk sejak 3 hari |
| Analis | `PT1` | Asti Putri, 29 tahun, spesialisasi Hematologi |
| Dokter | `PT2` | dr. Hanif Amelia Putri, 25 tahun, STR-123456789 |
| Pemeriksaan | `PM1` | Tes Darah Lengkap - Rp150.000 |
| Pemeriksaan | `PM2` | Tes Urine - Rp100.000 |
| Pemeriksaan | `PM3` | Tes Gula Darah - Rp75.000 |
| Hasil Pemeriksaan | `H1` | Pasien `P1`, `PM1`, petugas `PT1`, "Hemoglobin 13.5 g/dL, Leukosit normal", status Normal |

---

## 6. Hasil Output Program 

Berikut tampilan program saat dijalankan, berurutan dari menu utama sampai keluar.

### Menu Utama

Tampilan pertama saat program dijalankan.

<img height="200" alt="image" src="https://github.com/user-attachments/assets/e08d72bc-32c8-4a46-bfd7-ce88df167df1" />


### Menu 1 - Pendaftaran Pemeriksaan

Pendaftaran dengan **pasien baru**: pengguna mengisi data pasien, lalu memilih pemeriksaan dan petugas.

<img height="500" alt="image" src="https://github.com/user-attachments/assets/d0c6835f-c0f3-4ae4-9a47-479a10919cc6" />

Penjelasan alur pada gambar di atas:

1. Pengguna memilih menu **1. Pendaftaran Pemeriksaan**, lalu memilih **1. Pasien Baru**.
2. Program meminta data pasien: nama, umur, jenis kelamin, dan keluhan. Setiap input divalidasi, misalnya jenis kelamin hanya menerima `Laki-laki` atau `Perempuan` (huruf besar/kecil tidak dibedakan).
3. Setelah data valid, pasien disimpan dan mendapat **ID otomatis** (`P2`, karena `P1` sudah dipakai dummy data).
4. Program menampilkan daftar pemeriksaan beserta biayanya, lalu pengguna memasukkan ID pemeriksaan (`PM1`).
5. Program menampilkan daftar petugas, lalu pengguna memasukkan ID petugas (`PT2`). Tampilan daftar ini memperlihatkan hasil **overriding**: `Analis` menampilkan spesialisasi, sedangkan `Dokter` menampilkan nomor STR.

Tampilan **konfirmasi pendaftaran** setelah semua data dipilih.

<img height="215" alt="image" src="https://github.com/user-attachments/assets/6221e7de-250e-428c-8675-93c7998378e2" />


Pendaftaran dengan **pasien yang sudah terdaftar** dan tampilan konfirmasi pendaftaran.

<img height="600" alt="image" src="https://github.com/user-attachments/assets/f9554086-2e57-4a52-80f7-2771cd47b236" />


Penjelasan alur pada gambar di atas:

1. Pengguna memilih **2. Pasien Sudah Terdaftar**, lalu program menampilkan semua pasien (`P1` dan `P2`). Pasien `P2` adalah pasien yang sebelumnya didaftarkan sebagai pasien baru, sehingga terlihat bahwa data tersimpan di `ArrayList`.
2. Pengguna memasukkan ID pasien (`p1`). Pencarian tidak membedakan huruf besar dan kecil (`equalsIgnoreCase`), sehingga `p1` tetap ditemukan sebagai `P1`.
3. Pengguna memilih pemeriksaan (`PM1`) dan petugas (`PT1`). Daftar petugas menampilkan hasil **overriding**: `Analis` menampilkan spesialisasi, sedangkan `Dokter` menampilkan nomor STR.
4. Program menampilkan **konfirmasi pendaftaran** berisi ID dan nama pasien, jenis pemeriksaan, biaya, petugas, serta status `Terdaftar`.
5. Data pendaftaran disimpan pada `pasienTerdaftar`, `pemeriksaanTerdaftar`, dan `petugasTerdaftar` untuk dipakai pada menu **Input Hasil Pemeriksaan**.



### Menu 2 - Kelola Pasien

**Menu 2**

<img height="205" alt="image" src="https://github.com/user-attachments/assets/8bf91a83-2c9f-47aa-8841-f9337e5d3e0c" />

**Tambah pasien.**

<img height="200" alt="image" src="https://github.com/user-attachments/assets/9456b43d-2908-4bd4-9cf2-805b9c8e4f2a" />


**Lihat semua pasien.**

<img height="200" alt="image" src="https://github.com/user-attachments/assets/e69c3b35-7371-4439-b58e-ac83c7d6180a" />



**Cari pasien berdasarkan ID.**

<img height="200" alt="image" src="https://github.com/user-attachments/assets/a63b2cdb-bf74-4d28-b62b-39b92a2b11d3" />


**Hapus pasien.**

<img height="300" alt="image" src="https://github.com/user-attachments/assets/8b07226f-a16f-4626-b5b1-056b92ec2431" />


### Menu 3 - Kelola Petugas

**Menu Petugas**

<img height="200" alt="image" src="https://github.com/user-attachments/assets/f894cbeb-39ae-4cf6-965c-41068a27e47f" />


**Tambah analis.**

<img height="200" alt="image" src="https://github.com/user-attachments/assets/6e467abc-22a5-450a-a7ad-5cd4de95b1d5" />


**Tambah dokter.**

<img height="200" alt="image" src="https://github.com/user-attachments/assets/9ef3e20d-ba4c-493a-9c07-bc76a0bc945b" />


**Lihat semua petugas.** Pada tampilan ini terlihat hasil **overriding**: `Analis` menampilkan spesialisasi, sedangkan `Dokter` menampilkan nomor STR.

<img height="200" alt="image" src="https://github.com/user-attachments/assets/2541d238-741b-4295-adb2-553c22211133" />


### Menu 4 - Kelola Pemeriksaan

**Menu Pemeriksaan**

<img height="300" alt="image" src="https://github.com/user-attachments/assets/1d988ff6-93f9-4cdd-b66e-975602c74b5f" />


**Tambah pemeriksaan.**

<img height="300" alt="image" src="https://github.com/user-attachments/assets/a96b2a2e-fc24-435e-bc07-5c3fa097019d" />


**Lihat semua pemeriksaan.**

<img height="300" alt="image" src="https://github.com/user-attachments/assets/4df5db88-4ea8-4e59-b67d-69aafd375d37" />


**Cari pemeriksaan.**

<img height="300" alt="image" src="https://github.com/user-attachments/assets/eecdf32a-d036-4861-ac4a-fc36a4e3faff" />


**Ubah pemeriksaan.**

<img height="400" alt="image" src="https://github.com/user-attachments/assets/62de538f-b1dd-4389-8f37-c4f3fa8423d4" />

**Hapus pemeriksaan.**

<img height="400" alt="image" src="https://github.com/user-attachments/assets/2a21e46a-a9bf-4de2-a2f9-ee1684ea5011" />

### Menu 5 - Kelola Hasil Pemeriksaan

**Menu Hasil Pemerikasaan**

<img height="200" alt="image" src="https://github.com/user-attachments/assets/71dece68-9187-43c3-b493-39cf59267399" />

**Input hasil pemeriksaan.**

<img height="300" alt="image" src="https://github.com/user-attachments/assets/c1702f36-0e91-4faf-899a-e27454e18335" />


**Lihat semua hasil.**

<img height="400" alt="image" src="https://github.com/user-attachments/assets/2ec5fbac-5815-4fa6-a40a-1c1406d6c424" />


**Lihat riwayat hasil per pasien.**

<img height="400" alt="image" src="https://github.com/user-attachments/assets/cae10999-f5d9-4f28-91bd-71c9ccc1051f" />


### Validasi Input

Contoh ketika pengguna memasukkan input yang salah (misalnya huruf pada kolom umur, atau jenis kelamin yang tidak valid). Program meminta input diulang dan tidak berhenti.

**contoh pada umur:**

<img height="200" alt="image" src="https://github.com/user-attachments/assets/2b3287a8-a402-4cb8-81f9-6f301150e1fa" />

**contoh pada jenis kelamin:**

<img height="200" alt="image" src="https://github.com/user-attachments/assets/f0d20258-6276-439c-baf2-865dc61df61d" />

**contoh pada nama:**

<img height="200" alt="image" src="https://github.com/user-attachments/assets/52eea062-1202-49e4-af27-3ba4c36a9144" />



### Menu 6 - Keluar

**Program menampilkan pesan penutup dan berhenti.**

<img height="200" alt="image" src="https://github.com/user-attachments/assets/4cd9ff28-b650-4b05-82ce-36129abfd574" />


---

## 7. Konsep PBO yang Diterapkan

Program ini menerapkan beberapa konsep Pemrograman Berorientasi Objek, yaitu:

| Konsep | Penerapan |
|---|---|
| Class & Object | Digunakan pada `Pasien`, `Petugas`, `Analis`, `Dokter`, `Pemeriksaan`, dan `HasilPemeriksaan` |
| Constructor | Digunakan untuk membuat object dan mengisi data awal |
| Access Modifier | Atribut menggunakan `private` dan method menggunakan `public` |
| Encapsulation | Data diakses melalui getter dan setter |
| Inheritance | `Analis` dan `Dokter` mewarisi `Petugas` |
| Overriding | `tampilkanInfo()` dan `tampilkanInfo(boolean)` dioverride pada `Analis` dan `Dokter` |
| Overloading | `tampilkanInfo()` dan `tampilkanInfo(boolean)`, serta `bacaInt(Scanner)` dan `bacaInt(Scanner, int, int)` |
| Polymorphism | `Analis` dan `Dokter` disimpan dalam `ArrayList<Petugas>` |
| ArrayList | Digunakan untuk menyimpan data selama program berjalan |
| Validasi | Digunakan untuk memeriksa input pengguna |
| MVC | Program dibagi menjadi bagian Main, Controller, Model, dan View |

---


## 8. Kesimpulan

Program Sistem Manajemen Laboratorium Kesehatan merupakan pengembangan dari Mini Project 1 yang menambahkan penerapan konsep Pemrograman Berorientasi Objek.

Program tidak hanya mengelola data pemeriksaan, tetapi juga menghubungkan data pasien, petugas, pemeriksaan, dan hasil pemeriksaan dalam satu alur.

Penerapan encapsulation, inheritance, overriding, polymorphism, validasi input, `ArrayList`, serta struktur MVC membuat program menjadi lebih terstruktur dan sesuai dengan konsep PBO yang dipelajari.

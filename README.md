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

```java
// Pasien.java
private String id;
private String nama;
private int umur;
private String jenisKelamin;
private String keluhan;
```

Atribut tersebut tidak dapat diakses langsung dari luar class (misalnya dari Controller atau View). Controller dan View harus memakai getter, contohnya `pasien.getNama()` atau `pemeriksaan.getBiaya()`.

**2. Getter dan setter bersifat `public`**

```java
// Pasien.java
public String getNama() {
    return nama;
}

public void setNama(String nama) {
    if (nama != null && !nama.trim().isEmpty()) {
        this.nama = nama;
    } else {
        System.out.println(">> ERROR: Nama tidak boleh kosong!");
    }
}
```

**3. Setter berfungsi sebagai validasi**

Setter tidak hanya mengisi nilai, tetapi juga menyaring data sehingga object selalu berisi data yang valid.

| Setter | Aturan validasi |
|---|---|
| `setNama()`, `setJenisKelamin()`, `setKeluhan()`, `setSpesialisasiBidang()`, `setNomorSTR()`, `setNamaPemeriksaan()`, `setHasil()`, `setStatus()` | Tidak boleh `null` atau kosong |
| `setUmur()` | Harus lebih dari 0 |
| `setBiaya()` | Tidak boleh negatif |

```java
// Pemeriksaan.java
public void setBiaya(double biaya) {
    if (biaya >= 0) {
        this.biaya = biaya;
    } else {
        System.out.println(">> ERROR: Biaya tidak boleh negatif!");
    }
}
```

**4. Konstruktor memakai setter**

Agar validasi juga berlaku saat object dibuat, konstruktor memanggil setter, bukan mengisi atribut secara langsung:

```java
// Pasien.java
public Pasien(String id, String nama, int umur, String jenisKelamin, String keluhan) {
    this.id = id;
    setNama(nama);
    setUmur(umur);
    setJenisKelamin(jenisKelamin);
    setKeluhan(keluhan);
}
```

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

```java
public class Analis extends Petugas { ... }
public class Dokter extends Petugas { ... }
```

Konstruktor subclass memanggil konstruktor superclass dengan `super(...)`, lalu mengisi atribut miliknya sendiri lewat setter:

```java
// Analis.java
public Analis(String id, String nama, int umur, String jenisKelamin, String spesialisasiBidang) {
    super(id, nama, umur, jenisKelamin);      // mengisi atribut milik Petugas
    setSpesialisasiBidang(spesialisasiBidang); // mengisi atribut milik Analis
}
```

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

```java
// Petugas.java (superclass)
public String tampilkanInfo() {
    return "ID: " + id + " | Nama: " + nama;
}

public String tampilkanInfo(boolean detail) {
    if (!detail) {
        return tampilkanInfo();
    }
    return "ID: " + id + " | Nama: " + nama
            + " | Umur: " + umur + " | Jenis Kelamin: " + jenisKelamin;
}
```

Versi di subclass `Analis`:

```java
// Analis.java
@Override
public String tampilkanInfo() {
    return super.tampilkanInfo() + " | Peran: Analis | Spesialisasi/Bidang: " + spesialisasiBidang;
}

@Override
public String tampilkanInfo(boolean detail) {
    if (!detail) {
        return tampilkanInfo();
    }
    return super.tampilkanInfo(true) + " | Peran: Analis | Spesialisasi/Bidang: " + spesialisasiBidang;
}
```

Versi di subclass `Dokter`:

```java
// Dokter.java
@Override
public String tampilkanInfo() {
    return super.tampilkanInfo() + " | Peran: Dokter | No. STR: " + nomorSTR;
}

@Override
public String tampilkanInfo(boolean detail) {
    if (!detail) {
        return tampilkanInfo();
    }
    return super.tampilkanInfo(true) + " | Peran: Dokter | No. STR: " + nomorSTR;
}
```

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

```text
ID: PT1 | Nama: Asti Putri | Umur: 29 | Jenis Kelamin: Laki-Laki | Peran: Analis | Spesialisasi/Bidang: Hematologi
------------------------------------
ID: PT2 | Nama: dr. Hanif Amelia Putri | Umur: 25 | Jenis Kelamin: Perempuan | Peran: Dokter | No. STR: STR-123456789
------------------------------------
```

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

## 6. Hasil Output Program (Screenshot)

Berikut tampilan program saat dijalankan, berurutan dari menu utama sampai keluar.

### Menu Utama

Tampilan pertama saat program dijalankan.

![Menu Utama](screenshots/01-menu-utama.png)

### Menu 1 - Pendaftaran Pemeriksaan

Pendaftaran dengan **pasien baru**: pengguna mengisi data pasien, lalu memilih pemeriksaan dan petugas.

![Pendaftaran Pasien Baru](screenshots/02-pendaftaran-pasien-baru.png)

Pendaftaran dengan **pasien yang sudah terdaftar** dan tampilan konfirmasi pendaftaran.

![Pendaftaran Pasien Terdaftar dan Konfirmasi](screenshots/03-pendaftaran-konfirmasi.png)

### Menu 2 - Kelola Pasien

Tambah pasien.

![Tambah Pasien](screenshots/04-tambah-pasien.png)

Lihat semua pasien.

![Lihat Semua Pasien](screenshots/05-lihat-pasien.png)

Cari pasien berdasarkan ID.

![Cari Pasien](screenshots/06-cari-pasien.png)

Hapus pasien.

![Hapus Pasien](screenshots/07-hapus-pasien.png)

### Menu 3 - Kelola Petugas

Tambah analis.

![Tambah Analis](screenshots/08-tambah-analis.png)

Tambah dokter.

![Tambah Dokter](screenshots/09-tambah-dokter.png)

Lihat semua petugas. Pada tampilan ini terlihat hasil **overriding**: `Analis` menampilkan spesialisasi, sedangkan `Dokter` menampilkan nomor STR.

![Lihat Semua Petugas](screenshots/10-lihat-petugas.png)

### Menu 4 - Kelola Pemeriksaan

Tambah pemeriksaan.

![Tambah Pemeriksaan](screenshots/11-tambah-pemeriksaan.png)

Lihat semua pemeriksaan.

![Lihat Semua Pemeriksaan](screenshots/12-lihat-pemeriksaan.png)

Cari pemeriksaan.

![Cari Pemeriksaan](screenshots/13-cari-pemeriksaan.png)

Ubah pemeriksaan.

![Ubah Pemeriksaan](screenshots/14-ubah-pemeriksaan.png)

Hapus pemeriksaan.

![Hapus Pemeriksaan](screenshots/15-hapus-pemeriksaan.png)

### Menu 5 - Kelola Hasil Pemeriksaan

Input hasil pemeriksaan.

![Input Hasil Pemeriksaan](screenshots/16-input-hasil.png)

Lihat semua hasil.

![Lihat Semua Hasil](screenshots/17-lihat-hasil.png)

Lihat riwayat hasil per pasien.

![Riwayat Hasil per Pasien](screenshots/18-riwayat-hasil.png)

### Validasi Input

Contoh ketika pengguna memasukkan input yang salah (misalnya huruf pada kolom umur, atau jenis kelamin yang tidak valid). Program meminta input diulang dan tidak berhenti.

![Validasi Input](screenshots/19-validasi-input.png)

### Menu 6 - Keluar

Program menampilkan pesan penutup dan berhenti.

![Keluar Program](screenshots/20-keluar.png)

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

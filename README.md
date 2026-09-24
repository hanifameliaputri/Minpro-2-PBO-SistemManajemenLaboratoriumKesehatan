# Minpro-2-PBO-SistemManajemenLaboratoriumKesehatan

**Nama:** Hanif Amelia Putri  
**Kelas:** B  
**NIM:** 2509116075  

## 1. Deskripsi Program

Sistem Manajemen Laboratorium Kesehatan adalah program berbasis Java yang digunakan untuk mengelola data pasien, petugas laboratorium, jenis pemeriksaan, dan hasil pemeriksaan.

Program ini merupakan pengembangan dari Mini Project 1 dengan menerapkan beberapa konsep Pemrograman Berorientasi Objek (PBO), seperti **encapsulation, inheritance, overriding, polymorphism, constructor, access modifier, getter dan setter**, serta validasi input.

Program juga menggunakan `ArrayList` untuk menyimpan data selama program berjalan dan menyediakan dummy data agar pengguna dapat langsung melihat data ketika program dijalankan.

---

## 2. Fitur Program

Program memiliki beberapa fitur utama, yaitu:

- Pendaftaran pemeriksaan
- Mengelola data pasien
- Mengelola data petugas
- Mengelola data pemeriksaan
- Mengelola hasil pemeriksaan
- Menambah, melihat, mencari, mengubah, dan menghapus data tertentu
- Validasi input pengguna
- Menampilkan riwayat hasil pemeriksaan berdasarkan pasien
- Menggunakan dummy data pada saat program pertama kali dijalankan

---

## 3. Alur Program

Program dijalankan melalui class `Main.java`. Class tersebut memanggil `LaboratoriumController` untuk menjalankan program.

Alur utama program:

```text
Main
  ↓
LaboratoriumController
  ↓
Menu Utama
  ├── 1. Pendaftaran Pemeriksaan
  ├── 2. Kelola Pasien
  ├── 3. Kelola Petugas
  ├── 4. Kelola Pemeriksaan
  ├── 5. Kelola Hasil Pemeriksaan
  └── 6. Keluar
```

### 1. Pendaftaran Pemeriksaan

Pada menu ini pengguna dapat melakukan pendaftaran pemeriksaan.

Pengguna dapat memilih:

- Pasien baru
- Pasien yang sudah terdaftar

Setelah pasien dipilih, pengguna memilih jenis pemeriksaan dan petugas yang menangani pemeriksaan.

Setelah semua data dipilih, sistem menampilkan konfirmasi pendaftaran.

### 2. Kelola Pasien

Menu ini digunakan untuk mengelola data pasien.

Fitur yang tersedia:

- Tambah pasien
- Lihat semua pasien
- Cari pasien
- Hapus pasien

### 3. Kelola Petugas

Menu ini digunakan untuk mengelola data petugas laboratorium.

Pengguna dapat menambahkan:

- Analis
- Dokter

Data kedua jenis petugas tersebut disimpan dalam satu `ArrayList<Petugas>`.

### 4. Kelola Pemeriksaan

Menu ini digunakan untuk mengelola jenis pemeriksaan laboratorium.

Fitur yang tersedia:

- Tambah pemeriksaan
- Lihat semua pemeriksaan
- Cari pemeriksaan
- Ubah pemeriksaan
- Hapus pemeriksaan

### 5. Kelola Hasil Pemeriksaan

Menu ini digunakan untuk mencatat dan melihat hasil pemeriksaan pasien.

Fitur yang tersedia:

- Input hasil pemeriksaan
- Lihat semua hasil
- Lihat riwayat hasil berdasarkan pasien

### 6. Keluar

Menu ini digunakan untuk menghentikan program.

---

## 4. Struktur Program

Program menggunakan pembagian package untuk memisahkan bagian-bagian program.

```text
LaboratoriumKesehatan
│
├── Main
│   └── Main.java
│
├── controller
│   └── LaboratoriumController.java
│
├── model
│   ├── Pasien.java
│   ├── Petugas.java
│   ├── Analis.java
│   ├── Dokter.java
│   ├── Pemeriksaan.java
│   └── HasilPemeriksaan.java
│
└── view
    └── LaboratoriumView.java
```

### Fungsi setiap package

**Main**

Digunakan sebagai titik awal program. `Main.java` membuat objek `LaboratoriumController` dan menjalankan program.

**Controller**

`LaboratoriumController` mengatur proses program, seperti input data, pengelolaan `ArrayList`, pencarian, penambahan, perubahan, penghapusan, serta proses pendaftaran pemeriksaan.

**Model**

Berisi class yang merepresentasikan data dalam program, yaitu pasien, petugas, analis, dokter, pemeriksaan, dan hasil pemeriksaan.

**View**

`LaboratoriumView` digunakan untuk menampilkan menu, judul, pilihan, informasi data, dan pesan kepada pengguna.

---

## 5. Penerapan Encapsulation

Encapsulation diterapkan dengan membuat atribut pada class menjadi `private`.

Contohnya pada class `Pasien`:

```java
private String id;
private String nama;
private int umur;
private String jenisKelamin;
private String keluhan;
```

Atribut tersebut tidak dapat diakses secara langsung dari luar class. Untuk mengakses atau mengubah nilainya digunakan getter dan setter.

Contohnya:

```java
public String getNama() {
    return nama;
}

public void setNama(String nama) {
    if (nama != null && !nama.trim().isEmpty()) {
        this.nama = nama;
    }
}
```

Selain sebagai akses data, setter juga digunakan untuk melakukan validasi.

Contohnya umur tidak boleh bernilai 0 atau negatif:

```java
public void setUmur(int umur) {
    if (umur > 0) {
        this.umur = umur;
    }
}
```

Dengan demikian, data pada object tetap dikontrol melalui method yang telah disediakan.

---

## 6. Penerapan Inheritance

Inheritance diterapkan pada class `Petugas`, `Analis`, dan `Dokter`.

`Petugas` digunakan sebagai superclass, sedangkan `Analis` dan `Dokter` menjadi subclass.

Strukturnya:

```text
          Petugas
          /     \
         /       \
      Analis    Dokter
```

Class `Petugas` memiliki atribut umum:

```java
private String id;
private String nama;
private int umur;
private String jenisKelamin;
```

Kemudian class `Analis` mewarisi class `Petugas` menggunakan:

```java
public class Analis extends Petugas
```

Sedangkan class `Dokter` menggunakan:

```java
public class Dokter extends Petugas
```

Selain mewarisi atribut dan method dari `Petugas`, masing-masing subclass memiliki atribut khusus.

Pada `Analis` terdapat:

```java
private String spesialisasiBidang;
```

Sedangkan pada `Dokter` terdapat:

```java
private String nomorSTR;
```

---

## 7. Penerapan Overriding

Overriding diterapkan pada method `tampilkanInfo()` yang terdapat pada class `Petugas`.

Pada class `Petugas` terdapat:

```java
public String tampilkanInfo() {
    return "ID: " + id + " | Nama: " + nama;
}
```

Kemudian method tersebut dioverride oleh class `Analis`:

```java
@Override
public String tampilkanInfo() {
    return super.tampilkanInfo()
            + " | Peran: Analis | Spesialisasi/Bidang: "
            + spesialisasiBidang;
}
```

Method tersebut juga dioverride oleh class `Dokter`:

```java
@Override
public String tampilkanInfo() {
    return super.tampilkanInfo()
            + " | Peran: Dokter | No. STR: "
            + nomorSTR;
}
```

Dengan overriding, masing-masing subclass dapat memberikan tampilan informasi yang berbeda sesuai dengan jenis petugasnya.

---

## 8. Penerapan Polymorphism

Polymorphism diterapkan ketika object `Analis` dan `Dokter` disimpan dalam `ArrayList<Petugas>`.

Contohnya:

```java
private final ArrayList<Petugas> daftarPetugas;
```

Karena `Analis` dan `Dokter` merupakan turunan dari `Petugas`, keduanya dapat dimasukkan ke dalam `ArrayList<Petugas>`.

Ketika data petugas ditampilkan:

```java
for (Petugas p : daftarPetugas) {
    view.tampilkanBaris(p.tampilkanInfo(true));
}
```

Program akan menjalankan method `tampilkanInfo()` sesuai dengan object sebenarnya.

Jika object merupakan `Analis`, informasi yang ditampilkan akan menggunakan versi `Analis`.

Jika object merupakan `Dokter`, informasi yang ditampilkan akan menggunakan versi `Dokter`.

Hal tersebut menunjukkan penerapan polymorphism dalam program.

---

## 9. Validasi Input

Program menerapkan validasi input agar data yang dimasukkan pengguna sesuai dengan ketentuan.

Beberapa validasi yang diterapkan antara lain:

### Validasi Nama

Nama tidak boleh kosong:

```java
if (nama != null && !nama.trim().isEmpty()) {
    this.nama = nama;
}
```

### Validasi Umur

Umur harus lebih dari 0:

```java
if (umur > 0) {
    this.umur = umur;
}
```

### Validasi Biaya

Biaya pemeriksaan tidak boleh negatif:

```java
if (biaya >= 0) {
    this.biaya = biaya;
}
```

### Validasi Input Teks

Program juga memiliki method untuk memastikan input teks tidak kosong sehingga pengguna tidak dapat memasukkan data kosong pada bagian yang diperlukan.

Validasi ini membantu mengurangi kesalahan ketika pengguna memasukkan data ke dalam program.

---

## 10. Dummy Data

Program menyediakan dummy data yang dimasukkan ketika program pertama kali dijalankan.

Dummy data digunakan agar data sudah tersedia ketika pengguna memilih menu lihat tanpa harus memasukkan data terlebih dahulu.

Data awal yang disediakan mencakup:

- Data pasien
- Data analis
- Data dokter
- Data pemeriksaan
- Data hasil pemeriksaan

Contoh data petugas disimpan dalam:

```java
ArrayList<Petugas>
```

Sedangkan data pasien, pemeriksaan, dan hasil pemeriksaan masing-masing disimpan dalam `ArrayList` sesuai dengan class-nya.

---

## 11. Konsep PBO yang Diterapkan

Program ini menerapkan beberapa konsep Pemrograman Berorientasi Objek, yaitu:

| Konsep | Penerapan |
|---|---|
| Class & Object | Digunakan pada `Pasien`, `Petugas`, `Analis`, `Dokter`, `Pemeriksaan`, dan `HasilPemeriksaan` |
| Constructor | Digunakan untuk membuat object dan mengisi data awal |
| Access Modifier | Atribut menggunakan `private` dan method menggunakan `public` |
| Encapsulation | Data diakses melalui getter dan setter |
| Inheritance | `Analis` dan `Dokter` mewarisi `Petugas` |
| Overriding | `tampilkanInfo()` dioverride pada `Analis` dan `Dokter` |
| Polymorphism | `Analis` dan `Dokter` disimpan dalam `ArrayList<Petugas>` |
| ArrayList | Digunakan untuk menyimpan data selama program berjalan |
| Validasi | Digunakan untuk memeriksa input pengguna |
| MVC | Program dibagi menjadi bagian Main, Controller, Model, dan View |

---

## 12. Kesimpulan

Program Sistem Manajemen Laboratorium Kesehatan merupakan pengembangan dari Mini Project 1 yang menambahkan penerapan konsep Pemrograman Berorientasi Objek.

Program tidak hanya mengelola data pemeriksaan, tetapi juga menghubungkan data pasien, petugas, pemeriksaan, dan hasil pemeriksaan dalam satu alur.

Penerapan encapsulation, inheritance, overriding, polymorphism, validasi input, `ArrayList`, serta struktur MVC membuat program menjadi lebih terstruktur dan sesuai dengan konsep PBO yang dipelajari.
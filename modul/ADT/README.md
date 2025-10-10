# Modul 3: ABSTRACT DATA TYPE (ADT)
---

## TUJUAN PRAKTIKUM

1. Memahami konsep **Abstract Data Type (ADT)** dan penggunaannya dalam pemrograman.
2. Mengetahui bagaimana cara **membuat, menggunakan, dan memisahkan ADT** dalam Python.
3. Mampu mengimplementasikan **ADT mahasiswa** dan **ADT pelajaran** secara modular.
4. Memahami peran ADT sebagai dasar untuk mempelajari struktur data yang lebih kompleks.

---

## 3.1 Pengantar Abstract Data Type (ADT)

### Definisi Tipe Data Abstrak (ADT)
Tipe Data Abstrak (Abstract Data Type/ADT) adalah model matematis atau spesifikasi logis dari sebuah tipe data. ADT mendefinisikan tipe data hanya berdasarkan perilaku dan operasi yang didukungnya, tanpa menentukan detail bagaimana data tersebut disimpan atau operasi tersebut diimplementasikan.

Sederhananya, ADT adalah kontrak atau cetak biru yang menjawab pertanyaan: "Apa yang dapat dilakukan oleh tipe data ini?"

Tiga Komponen Utama ADT:

1. Data (Nilai): Kumpulan nilai yang dapat ditampung oleh ADT.

2. Operasi (Behavior): Kumpulan fungsi atau metode yang mendefinisikan cara data dapat diakses, dimodifikasi, atau dikelola.

2. Aksioma (Hukum): Kondisi atau aturan yang mengatur bagaimana operasi-operasi tersebut berinteraksi dan memelihara konsistensi data.

### Abstraksi
**Abstraksi** berarti menyembunyikan detail implementasi dari pengguna. Seorang pengguna ADT (seorang programmer yang menggunakan ADT Stack, misalnya) hanya perlu tahu cara memanggil operasi `push()` dan `pop()`. Mereka tidak perlu tahu apakah data Stack disimpan menggunakan array dinamis (seperti list Python) atau menggunakan linked list.

Ini memungkinkan kita untuk berpikir pada level yang lebih tinggi (logika) tanpa terbebani oleh detail tingkat rendah (fisik penyimpanan).

### Contoh Analogi:
Bayangkan Remote TV sebagai ADT.

- Operasi (ADT): Tombol Volume Naik, Tombol Ganti Channel, Tombol Power.

- Detail Implementasi (Struktur Data): Sirkuit internal, kabel, sensor inframerah.

Anda, sebagai pengguna, hanya peduli dengan operasi remote (menaikkan volume). Anda tidak perlu tahu sirkuit internalnya. Jika pabrikan mengubah sirkuit internal (mengganti struktur data), asalkan tombolnya (operasi ADT) bekerja sama, Anda tidak terpengaruh.

---

### Komponen ADT

| Jenis Operasi | Deskripsi | Penamaan Umum |
|----------------|------------|----------------|
| **Konstruktor / Kreator** | Membentuk objek bertipe ADT | Harus sama dengan nama kelas (misalnya, `Mahasiswa::Mahasiswa(...)`).|
| **Selector / Getter** | Mengambil nilai komponen | `get_...` |
| **Mutator / Setter** | Mengubah nilai komponen | `set_...` |
| **Validator** | Memeriksa keabsahan nilai | `is_NamaKondisi()` |
| **Destruktor** | Menghapus atau membebaskan memori (Python otomatis dengan garbage collector) | `__del__` |
| **Input / Output** | Membaca dan menampilkan nilai objek | `display()`, `print()` |
| **Operator Relasional** | Perbandingan antar objek | `<`, `==`, dsb |
| **Konversi Tipe** | Mengubah ke tipe dasar dan sebaliknya | `to_TipeDasar()`|

---

## 3.2 ADT dalam Konteks CPP

### Hubungan ADT dan Data Structure
| ADT | Contoh Struktur Data di Python |
|------|-----------------------------|
| Stack | `std::stack` (sebagai adaptor yang menggunakan `std::deque` atau `std::vector` secara default), `std::vector`|
| Queue | `std::queue` (sebagai adaptor yang menggunakan `std::deque` secara default), `std::deque` |
| Set | `std::set` |
| Map | `std::map` |
| List | `std::vector`, `std::list` |

### Perbedaan ADT dan Structure data

| Aspek | Tipe Data Abstrak (ADT) | Struktur Data (Data Structure) |
|----------------|------------|----------------|
| **Definisi** | Model logis, kontrak perilaku. | Implementasi fisik data dan operasi. |
| **Pertanyaan** | APA yang bisa dilakukan? | BAGAIMANA itu dilakukan? |
| **Sifat** | Konseptual, Teoritis (Bebas Bahasa Pemrograman). | Konkret, Kode Aktual (Tergantung Bahasa Pemrograman). |
| **contoh** | Stack, Queue, List, Dictionary. | Array, Linked List, Hash Table, Binary Tree. |


---

## 3.3 Contoh Implementasi ADT di cpp
---
### contoh 1: struct

file .cpp
```cpp
#include <iostream>
#include <string>
using namespace std;

// Definisi struct Mahasiswa sebagai ADT
struct Mahasiswa {
    string nama;
    int usia;
    float nilai;
};

// Fungsi untuk mengisi data mahasiswa
void setMahasiswa(Mahasiswa &mhs, string nama, int usia, float nilai) {
    mhs.nama = nama;
    mhs.usia = usia;
    mhs.nilai = nilai;
}

// Fungsi untuk menampilkan data mahasiswa
void displayMahasiswa(const Mahasiswa &mhs) {
    cout << "Nama: " << mhs.nama << endl;
    cout << "Usia: " << mhs.usia << " tahun" << endl;
    cout << "Nilai: " << mhs.nilai << endl;
}

int main() {
    // Membuat objek Mahasiswa
    Mahasiswa mhs1;

    // Mengisi data mahasiswa
    setMahasiswa(mhs1, "Budi", 20, 85.5);

    // Menampilkan data mahasiswa
    displayMahasiswa(mhs1);

    return 0;
}
```

---
### Contoh 2: Class and Function

file .hpp
```cpp
#include <iostream>
#include <string>
#include <map> // Mengganti dictionary Python

class Mahasiswa {
private:
    // Atribut bersifat private (Enkapsulasi ADT C++)
    std::string nama;
    int usia;
    float nilai;

public:
    // 1. Konstruktor 
    Mahasiswa(const std::string& nama_mhs, int usia_mhs, float nilai_mhs)
        : nama(nama_mhs), usia(usia_mhs), nilai(nilai_mhs) {}

    // 2. Mutator/Setter: Mengatur ulang data mahasiswa
    void set_data(const std::string& nama_mhs, int usia_mhs, float nilai_mhs) {
        nama = nama_mhs;
        usia = usia_mhs;
        nilai = nilai_mhs;
    }

    // 3. Selector/Getter: Mengembalikan seluruh data 
    std::map<std::string, std::string> get_data() const {
        // Menggunakan map<string, string> untuk kesederhanaan, mengubah usia dan nilai menjadi string
        return {
            {"nama", nama},
            {"usia", std::to_string(usia)},
            {"nilai", std::to_string(nilai)}
        };
    }

    // 4. Input/Output: Menampilkan data mahasiswa
    void tampilkan_data() const {
        std::cout << "Nama  : " << nama << std::endl;
        std::cout << "Usia  : " << usia << " tahun" << std::endl;
        std::cout << "Nilai : " << nilai << std::endl;
    }

    // 5. Validator: Mengecek kelulusan (dengan nilai default)
    bool is_lulus(float passing_grade = 60.0f) const {
        return nilai >= passing_grade;
    }
};
```
file .cpp
```cpp
#include "Mahasiswa.hpp" // Memasukkan definisi ADT

int main() {
    // Membuat objek Mahasiswa (menggunakan Konstruktor)
    Mahasiswa mhs1("Budi", 20, 85.5f);

    // Menampilkan data mahasiswa (Mengganti tampilkan_data)
    std::cout << "=== Data Mahasiswa ===" << std::endl;
    mhs1.tampilkan_data();

    // Mengecek kelulusan (Menggunakan is_lulus)
    if (mhs1.is_lulus()) {
        std::cout << "Status: Lulus" << std::endl;
    } else {
        std::cout << "Status: Tidak Lulus" << std::endl;
    }

    // --- Mengubah data mahasiswa ---
    std::cout << "\n--- Mengubah data mahasiswa ---" << std::endl;
    mhs1.set_data("Budi", 20, 55.0f); // Mengubah nilai menjadi 55.0

    // Menampilkan data setelah diubah
    mhs1.tampilkan_data();
    std::cout << "Status: " << (mhs1.is_lulus() ? "Lulus" : "Tidak Lulus") << std::endl;

    // Mengambil data dalam bentuk map (abstraksi)
    std::map<std::string, std::string> data = mhs1.get_data();
    std::cout << "\nMap Data Mahasiswa: { ";
    bool first = true;
    for (const auto& pair : data) {
        if (!first) std::cout << ", ";
        std::cout << "\"" << pair.first << "\": \"" << pair.second << "\"";
        first = false;
    }
    std::cout << " }" << std::endl;

    return 0;
}
```
---
### Contoh 3: Queue
**Queue** (Antrian) adalah ADT koleksi terurut di mana penambahan elemen baru (operasi `enqueue` atau `insert`) hanya terjadi di satu ujung yang disebut Rear (Belakang), dan penghapusan elemen (operasi `dequeue` atau `remove`) hanya terjadi di ujung yang berlawanan, yang disebut Front (Depan). Queue mengikuti prinsip FIFO (First-In, First-Out)—elemen yang pertama masuk adalah yang pertama keluar.

file .hpp
```cpp
#include <iostream>
#include <deque>
#include <stdexcept>
#include <string>

// Mendefinisikan ADT Queue sebagai template class
template <typename T>
class Queue {
private:
    // Detail Implementasi (disembunyikan): std::deque lebih efisien untuk Queue (pop dari depan)
    std::deque<T> items;
    int max_size;

public:
    // 1. Konstruktor
    // Max size 0 berarti ukuran tidak terbatas (default)
    Queue(int size = 0) : max_size(size) {}

    // 2. Primitif Utama: enqueue (Menambahkan ke belakang)
    void enqueue(const T& item) {
        if (is_full()) {
            std::cout << "Queue penuh! Tidak bisa menambah data." << std::endl;
            return;
        }
        items.push_back(item);
        std::cout << "Enqueue: " << item << std::endl;
    }

    // 3. Primitif Utama: dequeue (Menghapus dari depan)
    T dequeue() {
        if (is_empty()) {
            // Melemparkan exception jika Queue kosong
            throw std::out_of_range("Queue kosong! Tidak bisa menghapus data.");
        }
        T removed = items.front(); // Ambil elemen terdepan
        items.pop_front();         // Hapus elemen terdepan
        std::cout << "Dequeue: " << removed << std::endl;
        return removed;
    }

    // 4. Selector: peek (Melihat elemen terdepan)
    T peek() const {
        if (is_empty()) {
            throw std::out_of_range("Queue kosong! Tidak bisa peek.");
        }
        std::cout << "Elemen terdepan: " << items.front() << std::endl;
        return items.front();
    }

    // 5. Validator: is_empty
    bool is_empty() const {
        return items.empty();
    }

    // 6. Validator: is_full
    bool is_full() const {
        // Jika max_size 0, Queue dianggap tidak penuh (unlimited)
        return max_size > 0 && items.size() >= max_size;
    }

    // 7. Display
    void display() const {
        if (is_empty()) {
            std::cout << "Queue kosong." << std::endl;
        } else {
            std::cout << "Isi Queue (depan ke belakang): [";
            for (size_t i = 0; i < items.size(); ++i) {
                std::cout << items[i];
                if (i < items.size() - 1) {
                    std::cout << ", ";
                }
            }
            std::cout << "]" << std::endl;
        }
    }
};

// Catatan: create_queue() Python diganti dengan langsung memanggil Konstruktor C++
```
file .cpp
```cpp
#include "Queue.hpp" // Memasukkan definisi ADT Queue

int main() {
    // Membuat objek Queue (menggunakan tipe string) dengan kapasitas maksimal 5 elemen
    // Menggantikan Queue.create_queue(5)
    Queue<std::string> antrian(5);

    std::cout << "--- Operasi Enqueue ---" << std::endl;
    antrian.enqueue("Andi");
    antrian.enqueue("Budi");
    antrian.enqueue("Citra");

    antrian.display();
    try {
        antrian.peek();
    } catch (const std::out_of_range& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    std::cout << "\n--- Operasi Dequeue ---" << std::endl;
    try {
        antrian.dequeue(); // Andi keluar
    } catch (const std::out_of_range& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }
    antrian.display();

    std::cout << "\n--- Menambahkan hingga Penuh ---" << std::endl;
    antrian.enqueue("Dewi"); // 4/5
    antrian.enqueue("Eka");  // 5/5
    antrian.enqueue("Fajar"); // Mencoba menambah Fajar (Queue Penuh)

    antrian.display();

    // Contoh penggunaan pada Queue kosong
    // try {
    //     Queue<int> q_kosong(1);
    //     q_kosong.dequeue(); 
    // } catch (const std::out_of_range& e) {
    //     std::cerr << "\nContoh Error: " << e.what() << std::endl;
    // }

    return 0;
}
```
### Contoh 4: Stack

Stack adalah ADT yang mengikuti prinsip LIFO (Last-In, First-Out) — elemen yang terakhir dimasukkan akan menjadi yang pertama keluar. Stack memiliki dua operasi utama:

`push()` → menambahkan elemen ke puncak stack.

`pop()` → menghapus elemen dari puncak stack.

Analogi sederhana: tumpukan piring. Piring yang terakhir diletakkan di atas akan menjadi yang pertama diambil.

file .hpp
```cpp
#include <iostream>
#include <vector>
#include <algorithm> // Untuk std::reverse (pada display)
#include <stdexcept> // Untuk exception handling

// Mendefinisikan ADT Stack sebagai template class
template <typename T>
class Stack {
private:
    // Detail Implementasi (disembunyikan): std::vector efisien untuk operasi LIFO
    std::vector<T> items;
    int max_size;

public:
    // 1. Konstruktor
    // max_size 0 berarti ukuran tidak terbatas (default)
    Stack(int size = 0) : max_size(size) {}

    // 2. Operasi Utama: push (Menambahkan ke atas)
    void push(const T& item) {
        if (is_full()) {
            std::cout << "Stack penuh! Tidak bisa menambahkan data." << std::endl;
            return;
        }
        items.push_back(item);
        std::cout << "Push: " << item << std::endl;
    }

    // 3. Operasi Utama: pop (Menghapus dari atas)
    T pop() {
        if (is_empty()) {
            // Melemparkan exception jika Stack kosong
            throw std::out_of_range("Stack kosong! Tidak bisa menghapus data.");
        }
        T removed = items.back(); // Ambil elemen teratas
        items.pop_back();        // Hapus elemen teratas
        std::cout << "Pop: " << removed << std::endl;
        return removed;
    }

    // 4. Selector: peek (Melihat elemen teratas)
    // Dideklarasikan const karena tidak memodifikasi data
    T peek() const {
        if (is_empty()) {
            throw std::out_of_range("Stack kosong! Tidak bisa peek.");
        }
        const T& top_item = items.back();
        std::cout << "Elemen teratas: " << top_item << std::endl;
        return top_item;
    }

    // 5. Validator: is_empty
    bool is_empty() const {
        return items.empty();
    }

    // 6. Validator: is_full
    bool is_full() const {
        // Jika max_size 0, Stack dianggap tidak penuh
        return max_size > 0 && items.size() >= max_size;
    }

    // 7. Display
    void display() const {
        if (is_empty()) {
            std::cout << "Stack kosong." << std::endl;
        } else {
            std::cout << "Isi Stack (atas ke bawah): [";
            // Menampilkan dari belakang ke depan (atas ke bawah)
            for (int i = items.size() - 1; i >= 0; --i) {
                std::cout << items[i];
                if (i > 0) {
                    std::cout << ", ";
                }
            }
            std::cout << "]" << std::endl;
        }
    }
};
```

file .cpp
```cpp
#include "Stack.hpp" // Memasukkan definisi ADT Stack
#include <string>
#include <stdexcept>

int main() {
    // Membuat objek Stack (menggunakan tipe string) dengan kapasitas maksimal 5 elemen
    // Menggantikan stack = Stack.create_stack(5)
    Stack<std::string> stack(5);

    std::cout << "--- Operasi Push ---" << std::endl;
    stack.push("A");
    stack.push("B");
    stack.push("C");

    stack.display();

    try {
        stack.peek();
    } catch (const std::out_of_range& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    std::cout << "\n--- Operasi Pop ---" << std::endl;
    try {
        stack.pop(); // C keluar
    } catch (const std::out_of_range& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }
    stack.display();

    std::cout << "\n--- Menambahkan hingga Penuh ---" << std::endl;
    stack.push("D"); // 4/5
    stack.push("E"); // 5/5
    stack.push("F"); // Mencoba menambah F (Stack Penuh)

    stack.display();
    
    // Contoh penggunaan pop pada stack kosong
    // try {
    //     Stack<int> s_kosong(1);
    //     s_kosong.pop(); 
    // } catch (const std::out_of_range& e) {
    //     std::cerr << "\nContoh Error: " << e.what() << std::endl;
    // }

    return 0;
}
```

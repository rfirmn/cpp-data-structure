# Modul 4: Singly Linked List (Bagian Pertama)

## 🎯 Tujuan Praktikum

1. Memahami penggunaan linked list dengan pointer dan operator dalam program.
2. Memahami operasi-operasi dasar dalam linked list.
3. Membuat program menggunakan linked list sesuai dengan prototype yang ada.

---

## 4.1 Linked List dengan Pointer

Linked list adalah struktur data dinamis yang terdiri dari elemen-elemen yang saling terhubung menggunakan **pointer**. Struktur ini fleksibel karena dapat bertambah atau berkurang sesuai kebutuhan.

### Keunggulan Linked List dengan Pointer

* **Dinamis:** Tidak perlu menentukan ukuran di awal seperti array.
* **Fleksibel:** Mudah menambah atau menghapus elemen di tengah list.
* **Efisien:** Tidak perlu realokasi seluruh data saat kapasitas berubah.

### Perbandingan dengan Array

| Aspek                   | Array             | Linked List         |
| ----------------------- | ----------------- | ------------------- |
| Alokasi memori          | Statis            | Dinamis             |
| Ukuran                  | Tetap             | Fleksibel           |
| Akses elemen            | Langsung (indeks) | Berurutan (pointer) |
| Insert/Delete di tengah | Sulit             | Mudah               |

### Model ADT Linked List

1. Singly Linked List
2. Doubly Linked List
3. Circular Linked List
4. Multi Linked List
5. Stack (Tumpukan)
6. Queue (Antrean)
7. Tree
8. Graph

### Operasi Dasar ADT Linked List

1. Create List
2. Insert
3. Delete
4. View (Traversal)
5. Searching
6. Update

---

## 4.2 Singly Linked List

Singly Linked List memiliki arah pointer **satu arah** (hanya ke node berikutnya).

### Komponen Elemen

* **Data:** Informasi utama dalam node.
* **Successor (Next):** Pointer menuju node berikutnya.

### Sifat Singly Linked List

1. Hanya membutuhkan satu pointer `next`.
2. Node terakhir menunjuk ke `NULL`.
3. Traversal hanya dapat dilakukan **maju**.
4. Cocok untuk operasi sisip/hapus di tengah list.

### Istilah Penting

* **first/head:** Menunjuk ke elemen pertama.
* **next:** Penunjuk elemen berikutnya.
* **NULL:** Menandakan akhir list.
* **node/simpul/elemen:** Struktur data yang menyimpan `info` dan `next`.

---

## Contoh Struktur Data Singly Linked List (C++)

```cpp
#ifndef LIST_H_INCLUDED
#define LIST_H_INCLUDED
#define Nil NULL
using namespace std;

typedef int infotype;
typedef struct elmlist *address;

struct elmlist {
    infotype info;
    address next;
};

struct list {
    address first;
};

#endif // LIST_H_INCLUDED
```

### Contoh untuk Data Mahasiswa

```cpp
#ifndef LIST_H_INCLUDED
#define LIST_H_INCLUDED
#define Nil NULL
using namespace std;

struct mahasiswa {
    char nama[30];
    char nim[10];
};

typedef mahasiswa infotype;
typedef struct elmlist *address;

struct elmlist {
    infotype info;
    address next;
};

struct list {
    address first;
};

#endif // LIST_H_INCLUDED
```

---

## ⚙️ 4.2.1 Pembentukan Komponen List

### A. Create List

Membuat list baru dengan `first = Nil`.

```cpp
void createList(list &L) {
    L.first = Nil;
}
```

### B. Alokasi Memori

Mengalokasikan node baru menggunakan `new`.

```cpp
address alokasi(mahasiswa M) {
    address P = new elmlist;
    P->info = M;
    P->next = Nil;
    return P;
}
```

### C. Dealokasi

Menghapus node dari memori.

```cpp
void dealokasi(address P) {
    delete P;
}
```

### D. Pengecekan List Kosong

```cpp
bool isEmpty(list L) {
    return (L.first == Nil);
}
```

---

## 🔗 4.2.2 Operasi Insert

### A. Insert First

Menambahkan elemen di awal list.

```cpp
void insertFirst(list &L, address P) {
    P->next = L.first;
    L.first = P;
}
```

### B. Insert Last

Menambahkan elemen di akhir list.

```cpp
void insertLast(list &L, address P) {
    if (L.first == Nil) {
        L.first = P;
    } else {
        address Q = L.first;
        while (Q->next != Nil) {
            Q = Q->next;
        }
        Q->next = P;
    }
}
```

### C. Insert After

Menambahkan elemen setelah node tertentu.

```cpp
void insertAfter(address Prec, address P) {
    P->next = Prec->next;
    Prec->next = P;
}
```

---

## 4.2.3 View List

Menampilkan seluruh isi node dalam list.

```cpp
void printInfo(list L) {
    address P = L.first;
    while (P != Nil) {
        cout << P->info << endl;
        P = P->next;
    }
}
```

Menghitung jumlah elemen dalam list:

```cpp
int nbList(list L) {
    int count = 0;
    address P = L.first;
    while (P != Nil) {
        count++;
        P = P->next;
    }
    return count;
}
```

---

## Ringkasan

| Operasi         | Fungsi              | Deskripsi                         |
| --------------- | ------------------- | --------------------------------- |
| `createList()`  | Membuat list kosong | Inisialisasi awal list            |
| `alokasi()`     | Membuat node baru   | Menyediakan memori untuk data     |
| `dealokasi()`   | Menghapus node      | Mengembalikan memori ke sistem    |
| `isEmpty()`     | Mengecek list       | Mengecek apakah list kosong       |
| `insertFirst()` | Tambah di awal      | Menambahkan node di depan         |
| `insertLast()`  | Tambah di akhir     | Menambahkan node di belakang      |
| `insertAfter()` | Tambah di tengah    | Menambahkan setelah node tertentu |
| `printInfo()`   | Tampilkan list      | Traversal dan cetak data          |
| `nbList()`      | Hitung elemen       | Menghitung jumlah node dalam list |

---

## File ADT Lengkap (Header)

```cpp
#ifndef LIST_H
#define LIST_H
#include "boolean.h"
#define Nil NULL

typedef int infotype;
typedef struct elmlist *address;

struct elmlist {
    infotype info;
    address next;
};

struct list {
    address first;
};

// ADT Function Prototypes
bool ListEmpty(list L);
void CreateList(list &L);
void dealokasi(address P);
void insertFirst(list &L, address P);
void insertAfter(list &L, address P, address Prec);
void insertLast(list &L, address P);
void printInfo(list L);
int nbList(list L);

#endif
```

---

##  Ilustrasi Sederhana

```
First -> [10 | next] -> [20 | next] -> [30 | NULL]
```

Operasi:

1. `insertFirst(5)` → `[5 | next] -> [10 | next] -> [20 | next] -> [30 | NULL]`
2. `insertLast(40)` → `[10 | next] -> [20 | next] -> [30 | next] -> [40 | NULL]`
3. `insertAfter(20, 25)` → `[10 | next] -> [20 | next] -> [25 | next] -> [30 | NULL]`

---

##  Kesimpulan

Singly Linked List adalah struktur data dinamis berbasis pointer yang memungkinkan penyimpanan data secara fleksibel dan efisien. Dengan memahami operasi dasar seperti **create**, **insert**, **delete**, dan **traversal**, mahasiswa dapat membangun berbagai tipe struktur data kompleks seperti **stack**, **queue**, dan **tree**.


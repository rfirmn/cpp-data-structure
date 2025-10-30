# Modul 5: Double Linked List

## 🎯 Tujuan Praktikum

1. Memahami konsep **Doubly Linked List (DLL)** yang memiliki dua arah pointer.
2. Mempelajari operasi dasar dalam Doubly Linked List seperti **insert**, **delete**, **traversal**, dan **search**.
3. Membuat program **Doubly Linked List** menggunakan C++ baik dengan pendekatan **struct** maupun **OOP**.

---

![Doubly Linked List](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2014/03/DLL1.png)

## 5.1 Pengantar Double Linked List

Doubly Linked List (DLL) adalah versi lanjutan dari **Singly Linked List** yang memiliki **dua pointer** di setiap node:

* `prev`: menunjuk ke node sebelumnya.
* `next`: menunjuk ke node berikutnya.

Struktur ini memungkinkan traversal dilakukan **maju** dan **mundur**, membuat operasi seperti **insert** dan **delete** menjadi lebih fleksibel.

### Keunggulan Double Linked List

* Bisa menelusuri elemen dari depan maupun belakang.
* Proses **hapus node tengah** lebih efisien karena memiliki pointer ke node sebelumnya.
* Cocok untuk aplikasi seperti **browser history**, **undo/redo**, dan **navigasi playlist**.

### Kekurangan Double Linked List

* Membutuhkan memori lebih banyak (2 pointer per node).
* Implementasinya lebih kompleks dibanding singly linked list.

---

## 5.2 Struktur Data Double Linked List (C++)

### `list.h`

```cpp
#ifndef LIST_H_INCLUDED
#define LIST_H_INCLUDED
#include <iostream>
#include <cstring>
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
    address prev;
};

struct list {
    address first;
    address last;
};

#endif // LIST_H_INCLUDED
```

---

## 5.3 Operasi Dasar Double Linked List

### A. Create List

```cpp
void createList(list &L) {
    L.first = nullptr;
    L.last = nullptr;
}
```

### B. Alokasi dan Dealokasi

```cpp
address alokasi(mahasiswa M) {
    address P = new elmlist;
    P->info = M;
    P->next = nullptr;
    P->prev = nullptr;
    return P;
}

void dealokasi(address P) {
    delete P;
}
```

### C. Pengecekan List Kosong

```cpp
bool isEmpty(list L) {
    return (L.first == nullptr);
}
```

---

## 5.4 Operasi Insert

### A. Insert First

```cpp
void insertFirst(list &L, address P) {
    if (isEmpty(L)) {
        L.first = P;
        L.last = P;
    } else {
        P->next = L.first;
        L.first->prev = P;
        L.first = P;
    }
}
```

### B. Insert Last

```cpp
void insertLast(list &L, address P) {
    if (isEmpty(L)) {
        L.first = P;
        L.last = P;
    } else {
        L.last->next = P;
        P->prev = L.last;
        L.last = P;
    }
}
```

### C. Insert After

```cpp
void insertAfter(list &L, address Prec, address P) {
    if (Prec != nullptr && P != nullptr) {
        P->next = Prec->next;
        P->prev = Prec;
        if (Prec->next != nullptr) {
            Prec->next->prev = P;
        } else {
            L.last = P;
        }
        Prec->next = P;
    }
}
```

---

## 5.5 Operasi Delete

### A. Delete First

```cpp
void deleteFirst(list &L, address &P) {
    if (!isEmpty(L)) {
        P = L.first;
        if (L.first == L.last) {
            L.first = nullptr;
            L.last = nullptr;
        } else {
            L.first = P->next;
            L.first->prev = nullptr;
        }
        P->next = nullptr;
    }
}
```

### B. Delete Last

```cpp
void deleteLast(list &L, address &P) {
    if (!isEmpty(L)) {
        P = L.last;
        if (L.first == L.last) {
            L.first = nullptr;
            L.last = nullptr;
        } else {
            L.last = P->prev;
            L.last->next = nullptr;
        }
        P->prev = nullptr;
    }
}
```

### C. Delete After

```cpp
void deleteAfter(list &L, address Prec, address &P) {
    if (Prec != nullptr && Prec->next != nullptr) {
        P = Prec->next;
        Prec->next = P->next;
        if (P->next != nullptr) {
            P->next->prev = Prec;
        } else {
            L.last = Prec;
        }
        P->next = nullptr;
        P->prev = nullptr;
    }
}
```

---

## 5.6 Traversal dan View Data

### Cetak dari Depan

```cpp
void printForward(list L) {
    address P = L.first;
    cout << "Isi list dari depan:\n";
    while (P != nullptr) {
        cout << "NIM: " << P->info.nim << ", Nama: " << P->info.nama << endl;
        P = P->next;
    }
}
```

### Cetak dari Belakang

```cpp
void printBackward(list L) {
    address P = L.last;
    cout << "Isi list dari belakang:\n";
    while (P != nullptr) {
        cout << "NIM: " << P->info.nim << ", Nama: " << P->info.nama << endl;
        P = P->prev;
    }
}
```

---

## 5.7 Fungsi Utama

### `main.cpp`

```cpp
#include <iostream>
#include "list.h"
using namespace std;

int main() {
    list L;
    createList(L);

    mahasiswa mA = {"Budi", "101"};
    mahasiswa mB = {"Citra", "102"};
    mahasiswa mC = {"Doni", "103"};

    address pA = alokasi(mA);
    address pB = alokasi(mB);
    address pC = alokasi(mC);

    insertFirst(L, pA);
    insertLast(L, pB);
    insertLast(L, pC);

    cout << "Setelah insertFirst dan insertLast:\n";
    printForward(L);

    address hapus;
    deleteFirst(L, hapus);
    cout << "\nSetelah deleteFirst (hapus: " << hapus->info.nama << "):\n";
    printForward(L);

    deleteLast(L, hapus);
    cout << "\nSetelah deleteLast (hapus: " << hapus->info.nama << "):\n";
    printForward(L);

    cout << "\nCetak dari belakang:\n";
    printBackward(L);

    return 0;
}
```

---

## 5.8 Double Linked List menggunakan OOP

### `class_dll.cpp`

```cpp
#include <iostream>
using namespace std;

class Node {
public:
    int info;
    Node *next;
    Node *prev;

    Node(int data) {
        info = data;
        next = nullptr;
        prev = nullptr;
    }
};

class DoublyList {
public:
    Node *first;
    Node *last;

    DoublyList() {
        first = nullptr;
        last = nullptr;
    }

    void insertFirst(int data) {
        Node *P = new Node(data);
        if (first == nullptr) {
            first = last = P;
        } else {
            P->next = first;
            first->prev = P;
            first = P;
        }
    }

    void insertLast(int data) {
        Node *P = new Node(data);
        if (last == nullptr) {
            first = last = P;
        } else {
            last->next = P;
            P->prev = last;
            last = P;
        }
    }

    void deleteFirst() {
        if (first != nullptr) {
            Node *P = first;
            if (first == last) {
                first = last = nullptr;
            } else {
                first = first->next;
                first->prev = nullptr;
            }
            delete P;
        }
    }

    void deleteLast() {
        if (last != nullptr) {
            Node *P = last;
            if (first == last) {
                first = last = nullptr;
            } else {
                last = last->prev;
                last->next = nullptr;
            }
            delete P;
        }
    }

    void printForward() {
        Node *P = first;
        cout << "List (forward): ";
        while (P != nullptr) {
            cout << P->info << " ";
            P = P->next;
        }
        cout << endl;
    }

    void printBackward() {
        Node *P = last;
        cout << "List (backward): ";
        while (P != nullptr) {
            cout << P->info << " ";
            P = P->prev;
        }
        cout << endl;
    }
};

int main() {
    DoublyList L;
    L.insertFirst(10);
    L.insertLast(20);
    L.insertLast(30);

    L.printForward();
    L.printBackward();

    L.deleteFirst();
    L.printForward();

    L.deleteLast();
    L.printForward();

    return 0;
}
```

---

## Ringkasan Operasi Double Linked List

| Operasi           | Fungsi            | Deskripsi                                     |
| ----------------- | ----------------- | --------------------------------------------- |
| `createList()`    | Inisialisasi list | Membuat list kosong dengan `first` dan `last` |
| `insertFirst()`   | Tambah di awal    | Menambah node di depan                        |
| `insertLast()`    | Tambah di akhir   | Menambah node di belakang                     |
| `insertAfter()`   | Tambah di tengah  | Menambah node setelah node tertentu           |
| `deleteFirst()`   | Hapus di awal     | Menghapus node pertama                        |
| `deleteLast()`    | Hapus di akhir    | Menghapus node terakhir                       |
| `deleteAfter()`   | Hapus di tengah   | Menghapus node setelah node tertentu          |
| `printForward()`  | Cetak maju        | Traversal dari depan ke belakang              |
| `printBackward()` | Cetak mundur      | Traversal dari belakang ke depan              |

---

## Kesimpulan

Doubly Linked List merupakan pengembangan dari singly linked list yang memungkinkan navigasi dua arah dengan menambahkan pointer `prev`. Struktur ini memudahkan operasi **insert** dan **delete** di posisi mana pun, meskipun membutuhkan memori lebih besar. Pemahaman konsep DLL menjadi dasar untuk membangun struktur data lanjutan seperti **Deque**, **Queue Ganda**, dan **Editor Undo/Redo**.

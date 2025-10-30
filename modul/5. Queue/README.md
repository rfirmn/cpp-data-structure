# Belajar Queue di C++

## 🧠 Deskripsi Singkat

Modul **“Belajar Queue di C++”** ini dirancang untuk membantu mahasiswa memahami konsep antrian (*queue*) dan implementasinya dalam bahasa C++. Peserta akan belajar bagaimana *queue* bekerja berdasarkan prinsip FIFO (*First In First Out*), serta bagaimana menerapkannya dalam berbagai konteks pemrograman.

---

## 📘 Pendahuluan

### Apa itu Queue?

Queue (Antrian) adalah struktur data linear di mana elemen dimasukkan dari satu ujung (belakang) dan dihapus dari ujung lainnya (depan). Prinsip dasarnya adalah **First In First Out (FIFO)**.

### Contoh Dunia Nyata

* Antrian pembeli di kasir supermarket.
* Sistem antrean cetak dokumen di printer.
* Buffer jaringan untuk paket data.

---

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan modul ini, peserta diharapkan mampu:

1. Memahami konsep dasar FIFO.
2. Menjelaskan operasi dasar queue.
3. Mengimplementasikan queue dengan berbagai metode di C++.
4. Menerapkan konsep queue dalam pemecahan masalah nyata.

---

## ⚙️ Operasi Dasar Queue

### 1. Enqueue (Menambahkan Elemen)

Menambahkan elemen ke belakang antrian.

```cpp
queue<int> q;
q.push(10); // enqueue
```

### 2. Dequeue (Menghapus Elemen)

Menghapus elemen dari depan antrian.

```cpp
q.pop(); // dequeue elemen terdepan
```

### 3. Front / Peek

Melihat elemen terdepan tanpa menghapusnya.

```cpp
cout << q.front(); // melihat elemen pertama
```

### 4. isEmpty

Memeriksa apakah queue kosong.

```cpp
if (q.empty()) cout << "Queue kosong";
```

### 5. Size / Length

Menghitung jumlah elemen dalam queue.

```cpp
cout << q.size();
```

---

## 🧩 Implementasi Queue di C++

### 1. Menggunakan `std::queue`

Implementasi bawaan dari C++ STL.

```cpp
#include <queue>
using namespace std;

queue<int> q;
q.push(1);
q.push(2);
cout << q.front();
q.pop();
```

✅ **Kelebihan:** Mudah digunakan, aman, dan efisien.
❌ **Kekurangan:** Tidak fleksibel untuk manipulasi langsung elemen.

### 2. Menggunakan `std::deque`

Deque (*Double Ended Queue*) dapat digunakan sebagai dasar queue.

```cpp
#include <deque>
using namespace std;

deque<int> dq;
dq.push_back(1);
dq.push_back(2);
dq.pop_front();
```

✅ Lebih fleksibel karena dapat menambahkan dari dua sisi.
❌ Sedikit lebih kompleks dibanding `queue` biasa.

### 3. Implementasi Manual (Array)

```cpp
#define SIZE 5
int queue[SIZE];
int front = -1, rear = -1;

void enqueue(int val) {
    if (rear == SIZE - 1) cout << "Queue penuh\n";
    else {
        if (front == -1) front = 0;
        queue[++rear] = val;
    }
}

void dequeue() {
    if (front == -1 || front > rear) cout << "Queue kosong\n";
    else front++;
}
```

✅ Dapat dikontrol penuh.
❌ Tidak dinamis, kapasitas terbatas.

### 4. Menggunakan Linked List

```cpp
struct Node {
    int data;
    Node* next;
};

Node *front = nullptr, *rear = nullptr;

void enqueue(int val) {
    Node* temp = new Node{val, nullptr};
    if (!rear) front = rear = temp;
    else {
        rear->next = temp;
        rear = temp;
    }
}

void dequeue() {
    if (!front) return;
    Node* temp = front;
    front = front->next;
    if (!front) rear = nullptr;
    delete temp;
}
```

✅ Dinamis dan efisien.
❌ Implementasi lebih panjang.

---

## 🌀 Varian Queue / Queue Khusus

### 1. Circular Queue

Queue yang memutar indeks ketika penuh agar ruang dapat digunakan kembali.

### 2. Priority Queue

Elemen dengan prioritas lebih tinggi dikeluarkan lebih dulu.

```cpp
#include <queue>
#include <vector>
using namespace std;

priority_queue<int> pq;
pq.push(10);
pq.push(5);
cout << pq.top(); // 10
```

### 3. Double-Ended Queue (Deque)

Memungkinkan operasi di kedua ujung antrian.

```cpp
#include <deque>

deque<int> dq;
dq.push_front(1);
dq.push_back(2);
```

---

## ⏱️ Kompleksitas Operasi

| Operasi        | Array | Linked List | STL Queue |
| -------------- | ----- | ----------- | --------- |
| Enqueue        | O(1)  | O(1)        | O(1)      |
| Dequeue        | O(1)  | O(1)        | O(1)      |
| Peek / Front   | O(1)  | O(1)        | O(1)      |
| isEmpty / Size | O(1)  | O(1)        | O(1)      |

---

## 💡 Contoh Aplikasi Queue

### 1. Simulasi Antrian Kasir

### 2. Sistem Cetak Dokumen

### 3. Breadth-First Search (BFS)

### 4. Task Scheduling

```cpp
#include <queue>
#include <iostream>
using namespace std;

void bfs(int start, vector<vector<int>>& graph) {
    queue<int> q;
    vector<bool> visited(graph.size(), false);
    q.push(start);
    visited[start] = true;

    while(!q.empty()) {
        int node = q.front(); q.pop();
        cout << node << " ";
        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                q.push(neighbor);
                visited[neighbor] = true;
            }
        }
    }
}
```

---

## 📚 Referensi / Bacaan Tambahan

* C++ STL Documentation: [https://cplusplus.com/reference/queue/queue/](https://cplusplus.com/reference/queue/queue/)
* GeeksforGeeks Queue Tutorial: [https://www.geeksforgeeks.org/queue-data-structure/](https://www.geeksforgeeks.org/queue-data-structure/)
* Book: *Data Structures and Algorithm Analysis in C++* by Mark Allen Weiss

# Belajar Stack di C++

## Deskripsi Singkat

Modul **“Belajar Stack di C++”** ini dirancang untuk membantu mahasiswa memahami konsep tumpukan (*stack*) dan implementasinya dalam bahasa C++. Peserta akan belajar bagaimana *stack* bekerja berdasarkan prinsip **LIFO (Last In First Out)**, serta bagaimana menerapkannya dalam berbagai konteks pemrograman.

---

## Pendahuluan

### Apa itu Stack?

Stack (tumpukan) adalah struktur data linear di mana elemen terakhir yang dimasukkan akan menjadi yang pertama dikeluarkan (prinsip **LIFO**).

### Contoh

* Tumpukan piring di dapur: piring terakhir yang diletakkan akan diambil terlebih dahulu.
* Fitur *Undo/Redo* di aplikasi teks.
* *Call stack* dalam pemanggilan fungsi.

---
## Operasi Dasar Stack

### 1. Push

Menambahkan elemen ke atas tumpukan.

```cpp
stack<int> s;
s.push(10);
```

### 2. Pop

Menghapus elemen dari atas tumpukan.

```cpp
s.pop();
```

### 3. Top / Peek

Melihat elemen paling atas tanpa menghapusnya.

```cpp
cout << s.top();
```

### 4. isEmpty

Memeriksa apakah stack kosong.

```cpp
if (s.empty()) cout << "Stack kosong";
```

### 5. Size / Length

Menghitung jumlah elemen dalam stack.

```cpp
cout << s.size();
```

---

## Implementasi Stack di C++

### 1. Menggunakan `std::stack`

```cpp
#include <stack>
#include <iostream>
using namespace std;

int main() {
    stack<int> s;
    s.push(10);
    s.push(20);
    cout << s.top() << endl; // 20
    s.pop();
    cout << s.top() << endl; // 10
}
```

✅ **Kelebihan:** Mudah digunakan dan efisien.
❌ **Kekurangan:** Tidak dapat mengakses elemen selain yang teratas.

---

### 2. Implementasi dengan Array

```cpp
#define MAX 5
int stack[MAX];
int top = -1;

void push(int val) {
    if (top == MAX - 1) cout << "Stack penuh\n";
    else stack[++top] = val;
}

void pop() {
    if (top == -1) cout << "Stack kosong\n";
    else top--;
}

int peek() {
    if (top == -1) return -1;
    return stack[top];
}
```

✅ Kontrol penuh terhadap memori.
❌ Kapasitas terbatas (tidak dinamis).

---

### 3. Implementasi dengan Linked List

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

Node* topNode = nullptr;

void push(int val) {
    Node* temp = new Node{val, topNode};
    topNode = temp;
}

void pop() {
    if (!topNode) return;
    Node* temp = topNode;
    topNode = topNode->next;
    delete temp;
}

int peek() {
    if (!topNode) return -1;
    return topNode->data;
}
```

✅ Dinamis, tidak terbatas kapasitas.
❌ Implementasi lebih panjang dan butuh manajemen memori.

---

## Aplikasi Stack

### 1. Membalik String

```cpp
#include <stack>
#include <iostream>
using namespace std;

int main() {
    string str = "RIO";
    stack<char> s;
    for (char c : str) s.push(c);

    while (!s.empty()) {
        cout << s.top();
        s.pop();
    }
}
```

### 2. Mengecek Keseimbangan Kurung

```cpp
#include <stack>
#include <iostream>
using namespace std;

bool isBalanced(string expr) {
    stack<char> s;
    for (char c : expr) {
        if (c == '(') s.push(c);
        else if (c == ')') {
            if (s.empty()) return false;
            s.pop();
        }
    }
    return s.empty();
}

int main() {
    string expr = "(())()";
    cout << (isBalanced(expr) ? "Balanced" : "Not Balanced");
}
```

### 3. Reverse Integer atau Array

### 4. Evaluasi Ekspresi Postfix

### 5. DFS (menggunakan stack alih-alih rekursi)

---

## ⏱️ Kompleksitas Operasi

| Operasi | Array | Linked List | STL Stack |
| ------- | ----- | ----------- | --------- |
| Push    | O(1)  | O(1)        | O(1)      |
| Pop     | O(1)  | O(1)        | O(1)      |
| Peek    | O(1)  | O(1)        | O(1)      |
| isEmpty | O(1)  | O(1)        | O(1)      |

---

## 📚 Referensi / Bacaan Tambahan

* C++ STL Stack: [https://cplusplus.com/reference/stack/stack/](https://cplusplus.com/reference/stack/stack/)
* GeeksforGeeks Stack Tutorial: [https://www.geeksforgeeks.org/stack-data-structure/](https://www.geeksforgeeks.org/stack-data-structure/)
* Book: *Data Structures and Algorithm Analysis in C++* – Mark Allen Weiss

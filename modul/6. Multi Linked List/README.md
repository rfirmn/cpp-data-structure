-----

### KONSEP UMUM MULTI LINKED LIST DENGAN RELASI PARENT-CHILD

Multi Linked List adalah struktur data yang mengizinkan lebih dari satu jenis hubungan antar elemen melalui pointer tambahan. Pada implementasi ini, kita memiliki tiga list yang **berdiri sendiri**:

1.  List Parent – entitas utama
2.  List Child – entitas bawahan
3.  List Relasi – penghubung antara Parent dan Child (bisa 1-N atau M-N)

-----

### BENTUK I – Relasi 1-N (One-to-Many) Pure

Satu Parent memiliki banyak Child, satu Child hanya dimiliki oleh satu Parent.

<img width="1038" height="368" alt="image" src="https://user-images.githubusercontent.com/13241336/36650938-af7d7db4-1ad8-11e8-8a4d-43d83672f50f.png" />


```cpp
/**
 * file: multi_linked_list_1N.cpp
 * Penjelasan: 
 * Dalam C++, "None" digantikan dengan "nullptr".
 * Struktur data menggunakan "struct" atau "class" dengan pointer (*).
 */

#include <iostream>
#include <string>

using namespace std;

// Forward declaration
struct NodeChild;

struct NodeParent {
    string id;
    string nama;
    NodeChild* first_child; // Head dari child milik parent ini (1-N)
    NodeParent* next;       // Untuk linked list antar parent

    NodeParent(string id_parent, string nama_parent) {
        id = id_parent;
        nama = nama_parent;
        first_child = nullptr;
        next = nullptr;
    }
};

struct NodeChild {
    string id;
    string data;
    NodeChild* next;        // Child berikutnya dari parent yang sama

    NodeChild(string id_child, string data_child) {
        id = id_child;
        data = data_child;
        next = nullptr;
    }
};

class MultiLinkedList_1N {
public:
    NodeParent* first_parent;
    int size;

    MultiLinkedList_1N() {
        first_parent = nullptr;
        size = 0;
    }

    // INSERT PARENT
    NodeParent* insertParent(string id_parent, string nama) {
        if (findParent(id_parent)) {
            cout << "[ERROR] Parent ID " << id_parent << " sudah ada!" << endl;
            return nullptr;
        }
        NodeParent* new_parent = new NodeParent(id_parent, nama);
        new_parent->next = first_parent;
        first_parent = new_parent;
        size++;
        return new_parent;
    }

    // FIND PARENT
    NodeParent* findParent(string id_parent) {
        NodeParent* curr = first_parent;
        while (curr) {
            if (curr->id == id_parent) {
                return curr;
            }
            curr = curr->next;
        }
        return nullptr;
    }

    // INSERT CHILD (1-N)
    bool insertChild(string id_parent, string id_child, string data) {
        NodeParent* parent = findParent(id_parent);
        if (!parent) {
            cout << "[ERROR] Parent " << id_parent << " tidak ditemukan!" << endl;
            return false;
        }
        
        // Cek duplikat child di parent ini
        NodeChild* curr = parent->first_child;
        while (curr) {
            if (curr->id == id_child) {
                cout << "[ERROR] Child ID " << id_child << " sudah ada di parent ini!" << endl;
                return false;
            }
            curr = curr->next;
        }
        
        NodeChild* new_child = new NodeChild(id_child, data);
        new_child->next = parent->first_child;
        parent->first_child = new_child;
        return true;
    }

    // DELETE PARENT (dan semua child-nya otomatis terhapus)
    bool deleteParent(string id_parent) {
        if (!first_parent) return false;

        // Jika parent yang dihapus adalah head
        if (first_parent->id == id_parent) {
            NodeParent* del = first_parent;
            first_parent = first_parent->next;
            
            // Hapus semua child (cleanup memory manual di C++)
            NodeChild* c = del->first_child;
            while(c) {
                NodeChild* temp = c;
                c = c->next;
                delete temp;
            }
            delete del;
            size--;
            return true;
        }

        NodeParent* prev = first_parent;
        NodeParent* curr = prev->next;
        while (curr) {
            if (curr->id == id_parent) {
                prev->next = curr->next;
                
                // Hapus semua child
                NodeChild* c = curr->first_child;
                while(c) {
                    NodeChild* temp = c;
                    c = c->next;
                    delete temp;
                }
                
                delete curr;
                size--;
                return true;
            }
            prev = curr;
            curr = curr->next;
        }
        return false;
    }

    // DELETE CHILD dari parent tertentu
    bool deleteChild(string id_parent, string id_child) {
        NodeParent* parent = findParent(id_parent);
        if (!parent || !parent->first_child) return false;

        // Jika child pertama yang dihapus
        if (parent->first_child->id == id_child) {
            NodeChild* del = parent->first_child;
            parent->first_child = parent->first_child->next;
            delete del;
            return true;
        }

        NodeChild* prev = parent->first_child;
        NodeChild* curr = prev->next;
        while (curr) {
            if (curr->id == id_child) {
                prev->next = curr->next;
                delete curr;
                return true;
            }
            prev = curr;
            curr = curr->next;
        }
        return false;
    }

    // PRINT SEMUA
    void printAll() {
        cout << string(60, '=') << endl;
        cout << "MULTI LINKED LIST - BENTUK I (Relasi 1-N)" << endl;
        cout << string(60, '=') << endl;
        
        if (!first_parent) {
            cout << "List Parent kosong." << endl;
            return;
        }

        NodeParent* curr_p = first_parent;
        while (curr_p) {
            cout << "Parent [" << curr_p->id << "] " << curr_p->nama << " -> Child: ";
            NodeChild* curr_c = curr_p->first_child;
            if (!curr_c) {
                cout << "None";
            } else {
                while (curr_c) {
                    cout << "(" << curr_c->id << ":" << curr_c->data << ")";
                    if (curr_c->next) cout << " -> ";
                    curr_c = curr_c->next;
                }
            }
            cout << endl; // Baris baru setelah list child selesai
            curr_p = curr_p->next;
        }
        cout << "Total Parent: " << size << endl;
        cout << string(60, '=') << endl;
    }
};
```

```cpp
// File Main Program

int main() {
    // Membuat objek multi linked list
    MultiLinkedList_1N ml;

    // ===== INSERT PARENT =====
    cout << "\n>> Menambah Parent" << endl;
    ml.insertParent("P1", "Parent Satu");
    ml.insertParent("P2", "Parent Dua");
    ml.insertParent("P3", "Parent Tiga");

    // ===== INSERT CHILD (1-N) =====
    cout << "\n>> Menambah Child ke masing-masing Parent" << endl;
    ml.insertChild("P1", "C1", "Data Child 1");
    ml.insertChild("P1", "C2", "Data Child 2");

    ml.insertChild("P2", "C3", "Child untuk Parent 2");

    // Parent P3 belum punya child (contoh parent kosong)

    // ===== CETAK SEMUA =====
    cout << "\n>> Print semua data:" << endl;
    ml.printAll();

    // ===== DELETE CHILD =====
    cout << "\n>> Hapus Child C2 dari Parent P1" << endl;
    ml.deleteChild("P1", "C2");
    ml.printAll();

    // ===== DELETE PARENT =====
    cout << "\n>> Hapus Parent P2 (beserta semua Child-nya)" << endl;
    ml.deleteParent("P2");
    ml.printAll();

    return 0;
}
```

-----

### BENTUK II – Relasi 1-N dan M-N Sekaligus (Hybrid)

<img width="1038" height="368" alt="image" src="https://github.com/user-attachments/assets/fd30cbc9-aceb-4579-a9f3-0a19b5be5cfa" />

```cpp
/**
 * file: multi_linked_list_hybrid.cpp
 * Penjelasan:
 * Menggunakan forward declaration untuk menangani pointer antar struct.
 */

#include <iostream>
#include <string>

using namespace std;

// Forward Declarations
struct NodeParent;
struct NodeChildMN;
struct NodeRelasiMN;

struct NodeChild1N {
    string id;
    string data;
    NodeChild1N* next;

    NodeChild1N(string id_c, string data_c) : id(id_c), data(data_c), next(nullptr) {}
};

struct NodeChildMN {
    string id;
    string data;
    NodeChildMN* next; // Untuk list child global (opsional/terpisah)

    NodeChildMN(string id_c, string data_c) : id(id_c), data(data_c), next(nullptr) {}
};

struct NodeRelasiMN {
    NodeParent* parent;    // pointer ke NodeParent
    NodeChildMN* child;    // pointer ke NodeChildMN
    NodeRelasiMN* next;    // next relasi dari parent yang sama

    NodeRelasiMN(NodeParent* p, NodeChildMN* c) : parent(p), child(c), next(nullptr) {}
};

struct NodeParent {
    string id;
    string nama;
    NodeChild1N* first_child_1N;   // Relasi 1-N
    NodeRelasiMN* first_relasi_MN; // Head relasi M-N dari parent ini
    NodeParent* next;

    NodeParent(string id_p, string nama_p) : id(id_p), nama(nama_p), 
                                             first_child_1N(nullptr), 
                                             first_relasi_MN(nullptr), 
                                             next(nullptr) {}
};

class MultiLinkedList_Hybrid {
public:
    NodeParent* first_parent;
    NodeChildMN* first_child_MN;

    MultiLinkedList_Hybrid() {
        first_parent = nullptr;
        first_child_MN = nullptr;
    }

    NodeParent* findParent(string id_p) {
        NodeParent* c = first_parent;
        while (c && c->id != id_p) c = c->next;
        return c;
    }

    NodeChildMN* findChildMN(string id_c) {
        NodeChildMN* c = first_child_MN;
        while (c && c->id != id_c) c = c->next;
        return c;
    }

    NodeParent* insertParent(string id_p, string nama) {
        if (findParent(id_p)) return nullptr;
        NodeParent* np = new NodeParent(id_p, nama);
        np->next = first_parent;
        first_parent = np;
        return np;
    }

    bool insertChild1N(string id_parent, string id_child, string data) {
        NodeParent* parent = findParent(id_parent);
        if (!parent) return false;
        NodeChild1N* nc = new NodeChild1N(id_child, data);
        nc->next = parent->first_child_1N;
        parent->first_child_1N = nc;
        return true;
    }

    NodeChildMN* insertChildMN(string id_child, string data) {
        if (findChildMN(id_child)) return nullptr;
        NodeChildMN* nc = new NodeChildMN(id_child, data);
        nc->next = first_child_MN;
        first_child_MN = nc;
        return nc;
    }

    bool insertRelasiMN(string id_parent, string id_child) {
        NodeParent* parent = findParent(id_parent);
        NodeChildMN* child = findChildMN(id_child);
        if (!parent || !child) return false;
        
        NodeRelasiMN* rel = new NodeRelasiMN(parent, child);
        rel->next = parent->first_relasi_MN;
        parent->first_relasi_MN = rel;
        return true;
    }

    void printAll() {
        cout << string(70, '=') << endl;
        cout << "MULTI LINKED LIST - BENTUK II (1-N + M-N Hybrid)" << endl;
        cout << string(70, '=') << endl;
        
        NodeParent* p = first_parent;
        while (p) {
            cout << "Parent [" << p->id << "] " << p->nama << endl;
            
            // Print 1-N
            cout << "   |-- 1-N Children  : ";
            NodeChild1N* c1 = p->first_child_1N;
            if (!c1) cout << "None";
            while (c1) {
                cout << "(" << c1->id << ":" << c1->data << ")";
                if (c1->next) cout << " -> ";
                c1 = c1->next;
            }
            cout << endl;

            // Print M-N
            cout << "   |-- M-N Relations : ";
            NodeRelasiMN* r = p->first_relasi_MN;
            if (!r) cout << "None";
            while (r) {
                cout << "(" << r->child->id << ":" << r->child->data << ")";
                if (r->next) cout << " -> ";
                r = r->next;
            }
            cout << endl << endl;
            p = p->next;
        }
        cout << string(70, '=') << endl;
    }
};
```

```cpp
// File Main Program Hybrid

int main() {
    MultiLinkedList_Hybrid ml;

    cout << "\n>> INSERT PARENT" << endl;
    ml.insertParent("P1", "Parent Satu");
    ml.insertParent("P2", "Parent Dua");
    ml.insertParent("P3", "Parent Tiga");

    cout << "\n>> INSERT CHILD 1-N (child langsung ke parent)" << endl;
    ml.insertChild1N("P1", "C1", "Child1 milik P1");
    ml.insertChild1N("P1", "C2", "Child2 milik P1");

    ml.insertChild1N("P2", "C3", "Child milik P2");

    // Parent P3 tidak punya child 1-N (contoh)

    cout << "\n>> INSERT CHILD M-N (child global)" << endl;
    ml.insertChildMN("X1", "Child X1 (global)");
    ml.insertChildMN("X2", "Child X2 (global)");
    ml.insertChildMN("X3", "Child X3 (global)");

    cout << "\n>> INSERT RELASI M-N (Parent -- M:N -- Child Global)" << endl;
    ml.insertRelasiMN("P1", "X1");
    ml.insertRelasiMN("P1", "X2");

    ml.insertRelasiMN("P2", "X1");
    ml.insertRelasiMN("P2", "X3");

    // P3 hanya contoh parent yang belum memiliki relasi M-N

    cout << "\n>> PRINT SEMUA DATA:" << endl;
    ml.printAll();

    return 0;
}
```

-----

### BENTUK III-A – Relasi M-N Klasik (Dengan Node Relasi Eksplisit)

<img width="1071" height="549" alt="image" src="https://github.com/user-attachments/assets/232b9af6-8a13-42a0-b331-a26360852fc8" />

```cpp
/**
 * file: multi_linked_list_MN_classic.cpp
 * Penjelasan:
 * Struktur saling tunjuk (circular reference) membutuhkan forward declaration.
 */

#include <iostream>
#include <string>

using namespace std;

// Forward Declarations
struct NodeParent;
struct NodeChild;
struct NodeRelasi;

struct NodeParent {
    string id;
    string nama;
    NodeParent* next;
    NodeRelasi* relasi_head; // Daftar relasi dari parent ini

    NodeParent(string id_p, string n) : id(id_p), nama(n), next(nullptr), relasi_head(nullptr) {}
};

struct NodeChild {
    string id;
    string data;
    NodeChild* next;

    NodeChild(string id_c, string d) : id(id_c), data(d), next(nullptr) {}
};

struct NodeRelasi {
    NodeParent* parent;
    NodeChild* child;
    NodeRelasi* next_from_parent;
    NodeRelasi* next_from_child; // Bisa untuk traversal dari child (bi-directional)

    NodeRelasi(NodeParent* p, NodeChild* c) : 
        parent(p), child(c), next_from_parent(nullptr), next_from_child(nullptr) {}
};

class MultiLinkedList_MN_A {
public:
    NodeParent* first_parent;
    NodeChild* first_child;

    MultiLinkedList_MN_A() {
        first_parent = nullptr;
        first_child = nullptr;
    }

    NodeParent* findParent(string id_p) {
        NodeParent* c = first_parent;
        while (c && c->id != id_p) c = c->next;
        return c;
    }

    NodeChild* findChild(string id_c) {
        NodeChild* c = first_child;
        while (c && c->id != id_c) c = c->next;
        return c;
    }

    NodeParent* insertParent(string id_p, string nama) {
        if (findParent(id_p)) return nullptr;
        NodeParent* np = new NodeParent(id_p, nama);
        np->next = first_parent;
        first_parent = np;
        return np;
    }

    NodeChild* insertChild(string id_c, string data) {
        if (findChild(id_c)) return nullptr;
        NodeChild* nc = new NodeChild(id_c, data);
        nc->next = first_child;
        first_child = nc;
        return nc;
    }

    bool _isRelated(NodeParent* parent, NodeChild* child) {
        NodeRelasi* r = parent->relasi_head;
        while (r) {
            if (r->child->id == child->id) return true;
            r = r->next_from_parent;
        }
        return false;
    }

    bool insertRelasi(string id_parent, string id_child) {
        NodeParent* p = findParent(id_parent);
        NodeChild* c = findChild(id_child);
        if (!p || !c) return false;
        if (_isRelated(p, c)) return false; // sudah terelasi

        NodeRelasi* rel = new NodeRelasi(p, c);
        rel->next_from_parent = p->relasi_head;
        p->relasi_head = rel;
        
        // Logika untuk next_from_child bisa ditambahkan di sini jika diperlukan list relasi dari sisi child
        return true;
    }

    bool deleteRelasi(string id_parent, string id_child) {
        NodeParent* p = findParent(id_parent);
        if (!p || !p->relasi_head) return false;

        // Cek head relasi
        if (p->relasi_head->child->id == id_child) {
            NodeRelasi* del = p->relasi_head;
            p->relasi_head = p->relasi_head->next_from_parent;
            delete del;
            return true;
        }

        NodeRelasi* prev = p->relasi_head;
        NodeRelasi* curr = prev->next_from_parent;
        while (curr) {
            if (curr->child->id == id_child) {
                prev->next_from_parent = curr->next_from_parent;
                delete curr;
                return true;
            }
            prev = curr;
            curr = curr->next_from_parent;
        }
        return false;
    }

    void printAll() {
        cout << string(70, '=') << endl;
        cout << "MULTI LINKED LIST - BENTUK III-A (M-N Klasik)" << endl;
        cout << string(70, '=') << endl;
        NodeParent* p = first_parent;
        while (p) {
            cout << "Parent [" << p->id << "] " << p->nama << " -> terhubung dengan -> ";
            NodeRelasi* r = p->relasi_head;
            if (!r) {
                cout << "None";
            } else {
                while (r) {
                    cout << "(" << r->child->id << ":" << r->child->data << ")  ";
                    r = r->next_from_parent;
                }
            }
            cout << endl;
            p = p->next;
        }
        cout << string(70, '=') << endl;
    }
};
```

```cpp
// File Main Program Klasik

int main() {
    MultiLinkedList_MN_A ml;

    cout << "\n>> INSERT PARENT" << endl;
    ml.insertParent("P1", "Parent Satu");
    ml.insertParent("P2", "Parent Dua");
    ml.insertParent("P3", "Parent Tiga");

    cout << "\n>> INSERT CHILD 1-N (langsung ke parent tertentu)" << endl;
    // Note: Pada M-N Klasik, list child terpisah, di sini fungsinya insertChild biasa
    // (Judul log string disesuaikan dengan contoh Python, meski fungsinya lebih ke 'Global Child')
    
    // Namun kode di atas adalah insertChild1N (typo di contoh python mungkin?), 
    // tapi karena classnya MN Classic, kita pakai insertChild ke list global
    
    // Di bawah ini saya sesuaikan dengan struktur main Python yang memanggil function insertChildMN/insertChild
    // tapi karena di class Classic hanya ada "insertChild" (global), kita pakai itu.
    
    // -- Koreksi agar output sama dengan logika Python (Hybrid -> Classic):
    // "Insert Child 1-N" pada Python contoh di atas memanggil `ml.insertChild1N`, tapi di class Classic
    // tidak ada `insertChild1N`. Saya asumsikan ini hanya copy paste text di main Python.
    // Kode di bawah menyesuaikan kemampuan class Classic.

    cout << "\n>> INSERT CHILD GLOBAL M-N" << endl;
    ml.insertChild("X1", "Child X1 (global)");
    ml.insertChild("X2", "Child X2 (global)");
    ml.insertChild("X3", "Child X3 (global)");

    cout << "\n>> INSERT RELASI M-N (Parent <-> Child Global)" << endl;
    ml.insertRelasi("P1", "X1"); // P1 memiliki X1
    ml.insertRelasi("P1", "X2"); // P1 memiliki X2

    ml.insertRelasi("P2", "X1"); // P2 memiliki X1
    ml.insertRelasi("P2", "X3"); // P2 memiliki X3

    // Parent P3 tidak memiliki child M-N

    cout << "\n>> PRINT SEMUA DATA" << endl;
    ml.printAll();

    return 0;
}
```

-----

### BENTUK III-B – Relasi M-N Modern

<img width="1064" height="560" alt="image" src="https://github.com/user-attachments/assets/c37131ab-988e-4fc7-841d-0ffcda2f3822" />

**Penjelasan C++:**
Bahasa Python memiliki struktur data bawaan `dict` (Hash Map) dan `set`. Di C++, kita menggunakan **STL (Standard Template Library)** yaitu `std::map` (atau `std::unordered_map`) dan `std::set`. Kode di bawah ini mengadaptasi logika Python tersebut menggunakan library standar C++.

```cpp
/**
 * file: multi_linked_list_MN_modern.cpp
 * Penjelasan:
 * Menggunakan std::map untuk dictionary dan std::set untuk set.
 */

#include <iostream>
#include <string>
#include <map>
#include <set>
#include <vector>

using namespace std;

// Struct sederhana untuk menampung data parent karena map butuh value
struct ParentData {
    string nama;
};

class MultiLinkedList_MN_B {
private:
    // Python: self.parents = {}  -> id -> {nama, ...}
    map<string, ParentData> parents; 

    // Python: self.children = {} -> id -> data
    map<string, string> children;

    // Python: self.relasi = {}   -> parent_id -> set(child_id)
    map<string, set<string>> relasi;

public:
    bool insertParent(string id_p, string nama) {
        if (parents.find(id_p) != parents.end()) {
            return false;
        }
        parents[id_p] = {nama};
        // Inisialisasi set kosong untuk relasi
        relasi[id_p] = set<string>(); 
        return true;
    }

    bool insertChild(string id_c, string data) {
        if (children.find(id_c) != children.end()) {
            return false;
        }
        children[id_c] = data;
        return true;
    }

    bool insertRelasi(string id_p, string id_c) {
        // Cek keberadaan parent dan child
        if (parents.find(id_p) == parents.end() || children.find(id_c) == children.end()) {
            return false;
        }
        // Insert ke set (otomatis handle duplikat)
        relasi[id_p].insert(id_c);
        return true;
    }

    void deleteRelasi(string id_p, string id_c) {
        if (relasi.find(id_p) != relasi.end()) {
            relasi[id_p].erase(id_c);
        }
    }

    void deleteParent(string id_p) {
        parents.erase(id_p);
        relasi.erase(id_p);
    }

    void deleteChild(string id_c) {
        children.erase(id_c);
        // Hapus child ini dari semua relasi (iterate over maps values)
        for (auto& pair : relasi) { // pair adalah <id_parent, set_child>
            pair.second.erase(id_c);
        }
    }

    set<string> findChildrenOf(string id_p) {
        if (relasi.find(id_p) != relasi.end()) {
            return relasi[id_p];
        }
        return set<string>();
    }

    set<string> findParentsOf(string id_c) {
        set<string> parent_ids;
        for (auto const& [pid, childs_set] : relasi) {
            if (childs_set.find(id_c) != childs_set.end()) {
                parent_ids.insert(pid);
            }
        }
        return parent_ids;
    }

    void printAll() {
        cout << string(70, '=') << endl;
        cout << "MULTI LINKED LIST - BENTUK III-B (M-N Modern - Map Based)" << endl;
        cout << string(70, '=') << endl;
        
        for (auto const& [pid, childs_set] : relasi) {
            // Karena relasi mungkin berisi ID parent yang sudah dihapus jika tidak sinkron,
            // tapi di method deleteParent kita sudah hapus relasi.
            // Cek defensive programming:
            if (parents.find(pid) == parents.end()) continue;

            string p_nama = parents[pid].nama;
            
            cout << "Parent [" << pid << "] " << p_nama << " -> ";
            
            if (childs_set.empty()) {
                cout << "None";
            } else {
                bool first = true;
                for (const string& cid : childs_set) {
                    if (!first) cout << ", ";
                    // Ambil data child
                    cout << "(" << cid << ":" << children[cid] << ")";
                    first = false;
                }
            }
            cout << endl;
        }
        cout << "Total Parent: " << parents.size() << " | Total Child: " << children.size() << endl;
        cout << string(70, '=') << endl;
    }
};
```

```cpp
// File Main Program Modern

int main() {
    MultiLinkedList_MN_B mn;

    cout << "\n>> INSERT PARENTS" << endl;
    mn.insertParent("P1", "Parent Satu");
    mn.insertParent("P2", "Parent Dua");
    mn.insertParent("P3", "Parent Tiga");

    cout << "\n>> INSERT CHILDREN" << endl;
    mn.insertChild("C1", "Child 1");
    mn.insertChild("C2", "Child 2");
    mn.insertChild("C3", "Child 3");

    cout << "\n>> INSERT RELASI M-N" << endl;
    mn.insertRelasi("P1", "C1");
    mn.insertRelasi("P1", "C2");

    mn.insertRelasi("P2", "C1");
    mn.insertRelasi("P2", "C3");

    // Parent P3 tidak memiliki relasi

    cout << "\n>> PRINT ALL DATA" << endl;
    mn.printAll();

    cout << "\n>> FIND CHILDREN OF P1" << endl;
    set<string> childsP1 = mn.findChildrenOf("P1");
    for(const auto& c : childsP1) cout << c << " ";
    cout << endl;

    cout << "\n>> FIND PARENTS OF C1" << endl;
    set<string> parentsC1 = mn.findParentsOf("C1");
    for(const auto& p : parentsC1) cout << p << " ";
    cout << endl;

    cout << "\n>> DELETE RELATION (P1, C2)" << endl;
    mn.deleteRelasi("P1", "C2");

    cout << "\n>> DELETE CHILD C1 (akan hilang dari semua parent)" << endl;
    mn.deleteChild("C1");

    cout << "\n>> DELETE PARENT P3" << endl;
    mn.deleteParent("P3");

    cout << "\n>> PRINT ALL DATA (SETELAH DELETE)" << endl;
    mn.printAll();

    return 0;
}
```

---
layout: post
title:  "STL dan Data Struktur"
date:   2018-09-18 19:00:00 +7
categories: [Data Structure]
author: Yonas Adiel
profpic: yonasadiel
author-description: Ketua Komunitas CP ITB 2018/2019
---

Post kali ini akan membahasa beberapa data struktur sederhana: vector, stack, queue,
dequeue, priority queue, map, dan set.

### Vector

#### Motivasi

Diberikan matriks berisi angka, berukuran $$N \times M$$. Tuliskan hasil rotasi
matriks 90 derajat. Batasan: $$1 \leq N \times M \leq 10^5$$

Soal di atas sebenarnya sangat mudah, namun batasan yang ada membuat kita berpikir
dua kali. Karena matriks cukup besar, kita perlu mendeklarasikan matriks di luar
program main. Namun, deklarasi matriks secara global membutuhkan ukuran array
secara eksplisit. Tidak mungkin kita membuat `int matriks[100000][100000]`
karena kebutuhan RAM yang dibutuhkan akan sampai 40 GB.

Karena itu, kita gunakan vektor yang ukurannya dinamis, yakni vektor.

#### Penggunaan

Contoh penggunaan vector untuk membaca array dan menuliskan array secara terbalik:

``` C++
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, x;
    vector<int> v;
    cin >> n;
    for (int i=0; i<n; i++) {
        cin >> x;
        v.push_back(x);
    }
    for (int i=v.size()-1; v>=0; v++) {
        cout << v[i] << endl;
    }
}
```

### Stack

Stack pada dasarnya seperti array, namun kita hanya dapat mengakses nilai terakhir
yang dimaukkan ke dalam stack.

#### Pengunaan

``` C++
#include <bits/stdc++.h>
using namespace std;

int main() {
    int q, x;
    stack<int> s;

    cin >> q;
    for (int i=0; i<q; i++) {
        cin >> x;
        s.push(x);                           // memasukkan elemen ke dalam stack
    }

    while (!s.empty()) {
        cout << s.top();         // menuliskan elemen terakhir yang belum di pop
        s.pop();                  // menghapus elemen terakhir yang belum di pop
    }
}
```

### Queue

#### Motivasi

Terdapat sebuah antrian dan K buah query. Query bisa berupa `add X` (orang bernama
`X` masuk ke dalam antrian) atau `process` (menuliskan nama orang yang berada di
antrian paling depan, lalu mengeluarkan orang tersebut dari antrian). Dijamin
dalam satu waktu, panjang antrian maksimum $$10^5$$, $$K \leq 10^8$$.

Kita dapat menyelesaikan masalah ini dengan array:

```
    string nama[100000], s, x;
    int head = -1, tail = 0;
    for (int i=0; i<k; i++) {
        cin >> s;
        if (s == "add") {
            cin >> x;
            nama[tail] = x;
            tail++;
        } else {
            cout << nama[head] << endl;
            head++;
        }
    }
```

Namun, kita tidak tahu ukuran array nama. Worst case, kita butuh menyimpan array
sebesar K (ketika query berisi `add` saja). Karena itu, kita gunakan queue.

#### Penggunaan

Berikut contoh penggunaan queue untuk soal tadi:

``` C++
#include <bits/stdc++.h>
using namespace std;

int main() {
    int k, x;
    string s;
    queue<int> q;

    cin >> k;
    for (int i=0; i<n; i++) {
        cin >> s;
        if (s == "add") {
            cin >> x;
            q.push(x);
        } else {
            cout << q.front() << endl;
            q.pop();
        }
    }
}
```

### Dequeue

Pada dasarnya seperti stack ditambah queue, kita dapat melakukan `push` di depan
dan di belakang, dan `pop` di depan dan di belakang.

### Priority Queue

#### Motivasi

Diberikan Q buah query. Query bisa berupa `add X` (memasukkan `X` ke dalam
sebuah array) atau `remove` (menghapus elemen terkecil dari array dan menuliskan
ke layar).

Kita dapat menyelesaikan program ini dengan array, namun kita perlu sorting
array tiap kali dilakukan operasi `add` atau `remove`. Dengan sort $$O(Q log Q)$$,
maka kompleksitas solusi ini menjadi $$O(Q^2 log Q)$$.

Di sini lah kita membutuhkan priority queue. Data struktur priority queue seperti
queue, namun queue selalu terurut tidak menururn. Akses pq dapat dilakukan dengan
`.push(x)` untuk memasukkan x, `.top()` untuk melihat nilai paling besar, dan `.pop()`
untuk menghapus elemen paling besar.

#### Penggunaan

Contoh penggunaan untuk soal di atas:

``` c++
#include <bits/stdc++h>
using namespace std;

int main() {
    int q, x;
    string s;
    priority_queue<int> pq;

    cin >> q;
    for (int i=0; i<q; i++) {
        cin >> s;
        if (s == "add") {
            cin >> x;
            pq.push(x);
        } else {
            cout << pq.top() << endl;
            pq.pop();
        }
    }
}
```

Perhatikan kalau jawaban di atas belum menyelesaikan soal pertama. Pada soal,
diminta **nilai minimum** dari array, sedangkan pq selalu terurut dari nilai
maksimum. Ada 2 cara untuk menyelesaikan ini:

1. Sebelum di `push`, nilai dikali `-1` terlebih dahulu, dan saat di `top`,
   nilai dikali `-1` sebelum di proses. Prio-queue akan selalu berisi negatif
   dan nilai maksimum di pq adalah nilai minimum dari input.
2. Deklarasikan pq dengan `priority_queue<int, vector<int>, greater<int> >`
   pq akan otomatis terurut tidak menurun.

### Map

under construction.

### Set

under construction.

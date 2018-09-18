---
layout: post
title:  "Pembahasan Marathon Contest"
date:   2017-03-02 06:00:00 +7
categories: [ Contest ]
author: Jauhar Arifin
profpic: jauhararifin
author-description: Entah apa
---

Gimana nih setelah melalui perjalanan panjang di kontes marathon kemarin. Pasti banyak yang penasaran sama solusi soal-soalnya kan. Oleh karena itu, pembahasan soalnya ada nih dibikin sama beberapa kontestan yang berhasil AC di soal yang bersangkutan.

### A. Gabriel and Caterpillar (Oleh Dery Rahman A., IF 2015)

Pada day 0, current height yang teramati adalah pada pukul 2pm. Sehingga pada hari ke 0, caterpillar akan naik setinggi
$$u \times 8$$ ($$u$$ merupakan jarak caterpillar naik perjamnya). Dan akan turun sebesar $$d\times 12$$ ($$d$$ merupakan jarakcaterpillar turun perjamnya). Pada day-day selanjutnya, ca terpillar akan naik setinggi $$u\times 12$$ dan turun sebesar $$d\times 12$$. Lakukan cek setiap kenaikan apakah sudah mencapai tinggi apel? Jika setelah day 0, tepatnya setelah kenaikan pada day 1, $$u\leq d$$ dan tinggi yang dicapai kurang dari tinggi apel, maka caterpillar tidak mungkin pernah dapat mencapai tujuan.

``` c
#include <iostream>
using namespace std;
int main(){
	int h, m, u, d, day = 0;
	scanf("%d %d %d %d", &h, &m, &u, &d);
	h+=u*8;
	if(h>=m) printf("%d\n", day);
	else {
		while(h<m && (day==0 || u>d)){
			day++;
			h-=d*12;
			h+=u*12;
		}
		if(h<m) printf("-1\n");
		else printf("%d\n", day);
	}
	return 0;
}
```

### B. Z-Sort (Oleh Dicky Novanto, IF 2015)
Perhatikan ciri-ciri array terurut z-sort pada soal!

1.	$$ai \geq ai - 1$$ for all even $$i$$,
2.	$$ai \leq ai - 1$$ for all odd $$i \gt 1$$.

Dari ciri-ciri yang disebutkan, jelas bahwa array yang terurut secara z-sort, jika divisualisasikan akan terbentuk seperti gunung dan lembah (elemen array di samping kiri dan kanan indeks elemen array yang ditunjuk lebih kecil atau sama dengan elemen array (gunung) atau dikanan-kiri elemen array indeks tertentu lebih besar atau sama dengan elemen array indeks tersebut (lembah)).

Pada dasarnya, untuk menyelesaikan persoalan ini, kita cukup melakukan pengecekan di tiap elemen array dan elemen array sebelumnya. Jika ada salah satu ciri-ciri array terurut z-sort, maka cukup dilakukan penukaran kedua elemen array untuk membuat array dari indeks awal hingga indeks tersebut terurut z-sort. Hal tersebut dapat terjadi karena ketika $$i>1$$, dan harus dilakukan penukaran elemen array antara indeks i dengan indeks $$i-1$$, maka pasti array indeks 0 hingga $$i-2$$ telah terurut z-sort, sehingga setelah penukaran, array dari indeks 0 hingga i telah terurut z-sort. Hal ini tentu berlaku hingga $$i = n$$.

### C. Cellular Network (Oleh Jauhar Arifin, IF 2015)

Untuk yang belum baca soalnya, jadi singkatnya begini : ada N buah kota dan M buah tower yang posisinya diinterpretasikan dalam sumbu-x. Setiap tower ini akan memberikan layanan jaringan untuk kota-kota yang jaraknya tidak lebih dari r dari tower tersebut. Pertanyaannya adalah berapa r minimal sehingga semua kota dapat terlayani oleh minimal satu tower.

Nah, pertama-tama kita baca dulu posisi tiap kota dan tower. Tingal dimasukin array aja. Misalnya kotanya dimasukin array p, towernya dimasukin array q. Jadi kurang lebih begini :
```c
scanf("%d%d", &n, &m);
for (int i = 0; i < n; i++)
	scanf("%d", p + i);
for (int i = 0; i < m; i++)
	scanf("%d", q + i);
```

Nah sekarang inti dari permasalahannya. Gimana sih caranya biar dapet r seminimum mungkin. Pertama kita harus observasi : setiap kota itu pasti paling enggak punya satu tower yang harus melayani dirinya. Nah biar jaraknya seminimum mungkin, untuk tiap kota kita bisa ambil tower paling dekat dengan dia. Kalau setiap kota mengambil tower yang paling dekat dengan dia, otomatis r-nya minimum kan? Nah jadi solusinya tinggal greedy aja, untuk setiap kota kita ambil tower yang selisih posisi x nya paling kecil.

Setelah dapet jarak setiap kota ke tower terdekat, langkah selanjutnya tinggal cari "jarak kota ke tower terdekat" yang maksimum. Bingung? intinya dari seluruh jarak terdekat itu kita cari yang terjauh, jadi kita mencari nilai maksimum dari jarak setiap kota ke tower terdekatnya.

Untuk nyari jarak terdekatnya ada berbagai macam cara sih, salah satunya dengan cara sorting. Kita bisa sort kota dan tower itu berdasarkan posisi x. Pertama-tama kita iterasi dari kota dengan x terkiri dan tower dengan x terkiri juga. Untuk setiap kota, kita cari tuh tower yang jaraknya paling dekat, caranya tinggal iterasi di towernya aja, kalau kita iterasi ke-kanan jaraknya bisa makin kecil, kita iterasi terus, sampe gabisa lagi ke kanan karena kalau ke kanan jaraknya akan tambah gede.

Setelah kota pertama ketemu tower terdekatnya, dilanjutin ke kota ke-dua. Untuk nyari towernya kota ke-dua, iterasinya nggak perlu dari awal lagi tinggal lanjutin towernya kota pertama aja. Nggak mungkin kan kota ke-dua yang lebih kanan dari kota pertama tower terdekatnya lebih kiri dari kota pertama. Nah makannya pasti towernya kota ke-dua itu sama kaya kota pertama atau lebih kanan dari kota pertama. Jadi tinggal iterasi tower kota pertamanya dilanjutin aja. Jadi totalnya kita iterasi dengan kompleksitas O(N).

Langkah terakhir tinggal cari nilai maksimum dari nilai minimum tadi. total kompleksitasnya O(N log N) karena kita butuh sort juga. Kurang lebih untuk bagian ini kodenya gini :
``` c
int r = -1;
for (int i = 0, j = 0; i < n; i++) {
	int minimum = abs(p[i]-q[j]);
	while (j < m-1 && abs(p[i]-q[j+1]) <= minimum)
		minimum = abs(p[i]-q[j+1]), j++;
	r = max(r, minimum);
}
printf("%d\n", r);
```
### D. Foe Pairs (Oleh Jordhy Fernando, IF 2015)

Soal ini dapat diselesaikan dengan prinsip inklusi-eksklusi. Pertama dihitung banyaknya interval yang mengandung suatu foe pair $$(a, b)$$ dari seluruh interval yang berakhir pada posisi terbesar antara a dan b. Jika terdapat lebih dari 1 foe pair yang berakhir pada posisi yang sama maka banyaknya interval yang tidak boleh dimasukkan adalah maksimum di antara foe pairs tersebut. Setelah itu, dilakukan iterasi dari 2 sampai n untuk melakukan update banyaknya interval yang mengandung foe pair dari seluruh interval yang berakhir pada posisi iterasi tersebut. Pada sebuah iterasi ke-i banyaknya interval tersebut adalah `max(dp[i], dp[i -1])` dengan `dp[i]` menyatakan banyaknya interval yang tidak boleh dimasukkan. Banyaknya interval yang tidak mengandung foe pair dapat dilakukan dengan cara menjumlahkan semua interval yang ada dikurangi dengan banyaknya interval yang tidak boleh.
Misalkan $$p = [1,3,2,4]$$, dan foe pairnya adalah $${(3,2), (4,1), (1,2)}$$. Foe pair $$(3,2)$$ terletak pada interval yang berakhir pada posisi 3 di p. Untuk interval yang berkakhir pada posisi tersebut akan terdapat 2 interval yaitu $${(1,3), (2,3)}$$ yang mengandung foe pair tersebut sehingga `dp[3] = 2`. Lalu untuk foe pair $$(4,2)$$ yang terletak pada interval yang berakhir pada posisi 4 di p, akan terdapat 1 interval yaitu $$(1,4)$$ yang mengandung foe pair tersebut sehingga `dp[4] = 1`. Foe pair $$(1,2) $$terletak pada interval yang berakhir pada posisi 3 di p juga. Karena banyaknya interval yang mengandung foe pair tersebut ada 1 interval $$(1,3)$$, maka `dp[3]` akan tetap bernilai 2.  Setelah itu dilakukan update sehingga nilai setiap dp menjadi sebagai berikut.
```
dp[1] = 0
dp[2] = 0
dp[3] = 2
dp[4] = 2 (berubah dari 1 menjadi 2 karena pada interval yang tidak boleh dimasukkan pada interval yang berakhir di posisi 3 pasti juga tidak boleh pada posisi yang > 3)
```
Jawaban dapat didapatkan dengan menjumlahkan semua interval yang ada dikurangi dengan interval yang tidak boleh yaitu $$(10-4) = 6$$. Kode untuk solusi soal ini kurang lebih begini :

``` c
/* Author : Jordhy Fernando */
#include<bits/stdc++.h>
#define ll long long
#define EPS 1e-12

using namespace std;
typedef vector<int> vi;
typedef pair<int, int> ii;
typedef vector<ii> vii;
using namespace std;

int main()
{
    int n, m;
    vi pos(300001), dp(300001);
    scanf("%d %d", &n, &m);
    for(int i = 0; i < n; i++){
        int x;
        scanf("%d", &x);
        pos[x] = i;
        dp[i] = 0;
    }
    for(int i = 0; i < m; i++){
        int a, b;
        scanf("%d %d", &a, &b);
        if(pos[a] > pos[b]){
            swap(a,b);
        }
        dp[pos[b]] = max(dp[pos[b]], pos[a] + 1);
    }
    for(int i = 1; i < n; i++){
        dp[i] = max(dp[i], dp[i - 1]);
    }
    ll ans = 0;
    for(int i = 0; i < n; i++){
        ans += i + 1 - dp[i];
    }
    printf("%lld\n", ans);
    return 0;
}
```

### E. Nested Segments (Oleh Turfa Auliarachman, IF 2015)

Sebelum membaca pembahasan soal ini, sangat disarankan untuk membaca dulu materi tentang struktur data yang mendukung point update-range query, seperti Binary Indexed Tree (BIT) atau Segment Tree, lalu coba lagi untuk mengerjakan soal ini.

Inti dari soal ini adalah diberi n pasangan bilangan $$(l, r)$$ dengan $$0 \leq n \leq 2\times 10^5$$. Untuk setiap i dengan $$1 \leq i \leq n$$, cari banyak j berbeda di mana $$l_i < l_j$$ dan $$r_j < r_i$$.

Dengan melihat deskripsi permasalahan tersebut, kita dapat menyimpulkan algoritma brute force akan memiliki kompleksitas waktu $$O(n^2)$$ dan tentu itu bukan solusi yang diharapkan.

Jika kita urutkan data berdasarkan nilai l secara menurun, kita akan dapati bahwa $$l_i < l_j$$ jika dan hanya jika $$i < j$$. Maka, untuk setiap pasangan bilangan $$(l_i, r_i)$$, kita tinggal mencari ada berapa bilangan j yang kurang dari i dengan $$r_j < r_i$$.

Sekarang, bagaimana cara mencari banyaknya bilangan j yang memenuhi untuk setiap i seperti didefinisikan di atas?

Ada dua cara yang bisa digunakan :
1.	Kita loop j dari 0 sampai i-1 dan cari berapa banyak j sedemikian sehinnga $$r_j < r_i$$.
2.	Kita maintain sebuah array `visitedR[r]`. Ketika kita sedang mencari jawaban untuk pasangan bilangan ke-i, `visitedR[r]` akan berisi, "Ada berapa banyak j dengan $$j < i$$, yang memiliki $$r_j = r$$?". Lalu kita jumlahkan `visitedR[0] + visitedR[1] + .. + visitedR[r_i - 1]`. Jika sudah selesai, kita tambahkan `visitedR[r_i]` dan maju ke i selanjutnya, sehingga definisi `visitedR[r]` masih berlaku.

Untuk opsi pertama, algoritma tersebut sama saja dengan algoritma brute force dengan kompleksitas $$O(n^2)$$. Dengan feeling, kita bisa "merasakan" bahwa algoritma ini tidak dapat dioptimasi lagi.

Untuk opsi kedua malah lebih parah lagi. Satu kali menjumlahkan `visitedR[0] + visitedR[1] + ... + visitedR[r]`, kita memiliki kompleksitas $$O(r)$$. Maka algoritma tersebut akan berjalan dengan kompleksitas waktu $$O(nr)$$ dan kompleksitas ruang $$O(r)$$ dengan r bisa mencapai $$10^9$$.

Namun......

Kita perhatikan bahwa walaupun l dan r bisa bernilai sampai $$10^9$$, tetapi banyak bilangan berbeda maksimal hanya $$2n$$. Maka, kita bisa mengkompresi pasangan bilangan l dan r. Misal, jika kita memiliki kumpulan pasangan bilangan $$[(1000000, 234224143), (1, 29999), (-1782, 46714141)]$$, kita bisa mengubah datanya menjadi $$[(2, 4), (1, 2), (0, 3)]$$. Dengan begitu, bilangan maksimum yang mungkin untuk l dan r adalah $$399999$$.

Walaupun begitu, kompleksitas $$O(nr)$$ masih tidak bisa diterima. Tetapi ternyata, persoalan ini adalah persoalan klasik, yaitu point update-range query. Dalam suatu waktu, kita bisa mengubah `visitedR[v] = visitedR[v] + w` atau menjumlahkan `visitedR[u] + visitedR[u+1] + visitedR[u + 2] + visitedR[u + 3] + ... + visitedR[v]`, dengan $$u = 0, v = r$$, dan $$w = 1$$. Persoalan ini dapat diselesaikan menggunakan struktur data khusus, contohnya adalah BIT dan Segment Tree. Menggunakan struktur data tersebut, kedua jenis operasi dapat dilakukan dalam kompleksitas waktu $$O(log r)$$ dan kompleksitas ruang $$O(r)$$.
Jadi, secara keseluruhan, kompleksitas waktunya adalah $$O(n log n + n log r) = O(n log n)$$ dan kompleksitas ruangnya adalah $$O(r) = O(n)$$. (kita perhatikan bahwa maksimal r ada 2n).

Implementasi kode program dengan BIT-nya kurang lebih gini :
``` c
#include <bits/stdc++.h>
using namespace std;

typedef pair<int,int> pi;

#define sc second
#define fi first
#define mp make_pair

const int maxBIT = 400000;
int bit[maxBIT + 10];
int realBIT;

void update(int u){
    for(;u <= realBIT; u += (u & (-u))){
        bit[u]++;
    }
}

int query(int u){
    int ret = 0;
    for(; u > 0; u -= (u & (-u))){
        ret += bit[u];
    }
    return ret;
}

bool cmp(pair<int, pi> &x, pair<int, pi> &y){
    if (x.sc.fi == y.sc.fi) return x.sc.sc < y.sc.sc;
    return x.sc.fi > y.sc.fi;
}

int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n;
    vector<pair<int, pi>> inp;
    set<int> used;
    map<int, int> mapping;
    vector<int> jawaban;

    cin >> n;
    for(int i = 0; i < n; i++){
        int l,r;
        cin >> l >> r;

        inp.push_back(mp(i, mp(l,r)));
        jawaban.push_back(0);
        used.insert(l);
        used.insert(r);
    }

    realBIT = used.size();

    int to = 1;
    for(auto iter = used.begin(); iter != used.end(); iter++){
        mapping[*iter] = to++;
    }

    for(int i = 0 ; i <inp.size(); i++){
        inp[i].sc.fi = mapping[inp[i].sc.fi];
        inp[i].sc.sc = mapping[inp[i].sc.sc];
    }

    sort(inp.begin(), inp.end(), cmp);

    for(auto now : inp){
        jawaban[now.fi] = query(now.sc.sc);
        update(now.sc.sc);
    }

    for(auto ans : jawaban){
        cout << ans << endl;
    }
}
```

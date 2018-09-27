---
layout: post
title:  "Basic Math"
date:   2018-09-26 21:30:00 +7
categories: [Math]
author: Tony
profpic: tony
author-description: Informatika 2016
---

Post kali ini akan membahas beberapa algoritma dan/atau materi tentang math yang sering dipakai.

#### Greatest Common Divisor (GCD)
Mencari FPB merupakan hal yang sering kali dilakukan pada soal-soal math dan juga sering digunakan oleh orang-orang. Berikut implementasi C++ dengan menggunakan algoritma euclid yang menggunakan sifat FPB, yaitu
`fpb(a, b) = fpb(a, b - a)`

```c++
int gcd(int a, int b){
	if(b==0) return a;
	int r = a%b;
	while(r){
		a = b;
		b = r;
		r = a%b;
	}
	return r;
}
```

Akan tetapi, C++ sudah memberikan STL (Standard Template Library) untuk fungsi FPB, yaitu `__gcd`, dengan penggunaan sebagai berikut.

``` cpp
#include <cmath>

int main(){
	int a, b;
	a = 4;
	b = 10;
	printf("%d\n", __gcd(a, b));
	/* mengeluarkan 2 */
}
```

#### Binary Exponentiation / Fast Power
Untuk perpangkatan dapat dilakaukan dengan menggunakan sifat bahwa

A<sup>N</sup> = A * A <sup>N-1</sup>

Namun, pada implementasinya algoritma tersebut memiliki kompleksitas waktu `O(N)`, sehingga untuk menghitung N yang besar (> 10<sup>7</sup>) dapat memakan waktu yang lama. Terdapat suatu sifat pada perpangkant yang dapat digunakan untuk mempercepat perhitungan, yaitu

A<sup>N</sup> = (A<sup>N/2</sup>) * (A<sup>N/2</sup>)

Implementasi untuk algoritma diatas akan memiliki kompleksitas `O(log(N))`. berikut implementasinya

```c++
/* versi rekursif */
int binex(int val, int exp){
	if(exp == 0)
		return 1;
	int ans = binex(val, exp/2);
	ans *= ans;
	if(exp%2 == 1)
		ans *= val;
	return ans;
}

/* versi iteratif */
int binex(int val, int exp){
	int ans = 1;
	for(;exp;exp>>=1){
		if(exp%2==1)
			ans *= val;
		val *= val;
	}
	return ans;
}
```

#### Inverse Modulo
Inverse modulo merupakan cara untuk mencari suatu nilai `A`</sup> pada `X*A ≡ 1 (mod M)`. Syarat agar terdapat nilai `A` adalah `FPB(X, M)` haruslah 1 atau `X` dan `M` koprima. Ada terdapat 3 cara untuk menghitung nilai inverse modulo, yaitu:
##### 1. Extended Euclidean<br>
perhatikan bahwa `FPB(A, M) = 1`, sehingga dapat ditulis persamaan untuk sembarang `x` dan `y`<br>
`Ax + My = 1` <br>
untuk mencari nilai `x` dan `y`  dapat digunakan algoritma Extended Euclidean. Berikut implementasinya

```c++
int gcd(int a, int b, int &x, int &y) {
    if (a == 0) {
        x = 0; y = 1;
        return b;
    }
    int x1, y1;
    int d = gcd(b%a, a, x1, y1);
    x = y1 - (b / a) * x1;
    y = x1;
    return d;
}

int inverse_modulo(int a, int m){
	int x, y;
	int ans = gcd(a, m, x, y);
	return (x%m + x)%m;
}
```

Kompleksitas algoritma diatas adalah `O(log(N))` per pertanyaan.

##### 2. Euler Totient Function & Binary Exponentiation <br>
Teorema Euler menyatakan bahwa jika suatu nilai `A` dan `M` koprima, maka memenuhi persamaan berikut<br>
`A`<sup>phi(`M`)</sup> ≡ `1 (mod M)` <br>
phi(`M`) merupakan fungsi yang menghitung banyak angka dari `1` hingga `M` yang koprima dengan bilangan `M`.

```c++
int phi(int val){
	int ans = val;
	for(int i=2;i*i<=val;++i){
		if(val % i==0){
			ans -= ans/i;
			do{
				val/=i;
			}while(val%i==0);
		}
	}
	if(val>1)
		ans -= ans/val;
	return ans;
}

int binex(int val, int exp, int mod){
	int ans = 1;
	for(val%=mod;exp;exp>>=1){
		if(exp&1)
			ans = 1LL*ans*val % mod;
		val = 1LL*val*val % mod;
	}
	return ans;
}

int inverse_modulo(int a, int m){
	return binex(a, phi(m)-1, m);
}
```
Implementasi diatas memiliki kompleksitas waktu `O(SQRT(N))` per sekali pertanyaan. Untuk menghitung menghitung inverse modulo yang banyak dengan `Q` pertanyaan, implementasi diatas dapat dioptimasi dengan menambahkan Sieve dan iterasi pada fungsi phi hanya dengan prima sehingga mengubah kompleksitasnya menjadi `O(N*log(log(N)) + Q*log(N))`

##### 3. Modular Multiplicative Inverse <br>
Perhatikan penurunan rumus berikut. <br>
M mod A = M - ⌊M/A⌋ * A <br>
dengan memodulokan kedua sisi. <br>
M mod A ≡ - ⌊M/A⌋ * A (mod M) <br>
dengan mengalikan kedua sisi dengan A<sup>-1</sup> * (M mod A)<sup>-1</sup>, didapat <br>
M mod A * A<sup>-1</sup> * (M mod A)<sup>-1</sup> ≡ - ⌊M/A⌋ * A * A<sup>-1</sup> * (M mod A)<sup>-1</sup> (mod M) <br>
A<sup>-1</sup> ≡ - ⌊M/A⌋ * (M mod A)<sup>-1</sup> (mod M) <br>
sehingga didapat <br>
inverse(A) = - ⌊M/A⌋ * inverse(M mod A) (mod M)

```c++
int inverse_modulo(int a, int m){
	if(val == 1) return 1;
	int ans = 1LL*(-m/a)*inverse_modulo(m%a, m) % m;
	if(ans < 0)
		ans += m;
	return ans;
}
```
Implementasi diatas memiliki kompleksitas `O(log(m))` per pertanyaan dan hanya bisa untuk menghitung inverse modulo dari `1` hingga `m-1`. Untuk perhitungan inverse modulo yang banyak, dapat dioptimasi dengan dynamic programming.

#### Sieve of Eratosthenes
Merupakan algoritma yang dapat digunakan untuk mendapatkan informasi dari 1 sampai dengan N apakah blangan tersebut prima dengan kompleksitas waktu `O(N*log(log(N)))`.

Cara kerja algoritma ini adalah :
1. Buatlah list ankga A yang berisi `2, 3, 4, ..., n-1, n`
2. Untuk tiap angka pada indeks ke-i, hapus semua kelipatan `A[i]` dari list, yaitu `2*A[i], 3*A[i], 4*A[i], ...`
3. Ulangi langkah 2 sampai tidak ada angka yang bisa diiterasi.

Akan lebih baik membaca [laman ini](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) terlebih dahulu untuk pemahaman yang lebih baik.

``` cpp
/* implementasi O(N*log(log(N))) */
bool isprime[MAXN];

void sieve(int N){
	memset(isprime, true, sizeof isprime);
	isprime[0] = isprime[1] = false;
	for(int i=2;i<=(int)sqrt(N);++i){
		if(isprime[i]){
			for(int j=i*i;j<=N;j+=i){
				isprime[j] = false;
			}
		}
	}
}
```

Waktu implementasi diatas dapat sedikit dioptimasi sehingga memiliki kompleksitas linear namum kompleksitas ruangnya bertambah.

``` cpp
/* implementasi O(N) */
int min_prime_factor[MAXN];
vector<int> prime;

void sieve(int N){
	memset(min_prime_factor, 0, sizeof min_prime_factor);
	for(int i=2;i<=(int)sqrt(N);++i){
		if(min_prime_factor[i] == 0){
			min_prime_factor[i] = i;
			prime.push_back(i);
		}
		for(int j=0;j<(int)prime.size() && prime[j]<=min_prime_factor[i] && prime[j] <= N/i;++j){
			min_prime_factor[i * prime[j]] = prime[j];
		}
	}
}
```

Perlu diingat bahwa konsep <b>sieve</b> tidak hanya dapat diterapkan pada penghitungan prima.

#### Sumber

wikipedia<br>
[cp-algorithms.com](cp-algorithms.com)
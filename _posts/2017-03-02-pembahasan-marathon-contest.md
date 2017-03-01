---
layout: post
title:  "Pembahasan Marathon Contest"
date:   2017-03-02 06:00:00 +7
categories: Contest Pembahasan
author: Jauhar Arifin
profpic: jauhararifin
author-description: Entah apa
---

Gimana nih setelah melalui perjalanan panjang di kontes marathon kemarin. Pasti banyak yang penasaran sama solusi soal-soalnya kan. Oleh karena itu, pembahasan soalnya ada nih dibikin sama beberapa kontestan yang berhasil AC di soal yang bersangkutan.

## A. Gabriel and Caterpillar
## B. Z-Sort (Oleh Dicky Novanto, IF 2015)
Perhatikan ciri-ciri array terurut z-sort pada soal!

1.	ai ≥ ai - 1 for all even i,
2.	ai ≤ ai - 1 for all odd i > 1.

Dari ciri-ciri yang disebutkan, jelas bahwa array yang terurut secara z-sort, jika divisualisasikan akan terbentuk seperti gunung dan lembah (elemen array di samping kiri dan kanan indeks elemen array yang ditunjuk lebih kecil atau sama dengan elemen array (gunung) atau dikanan-kiri elemen array indeks tertentu lebih besar atau sama dengan elemen array indeks tersebut (lembah)).

Pada dasarnya, untuk menyelesaikan persoalan ini, kita cukup melakukan pengecekan di tiap elemen array dan elemen array sebelumnya. Jika ada salah satu ciri-ciri array terurut z-sort, maka cukup dilakukan penukaran kedua elemen array untuk membuat array dari indeks awal hingga indeks tersebut terurut z-sort. Hal tersebut dapat terjadi karena ketika i>1, dan harus dilakukan penukaran elemen array antara indeks i dengan indeks i-1, maka pasti array indeks 0 hingga i-2 telah terurut z-sort, sehingga setelah penukaran, array dari indeks 0 hingga i telah terurut z-sort. Hal ini tentu berlaku hingga i = n.
## C. Cellular Network
## D. Foe Pairs
## E. Nested Segments

# Simulasi Tahapan Kompilasi pada Struktur Perulangan `while`

**Mata Kuliah:** Teknik Kompilasi
**Tugas:** Proyek Akhir UAS
**Bahasa Pemrograman:** Python

---

# Pendahuluan

## Latar Belakang

Compiler merupakan perangkat lunak yang bertugas menerjemahkan program yang ditulis menggunakan bahasa pemrograman tingkat tinggi menjadi bahasa yang dapat dipahami oleh komputer. Proses kompilasi dilakukan melalui beberapa tahapan, yaitu analisis leksikal, analisis sintaksis, analisis semantik, hingga pembangkitan kode antara (*Intermediate Code*).

Pada tugas ini dibuat simulasi sederhana proses kompilasi terhadap konstruksi **perulangan `while`** menggunakan bahasa pemrograman Python. Program tidak dibuat sebagai compiler sesungguhnya, melainkan sebagai media pembelajaran untuk memahami bagaimana setiap tahapan kompilasi bekerja.

---

# Tujuan

Tujuan dari tugas ini adalah:

* Memahami proses kerja compiler.
* Memahami tahapan analisis leksikal.
* Memahami proses pembentukan Abstract Syntax Tree (AST).
* Memahami analisis semantik.
* Memahami proses pembangkitan Three Address Code (TAC).

---

# Konstruksi yang Dipilih

Konstruksi yang digunakan pada proyek ini adalah **Perulangan (`while`)**.

Contoh kode sumber yang digunakan:

```c
while (x < 10) {
    x = x + 1;
}
```

---

# Pola Tata Bahasa (BNF)

Grammar sederhana yang digunakan adalah sebagai berikut:

```bnf
<while_stmt> ::= "while" "(" <condition> ")" "{" <statement> "}"

<condition> ::= <identifier> <relop> <number>

<statement> ::= <identifier> "=" <expression>

<expression> ::= <identifier> "+" <number>
               | <identifier> "-" <number>
               | <number>

<identifier> ::= letter {letter | digit}

<number> ::= digit {digit}

<relop> ::= "<"
          | ">"
          | "<="
          | ">="
          | "=="
          | "!="
```

Grammar tersebut digunakan sebagai aturan agar parser dapat memvalidasi struktur sintaks dari kode yang diberikan.

---

# Tahapan Kompilasi

## 1. Analisis Leksikal (Lexical Analysis)

Tahapan pertama adalah memecah source code menjadi sekumpulan token.

Source Code:

```c
while (x < 10) {
    x = x + 1;
}
```

Hasil tokenisasi:

```text
while
(
x
<
10
)
{
x
=
x
+
1
;
}
```

Pada tahap ini setiap kata, operator, angka, maupun simbol dikenali sebagai token.

---

## 2. Analisis Sintaksis (Syntax Analysis)

Setelah token berhasil diperoleh, parser memeriksa apakah urutan token sesuai dengan grammar yang telah ditentukan.

Jika valid, parser membentuk Abstract Syntax Tree (AST).

Representasi AST:

```text
While
│
├── Condition
│      ├── x
│      ├── <
│      └── 10
│
└── Body
       └── Assignment
             ├── x
             └── +
                 ├── x
                 └── 1
```

AST menunjukkan hubungan antar elemen program sehingga lebih mudah diproses pada tahap berikutnya.

---

## 3. Analisis Semantik (Semantic Analysis)

Tahap semantik bertugas memastikan bahwa program memiliki makna yang benar.

Pada implementasi ini dilakukan pemeriksaan sederhana berupa:

* Variabel telah dideklarasikan.
* Variabel dapat digunakan dalam kondisi.
* Variabel dapat digunakan dalam proses assignment.

Hasil pemeriksaan:

```text
Semantic Analysis : VALID
```

---

## 4. Generasi Three Address Code (TAC)

Tahapan terakhir menghasilkan kode antara (Intermediate Code) dalam bentuk Three Address Code.

Hasil TAC:

```text
L1:
ifFalse x < 10 goto L2

t1 = x + 1
x = t1

goto L1

L2:
```

Penjelasan:

* **L1** merupakan awal perulangan.
* Jika kondisi salah maka program berpindah ke **L2**.
* Variabel sementara (`t1`) digunakan untuk menyimpan hasil operasi.
* Setelah assignment selesai, eksekusi kembali ke label awal.

---

# Implementasi Program

Program dibuat menggunakan Python.

Tahapan yang diimplementasikan meliputi:

1. Lexical Analysis
2. Syntax Analysis
3. Semantic Analysis
4. Three Address Code Generator

Alur program:

```text
Source Code
      │
      ▼
Lexical Analysis
      │
      ▼
Syntax Analysis
      │
      ▼
Abstract Syntax Tree
      │
      ▼
Semantic Analysis
      │
      ▼
Three Address Code
```

---

# Hasil Eksekusi

Output token:

```text
['while', '(', 'x', '<', '10', ')', '{',
 'x', '=', 'x', '+', '1', ';', '}']
```

Output AST:

```text
While
│
├── Condition
│      ├── x
│      ├── <
│      └── 10
│
└── Assignment
       ├── x
       └── x + 1
```

Output Semantic:

```text
VALID
```

Output Three Address Code:

```text
L1:
ifFalse x < 10 goto L2

t1 = x + 1
x = t1

goto L1

L2:
```

---

# Analisis

Dari hasil implementasi dapat disimpulkan bahwa setiap tahapan compiler memiliki fungsi yang berbeda.

* Analisis leksikal mengubah karakter menjadi token.
* Analisis sintaksis memvalidasi struktur program dan membentuk AST.
* Analisis semantik memastikan makna program sudah benar.
* Three Address Code menghasilkan representasi kode antara yang lebih mudah dioptimasi dan diterjemahkan ke bahasa mesin.

Walaupun implementasi masih sederhana, konsep utama proses kompilasi telah berhasil direpresentasikan.

---

# Kesimpulan

Simulasi compiler yang dibuat berhasil menggambarkan empat tahapan utama proses kompilasi, yaitu:

* Analisis Leksikal
* Analisis Sintaksis
* Analisis Semantik
* Generasi Three Address Code (TAC)

Program mampu menerima input berupa struktur perulangan `while`, melakukan tokenisasi, membentuk Abstract Syntax Tree (AST), memvalidasi semantik sederhana, serta menghasilkan Three Address Code sebagai representasi kode antara.

Implementasi ini menunjukkan bagaimana sebuah compiler bekerja secara bertahap sebelum akhirnya menghasilkan kode yang siap diterjemahkan menjadi bahasa mesin.

---

# Referensi

1. Aho, A. V., Lam, M. S., Sethi, R., & Ullman, J. D. *Compilers: Principles, Techniques, and Tools (2nd Edition)*. Pearson Education.

2. Grune, D., Jacobs, C. J. H. *Parsing Techniques: A Practical Guide*. Springer.

3. Dokumentasi Python. https://docs.python.org/3/

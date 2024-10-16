# Tugas6_KriptografiPlayfair Cihper

NAMA : ZULAEHA

NIM : 312210575

Kelas : TI.22.A.5

![image](https://github.com/user-attachments/assets/d3da5fd3-788e-48e4-b1c5-4bc09df10cb1)


- Berikut code pyhton nya :
```
import re

    # Fungsi untuk membersihkan input teks dan menyesuaikan panjangnya def prepare_text(text):
    # Menghilangkan karakter non-huruf dan mengganti J dengan I
    text = re.sub(r'[^A-Z]', '', text.upper().replace('J', 'I'))
    # Memisahkan pasangan huruf yang sama dengan X
    prepared_text = ''
    i = 0
    while i < len(text):
        prepared_text += text[i]
        if i + 1 < len(text) and text[i] == text[i + 1]:
            prepared_text += 'X'
            i += 1
        elif i + 1 < len(text):
            prepared_text += text[i + 1]
            i += 2
        else:
            # Tambahkan X jika huruf terakhir berdiri sendiri
            prepared_text += 'X'
            i += 1
    return prepared_text

# Fungsi untuk membuat matriks Playfair dari kunci
def create_playfair_matrix(key):
    key = re.sub(r'[^A-Z]', '', key.upper().replace('J', 'I'))
    matrix = []
    for char in key:
        if char not in matrix:
            matrix.append(char)
    for char in 'ABCDEFGHIKLMNOPQRSTUVWXYZ':
        if char not in matrix:
            matrix.append(char)
    return [matrix[i:i+5] for i in range(0, 25, 5)]

# Fungsi untuk mendapatkan posisi huruf di matriks
def find_position(char, matrix):
    for row in range(5):
        for col in range(5):
            if matrix[row][col] == char:
                return row, col
    return None

# Fungsi untuk enkripsi Playfair Cipher
def playfair_encrypt(plaintext, matrix):
    ciphertext = ''
    plaintext = prepare_text(plaintext)
    for i in range(0, len(plaintext), 2):
        row1, col1 = find_position(plaintext[i], matrix)
        row2, col2 = find_position(plaintext[i+1], matrix)
        if row1 == row2:
            ciphertext += matrix[row1][(col1+1) % 5] + matrix[row2][(col2+1) % 5]
        elif col1 == col2:
            ciphertext += matrix[(row1+1) % 5][col1] + matrix[(row2+1) % 5][col2]
        else:
            ciphertext += matrix[row1][col2] + matrix[row2][col1]
    return ciphertext

# Fungsi untuk dekripsi Playfair Cipher
def playfair_decrypt(ciphertext, matrix):
    plaintext = ''
    for i in range(0, len(ciphertext), 2):
        row1, col1 = find_position(ciphertext[i], matrix)
        row2, col2 = find_position(ciphertext[i+1], matrix)
        if row1 == row2:
            plaintext += matrix[row1][(col1-1) % 5] + matrix[row2][(col2-1) % 5]
        elif col1 == col2:
            plaintext += matrix[(row1-1) % 5][col1] + matrix[(row2-1) % 5][col2]
        else:
            plaintext += matrix[row1][col2] + matrix[row2][col1]
    return plaintext

# Main program
key = "TEKNIK INFORMATIKA"
plaintexts = [
    "GOOD BROOM SWEEP CLEAN",
    "REDWOOD NATIONAL STATE PARK",
    "JUNK FOOD AND HEALTH PROBLEMS"
]

# Buat matriks Playfair
matrix = create_playfair_matrix(key)

# Lakukan enkripsi dan dekripsi untuk setiap plaintext
for plaintext in plaintexts:
    encrypted = playfair_encrypt(plaintext, matrix)
    decrypted = playfair_decrypt(encrypted, matrix)
    print(f"Plaintext: {plaintext}")
    print(f"Encrypted: {encrypted}")
    print(f"Decrypted: {decrypted}\n")
```

- Hasilnya adalah :

![kriptografi](https://github.com/user-attachments/assets/e5fc14ae-be5b-4d61-89bd-efddf9715157)


## Penjelasan singkat :

1. Kunci dan Matriks Playfair:

Kunci yang digunakan adalah "TEKNIFORMA".
Matriks Playfair 5x5 dibuat dari kunci tersebut, dan huruf berulang dihilangkan. Sisanya diisi dengan huruf alfabet lainnya kecuali J.

Matriksnya menjadi :
```
T E K N I
F O R M A
B C D G H
L P Q S U
V W X Y Z
```

2. Plaintext:

Plaintext yang akan dienkripsi adalah "GOOD BROOM SWEEP CLEAN".
Teks diubah menjadi pasangan huruf: "GO", "OD", "BR", "OO", "MS", "WE", "EP", "CL", "EA", "NX" (huruf X ditambahkan untuk panjang ganjil).

3. Proses Enkripsi:

Setiap pasangan huruf diambil dari matriks, lalu dienkripsi dengan aturan Playfair:

- Jika kedua huruf berada dalam baris yang sama, geser ke kanan.
  
- Jika dalam kolom yang sama, geser ke bawah.
  
- Jika tidak, tukar dengan huruf di sudut berlawanan dari persegi panjang yang dibentuk.
  
Contoh enkripsi untuk beberapa pasangan:

GO: Huruf G (baris 3, kolom 4) dan O (baris 2, kolom 2) → jadi C dan M.

OD: Huruf O (baris 2, kolom 2) dan D (baris 3, kolom 3) → jadi R dan C.

BR: Huruf B (baris 3, kolom 1) dan R (baris 2, kolom 3) → jadi D dan F.

Ulangi proses ini untuk setiap pasangan huruf sampai seluruh plaintext terenkripsi.

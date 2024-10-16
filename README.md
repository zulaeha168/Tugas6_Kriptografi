# Tugas6_KriptografiPlayfair Cihper

NAMA : ZULAEHA

NIM : 312210575

Kelas : TI.22.A.5

![kriptografi tugas 6](https://github.com/user-attachments/assets/1513ab15-d819-41b5-9360-2c32f6148175)



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

- Outputnya

![output kriptografi](https://github.com/user-attachments/assets/653dc9a9-ffe3-4b05-ac07-da443b350b1b)



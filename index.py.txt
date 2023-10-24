# Fungsi untuk mengenkripsi pesan dengan Vigenere Cipher
def vigenere_encrypt(plain_text, key):
    encrypted_text = ""
    key_length = len(key)
    for i, char in enumerate(plain_text):
        shift = ord(key[i % key_length]) - ord('A')
        if char.isalpha():
            if char.islower():
                encrypted_text += chr(((ord(char) - ord('a') + shift) % 26) + ord('a'))
            else:
                encrypted_text += chr(((ord(char) - ord('A') + shift) % 26) + ord('A'))
        else:
            encrypted_text += char
    return encrypted_text

# Fungsi untuk mendekripsi pesan dengan Vigenere Cipher
def vigenere_decrypt(encrypted_text, key):
    decrypted_text = ""
    key_length = len(key)
    for i, char in enumerate(encrypted_text):
        shift = ord(key[i % key_length]) - ord('A')
        if char.isalpha():
            if char.islower():
                decrypted_text += chr(((ord(char) - ord('a') - shift) % 26) + ord('a'))
            else:
                decrypted_text += chr(((ord(char) - ord('A') - shift) % 26) + ord('A'))
        else:
            decrypted_text += char
    return decrypted_text

# Fungsi untuk mengenkripsi pesan dengan Transposisi Rail Fence
def rail_fence_encrypt(plain_text, key):
    encrypted_text = [''] * key
    level = 0
    direction = 1

    for char in plain_text:
        encrypted_text[level] += char
        if level == 0:
            direction = 1
        elif level == key - 1:
            direction = -1
        level += direction

    return ''.join(encrypted_text)

# Fungsi untuk mendekripsi pesan dengan Transposisi Rail Fence
def rail_fence_decrypt(encrypted_text, key):
    decrypted_text = [''] * key
    row = 0
    direction = 1

    for char in encrypted_text:
        decrypted_text[row] += char
        if row == 0:
            direction = 1
        elif row == key - 1:
            direction = -1
        row += direction

    result = ''
    for i in range(key):
        result += decrypted_text[i]

    return result

# Fungsi untuk mengenkripsi pesan dengan Vigenere dan kemudian Rail Fence
def encrypt_message(plain_text, vigenere_key, rail_fence_key):
    vigenere_encrypted = vigenere_encrypt(plain_text, vigenere_key)
    rail_fence_encrypted = rail_fence_encrypt(vigenere_encrypted, rail_fence_key)
    return rail_fence_encrypted

# Fungsi untuk mendekripsi pesan dengan Rail Fence dan kemudian Vigenere
def decrypt_message(encrypted_text, vigenere_key, rail_fence_key):
    rail_fence_decrypted = rail_fence_decrypt(encrypted_text, rail_fence_key)
    vigenere_decrypted = vigenere_decrypt(rail_fence_decrypted, vigenere_key)
    return vigenere_decrypted

# Contoh penggunaan
plain_text = "HELLO WORLD"
vigenere_key = "KEY"
rail_fence_key = 3

# Proses enkripsi
encrypted_message = encrypt_message(plain_text, vigenere_key, rail_fence_key)
print("Encrypted Message:", encrypted_message)

# Proses dekripsi
decrypted_message = decrypt_message(encrypted_message, vigenere_key, rail_fence_key)
print("Decrypted Message:", decrypted_message)

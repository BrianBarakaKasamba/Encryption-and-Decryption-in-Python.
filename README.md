# Offensive-Python
Python for exploits.

CHROME PASSWORD STEALER FOR WINDOWS OPERATING SYSTEM.(AS FROM BUILD 1803)
This is a python implementation of geting unnecrypted passwords stored in chrome
CHROME uses AES encryption which is a systemmetriic key algorithm to encrypt/decrypt 

STEPS TO *HACK* CHROME PASSWORDS
.Find encryption key
  .Find encrypted passworrds
   .nderand AES cryptography
   .Decrypt saved passwords

FIND ENCRYPTION KEY
An encryption key is random string of bits generated specifically to scramble and unscramble data. Encryption keys are created with algorithms designed to ensure that each key is unique and unpredictable.
  __for windows__
  The key is stored in Json format
  in the following location C:\Users\<PC Name>\AppData\Local\Google\Chrome\User Data\Local State
.Open file with text editor(Press Ctrl + F) search word "ecrypted_key"

FIND ENCRYPTED PASSWORDS
They are stored in an SQLite Database
FOund in following location in PC C:\Users\<PC Name>\AppData\Local\Google\Chrome\User Data\Default\Login Data

(run SQLite commands to get information)
_URL_  , _username_ , _CipherText_

The ciphertext contains info about AES encryption
__Initialization Vector__  $  __EncryptedPasswords__
The initialization vector is stored between characters 4 to 20, while the encrypted password is stored between characters 21 to N-16 (i.e. total characters minus 16).
After AES, the encrypted key is stored in the Local State File, while the encrypted password is concatenated with the initialization vector and stored into the SQLite3 database as ciphertext.
AES is symmetric-key cryptography where it uses the same key for both encryption and decryption.We can use the encrypted key and initialization vector that we discovered to decrypt the passwords with AES cryptography.

#Step 1: Extracting initilisation vector from ciphertext
initialisation_vector = ciphertext[3:15]
#Step 2: Extracting encrypted password from ciphertext
encrypted_password = ciphertext[15:-16]
#Step 3:Build the AES algorithm to decrypt the password
cipher = AES.new(secret_key, AES.MODE_GCM, initialisation_vector)
decrypted_pass = cipher.decrypt(encrypted_password)
decrypted_pass = decrypted_pass.decode()
#Step 4: Decrypted Password
print(decrypted_pass)


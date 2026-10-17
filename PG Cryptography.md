---
tags:
- Other
MOC: Programming
Course: "https://www.boot.dev/courses/learn-cryptography-golang"
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]
# Cryptography Is About Encryption

Simply put, [Cryptography](https://www.boot.dev/blog/security/what-is-cryptography/) provides a method for secure communication. By using cryptography, we can send secret messages to others across an insecure medium (like the internet), and be sure that only the intended recipients will be able to read the message.

In this course, we'll be writing code in Go that makes up part of "Passly", a password manager. To start, let's write a function that will debug the logic that encrypts and decrypts passwords!

We encrypt passwords so that if an attacker gains access to the computer on which the passwords are stored, they still won't be able to _read_ the passwords.

# What Is Cryptography?

Cryptography is what makes online communication what it is today. Without the security and privacy it affords, we would never be able to:

- Transact financially online
- Verify sources of information
- Communicate privately
- Own private user accounts on websites

At its most basic, cryptography is about encrypting and decrypting data. Encryption transforms a key and input, called the "plaintext", into an encrypted output, called the "ciphertext". Only the person with the key can then decrypt the ciphertext back into the plaintext.

| Key                 | Plaintext        | Ciphertext      |
| ------------------- | ---------------- | --------------- |
| someSecretMasterKey | a secret message | Asdk345SAdsk2$# |

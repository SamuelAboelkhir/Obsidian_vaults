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
# Symmetric vs. Asymmetric

In the context of password managers, cryptography is used to secure data at _rest_. Typically, a "vault" of passwords is safely stored so that only authorized parties can gain access.

Protecting files at rest, or more specifically, protecting files for _oneself_ is usually accomplished via [symmetric encryption](https://en.wikipedia.org/wiki/Symmetric-key_algorithm), which means encryption and decryption use the _same_ key.

 ![[Pasted image 20261019113859.png]]

Asymmetric encryption uses _two_ keys, and we'll talk more about it later.

[Symmetric encryption](https://www.boot.dev/blog/security/aes-256-cipher/#symmetric-encryption-vs-asymmetric-encryption) should typically just be used to encrypt information for _oneself_. If you're encrypting data for someone else, you should use asymmetric encryption. There are a couple of reasons for this:

- You _probably_ don't have a way to securely transport the key to another person
- With asymmetric encryption, the recipient's private key never has to leave their machine. You only need their public key, which isn't secret

That said, symmetric encryption is great for what it's good at. Symmetric schemes are typically faster than asymmetric schemes, and they're easier to implement. If you encrypt your computer's hard-drive, it's probably done with symmetric encryption because it can be done quickly on large amounts of data.

We'll talk about asymmetric encryption in a later chapter.
# Better Keys

Keys are typically encoded in software as raw binary data. In Go, this means we usually use a slice of bytes as the data type. For example:

```go
[]byte{0xAA, 0xBB}
```

`0xAA` and `0xBB` are [hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal) values, and hex is one of the more common ways to write raw binary in code. They represent the raw binary data `10101010` and `10111011` respectively.
# Cryptology, Cryptography and Cryptanalysis

## Cryptology

> The scientific study of cryptography and cryptanalysis.  
> – [Merriam-Webster Dictionary](https://www.merriam-webster.com/dictionary/cryptology)

Cryptology is the science of secret messages. Therefore, anything that has to do with making or breaking codes falls into cryptology's domain.

## Cryptography

> Cryptography is the practice and study of techniques for secure communication in the presence of third parties called adversaries.

People often lazily use [Cryptography](https://en.wikipedia.org/wiki/Cryptography) in place of the word cryptology, but in reality, cryptography focuses only on creating secure cryptosystems. For example, the design of RSA would have been cryptography work.

## Cryptanalysis

> You use [Cryptanalysis](https://en.wikipedia.org/wiki/Cryptanalysis) to breach cryptographic security systems and gain access to the contents of encrypted messages, even if the cryptographic key is unknown.

Cryptanalysis is the inverse of cryptography. It's the study of how to _break_ secret codes, not _make_ them. Having a solid understanding of cryptanalysis is fundamental in cryptography, however, as one must know their enemy.

## Honorable Mention – Cryptocurrency

> A cryptocurrency is a digital asset designed to work as a medium of exchange that uses strong cryptography to secure financial transactions, control the creation of additional units, and also verify the transfer of assets.  
> – [Wikipedia](https://en.wikipedia.org/wiki/Cryptocurrency)

In the past few years, the slang term "crypto" has been hijacked. It used to mean cryptography or cryptanalysis but has recently come to be understood as cryptocurrency.
# Don't Trust the Client

The browser belongs to the _user_. Anything it sends to your server can be inspected, changed, omitted, or downright fabricated.

That includes values your own HTML generated! A [`hidden` input](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/hidden) is absent from the visible page, but it still appears in the document and any submitted requests:

```html
<input name="discountCents" type="hidden" value="0" />
```

An attacker can change `0` to `4497` before submitting the form or send a hand-crafted request without using the form at all. Hidden means _not displayed_; it does _not_ mean _trusted_.

## Nightmare Scenario

Client input becomes dangerous when the server uses it to make a security-sensitive decision. Imagine a checkout handler that lets the browser choose the discount and price _values_ applied to an order. Just by using `curl`, an attacker can make a `$44.98` cart cost just 1 cent!

![[Pasted image 20261105112631.png]]![checkout values vs ids](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/8Kxtz5p-1280x539.png)

The server should load product prices and valid discounts from trusted server-side data, then calculate the total _itself_. The client can identify products or a promo code, but the server needs to resolve those IDs to trusted values.
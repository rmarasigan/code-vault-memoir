## Hash Algorithm
- A mathematical function that takes in a variable-sized input, such as a file or message, and produces a fixed-size output, known as a hash or digest
- Are used in a variety of applications such as data integrity checks, message authentication, password storage, and digital signatures
- Designed to be one-way functions, meaning it is extremely difficult (if not impossible) to reconstruct the original input data from the hash value
- Common examples of hash algorithms include MD5, SHA-1, SHA-2, and SHA-3
    - MD5 and SHA-1 are considered insecure and should not be used for new applications

## Encryption
- It ensures that some information does not fall into the wrong hands

### Synchronous / Symmetric Encryption
- The message is encrypted by using a key and the same key is used to decrypt the message which it easy to use but **less secure**
- It only requires one key for both encryption and decryption
- The encryption process is faster and **more efficient for large amounts of data**
- Common algorithms include Advanced Encryption Standard (AES), Data Encryption Standard (DES), RC4, Blowfish, etc.
- It is used in file encryption, VPNs, and secure data storage

### Asynchronous / Asymmetric Encryption
- One of the most common cryptographic methods that involve using a single key and its pendent, where one key is used to encrypt data and the second one is used to decrypt an encrypted text
- The second key is kept highly secret, while the first one which is called a **public key** can be freely distributed among the service's users
- It requires two keys, a **public key** for encryption and a **private key** for decryption
- The encryption process is slow but it is **more secure than symmetric encryption**
- Common algorithms include RSA, Elliptic Curve Cryptography (ECC), DSA, Diffie-Hellman, etc.
- It is used in digital signatures, SSL/TLS, and secure email communication

## Network Communication
### Simplex Communication
- The information flows in only one direction, this means that the sender can transmit data to the receiver, but the receiver cannot send data back to the sender
- An example of simplex communication include TV broadcasting and most remote control devices

### Half-Duplex Communication
- The information can flow in both directions, but not at the same time
- When one party is transmitting data, the other party must wait for the transmission to end before sending their own data
- An example of half-duplex communication include walkie-talkies and CB radios

### Full-Duplex Communication
- The information can flow in both direction simultaneously, this means that both the sender and receiver can send and receive data at the same time
- An example of full-duplex communication include telephone conversations, video conferencing, and internet chat

## 🔖 Extras
- [Difference Between Symmetric and Asymmetric Key Encryption](https://www.geeksforgeeks.org/difference-between-symmetric-and-asymmetric-key-encryption/)
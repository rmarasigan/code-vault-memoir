- A value that represents the number of bits in a transmission message and is used by IT professionals to detect high-level errors within data transmissions
- The term _**checksum**_ is also sometimes called _**hash sum**_ or _**hash value**_
- Typically a long string of letters and numbers that **acts as a sort of fingerprint** for a file or set of files to indicate the number of bits included in the transmission
- Often used to verify data integrity but are not relied upon to verify data authenticity
- The _**checksum function**_ or _**checksum algorithm**_ is the procedure that generates the checksum

### Common Types of Checksum Algorithms
- **Secure Hash Algorithm (SHA) 0**
    - The first of its kind but was withdrawn shortly after its creation in 1993
- **SHA-1**
    - As of 2010, this hash function is no longer considered secure
- **SHA-2 (SHA-224, SHA-256, SHA-383, SHA-512)**
    - Relies on the size of the file and numbers to create a checksum value
    - Resulting checksums are vulnerable to length extension attacks, which involve a hacker reconstructing the internal state of a file by learning its hash digest
    - **Example (SHA-256)**: `a971e147ef8f411b4a2476bba1de26b9a9a84553c43a90204f662ca72ee93910`
- **Message Digest 5 (MD5)**
    - Creates a checksum value, but each file won’t necessarily have a unique number
    - Open to vulnerabilities if a hacker swaps out a file with the same checksum value
    - **Example**: `120EA8A25E5D487BF68B5F7096440019` → This is a test

## 🔖 Extras
- [checksum](https://www.techtarget.com/searchsecurity/definition/checksum)
- [Checksum](https://en.wikipedia.org/wiki/Checksum)
- [Error Detection Code – Checksum](https://www.geeksforgeeks.org/error-detection-code-checksum/)
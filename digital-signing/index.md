## Digital Signature

#### What is Digital Signature?
* It is a signature created by using PKI to verify authenticity of message and its sender.
* It usually is a encrypted hash of the message or the document to be signed.

#### Digital Signature Example
* Creating the hash of the file

```python

BLOCKSIZE = 1024
hasher = hashlib.sha512()
with open('file.txt', 'rb') as afile:
    buf = afile.read(BLOCKSIZE)
    while len(buf) > 0:
        hasher.update(buf)
        buf = afile.read(BLOCKSIZE)
print(hasher.hexdigest())
hash = int.from_bytes(hasher.digest(), byteorder='big')
```

* Encrypting the hash

```python
signature = pow(hash, d, n)

```
##### d is private key exponent and n is the modulus of the keys.

* Decrypting the signature

```python
hashFromSignature = pow(signature, e, n)
```

##### e is the public key exponent.

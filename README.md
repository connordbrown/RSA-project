# RSA-project

**The Prompt:**

Your boss has been long intrigued by the RSA algorithm and has asked you to create a tutorial RSA walk through and exploration project to help the boss and others in the company to understand the basic structures and mathematics of an RSA implementation.

This project will:
1. Implement the algorithm for breaking codes with a given basic structure. 
2. Explain the steps 
4. Implement a custom feature using previously written code.

## 1. Introduction to RSA Project

The Rivest-Shamir-Adleman algorithm, or RSA for short, is the most common way for encrypting and exchanging information securely. In this project, I will be building RSA from scratch, using number theory and Python programming. RSA relvolves around public and private keys. A user sends a public key to another user, that user encrypts their information with the public key, and sends it back to the original user, who decrypts the information with the private key. To get these keys, we need two different prime numbers p and q. Their product n as well as phi function (p - 1)(q - 1) are used in determining the keys. p and q are taken as inputs to FindPublicKey(). n and phi_n (the phi function) are calculated, and then an intial value for e, the public key, is determined using a random number generator. e must fulfill three conditions: be coprime to phi_n and not equal to either p or q. Then, a private key d is determined by using the Extended Euclidean Algorithm to get the modular inverse of e mod phi. e and d are used to convert the ASCII characters in a message into numbers that have no apparent relationship to the original ASCII values, then convert the unreadable number list back into a comprehensible message. In this project, I will be showing the Python implementation of each of these steps. The end result will be a program with a custom main function that users can use to read, encrypt and decrypt messages.

## 2. Code Demo

**Generating Keys**

First, pass two different prime numbers p and q to FindPublicKey(), and extract the return values n_val and e_val (the public keys):

```python
p = 29
q = 347
n_val, e_val = Find_Public_Key_e(p, q)
```

Next, pass e_val, p and q to FindPrivateKey(), and extract the return value d (the private key):

```python
d_val = Find_Private_Key_d(e_val, p, q)
```

*Note:* e must be relatively prime to phi ((p - 1)(q - 1)), and d must be a modular inverse of e mod phi. This is ensured by using the Euclidean Algorithm (Euclidean_Alg()) and the Extended Eucliean Algorithm (EEA()) helper functions inside of the public and private key functions. The standard Euclidean Algorithm checks that the GCD of e and phi is 1, proving they are relatively prime, and the Extended Euclidean Algorithm both checks coprimality AND finds a d such that it is a modular inverse of e mod phi. These conditions are critical to the encoding and decoding process that make RSA work. The public and private keys must be correctly paired, otherwise the encryption and decryption will not function correctly.

**Encoding Messages**

Now on to the fun part. Encode the characters using Encode(), making sure to pass in the public keys n_val and e_val determined earlier:

```python
message = "Hello, World!"
encoded_text_list = Encode(n_val, e_val, message)
```

*Note:* Both Encode() and Decode() use Fast Modular Exponentiation (FME) to calculate the mod of each item in the number sequence. RSA, at full capacity, uses huge numbers (~100 digits) that take a very long time to process. Python's built-in modulo operator (%) does not use FME, meaning it has much longer runtimes as the numbers get bigger. The custom FME() function I wrote earlier and use inside of Encode() and Decode() is lightning-fast and better suited to the large numbers involved. Here's my encoded message:

```python
print(encoded_text_list)
```
```bash
[2483, 1120, 271, 271, 2224, 7348, 2376, 29, 2224, 2109, 271, 1124, 8271]
```

**Decoding Messages**

Decode the number list using Decode(), making sure to pass in the n_val and private key d_val determined earlier, and see the result:

```python
decoded_text = Decode(n_val, d_val, encoded_text_list)
print(decoded_text)
```
```bash
Hello, World!
```
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
```text
[2483, 1120, 271, 271, 2224, 7348, 2376, 29, 2224, 2109, 271, 1124, 8271]
```

**Decoding Messages**

Decode the number list using Decode(), making sure to pass in the n_val and private key d_val determined earlier, and see the result:

```python
decoded_text = Decode(n_val, d_val, encoded_text_list)
print(decoded_text)
```
```text
Hello, World!
```

## 2. Project Narrative

My process for this project was a solid combination of techniques. A few weeks before starting it, I was new to RSA and the mathematical techniques and algorithms involved in its execution. For me, learning those first was my first priority. I am sometimes slow to assimilate new information, so I wasn't really sure what to plan ahead for. It's hard to plan for something that I have never done before. The Mastery Workbooks definitely helped - they gave me work to do on the project relevant to that week's curriculum and encouraged me to plan ahead as best I could. In other words, I mostly planned ahead, telling myself when I would be getting certain parts of the project done, and then "winging" the rest, because that's the nature of doing new things. I am methodical whenever possible and adaptable whenever necessary.

It was satisfying to get encryption and decryption working. I was a little unsure of whether or not my code would work, but I tried to remember that this project is designed to facilitate success as long as you are diligent in the coursework. I was initially having some trouble encrypting and decrypting messages. I would input a message, then encrypt/decrypt it, and end up with a string of nonsense characters. I think this was because I did not initially write a Python script to keep the order of operations correct. Once I did that, making sure I was passing the correct p, q, e, and d values into my functions, I had more success. Overall, the code exchange went well. I was immediately able to decrypt other students' posts using their private keys, and then writing responses using the public ones. I always made to sure to double check that my responses were decrypting into readable messages. The repeated testing and checking was what took the most time. Sometimes the decryption would not work, and I think that was related to a bug in my Extended Euclidean Algorithm that I fixed soon after.

The most challenging part of this project was having to learn large amounts of new theory, and nearly immediately put it into practice. It was difficult to plan out something so new and fast-paced. Even when looking ahead, I didn't always understand what I was going to be doing, so it sometimes felt like being blindfolded and led by the hand through an unknown place. I just tried to be diligent in my studies and "trust the process".

Overall, I think this project was well-designed. It used material exactly from the course, and followed the curriculum timeline. One helpful resource was the Python Docs; that was where I found the "secrets" module, which I used to generate public keys with high-quality randomness. Another was GeeksForGeeks, which had some useful tips on coding and modules.

My Best Mistake was a bug in my code that caused my decryption function to periodically not work and return strings of nonsense characters. I thought I had found the issue: sometimes my d value was not the correct modular inverse of e mod phi. I was up late one night trying to figure out why, but got totally stumped. After office hours, I tried to check that e and d were relatively prime, but I was still getting junk values. Sometimes the modular inverse was correct, but still ended up giving me junk decryptions. My next thought was that the problem was inside of my Extended Euclidean Algorithm function, because that was where I was doing something different with my d value.  

In order to make EEA work, I swapped the inputs so that a would always be greater than b in the equation a mod b. I made sure to swap the resulting s1 and t1 values to reflect this. I also made sure to add the initial value of a, a0 to s1 until it was positive, so that my decryption would work later. What I failed to notice was that depending on the order of the inputs, it was necessary to add either a0 OR b0 to s1. I noticed this when I printed out the linear combinations of EEA(1012, 27) and EEA(27, 1012). To fix the issue, I added a conditional to the loop for updating s1 to make it positive, ensuring that the correct amount (a0 or b0) was added. This solved the problem immediately. I was deeply frustrated by this problem for several days, so I was really happy to figure it out. Lesson learned: when swapping numbers, CHECK EVERYTHING CAREFULLY.

## 3. Code Breaking

Basic code breaking is done by using a brute force algorithm to factor n to get p and q values, then reverse engineering the private key d and using it to decrypt a message. The algorithm is as follows:

```python
def factorize(n):
    # n is a number, return the smallest factor of n
    for i in range(2, n):
        if n % i == 0:
            return i
    return False
```

**How Code Breaking Works**

Code breaking is pretty straightforward. First, get the public key and encrypted_message given by the writer of the code:

```python
n, e = (781, 9021)   # public key
encrypted_message = [72, 321, 174, 174, 144, 539, 329, 604, 144, 444, 174, 221, 209]
```

Next, factor the n value to get the p and q values using the factorize() function and arithmetic:

```python
p = factorize(n)
q = n // p
```

Using these p and q values, as well as the provided public key e, determine the private key using Find_Private_Key_d():

```python
d = Find_Private_Key_d(e, p, q)
```

Lastly, using p, q, and the calculated private key d, decode the encrypted message using Decode():

```python
decoded_text = Decode(n, d, encrypted_message)
print(decoded_text)
```
```text
Hello, World!
```

After brute forcing an n value with 17 digits, I noticed that it takes a REALLY long time to brute force larger values of n, especially after n exceeds 14 digits. The p and q values are calculated fairly quickly, but the decryption is quite slow. If large values of n are used, RSA is very secure, because it takes a VERY LONG TIME to process the large numbers involved, if they can be processed at all. However, my implementation is not very secure, because it uses relatively small numbers, which can be cracked easily.

## 4. Custom Feature

The following block is an example of using my custom main() function:

```text
==========================
       RSA PROJECT        
   Discrete Structures        
       Connor Brown       
     [ID: XXXXXXXXX]      
==========================

Options:
1. Get Keys
2. Encode a Message
3. Decode a Message
4. Exit
Make a selection:  1

Enter first prime number 'p':  11
Enter second prime number 'q':  331
Your public keys (n, e) are: ((3641, 931))
Your private key 'd' is: 1471

Press ENTER to continue 

==========================
       RSA PROJECT        
   Discrete Structures        
       Connor Brown       
     [ID: XXXXXXXXX]      
==========================

Options:
1. Get Keys
2. Encode a Message
3. Decode a Message
4. Exit
Make a selection: 2

Enter public key 'n':  3641
Enter public key 'e':  931
Enter a message to encrypt, then press ENTER:  Hello, World!

Ciphertext: [2756, 1630, 3221, 3221, 936, 957, 32, 1330, 936, 422, 3221, 56, 2387]

Press ENTER to continue 

==========================
       RSA PROJECT        
   Discrete Structures         
       Connor Brown       
     [ID: XXXXXXXXX]      
==========================

Options:
1. Get Keys
2. Encode a Message
3. Decode a Message
4. Exit
Make a selection: 3

Enter public key 'n':  3641
Enter private key'd':  1471
Enter ciphertext as a list (e.g. [23, 14]):  [2756, 1630, 3221, 3221, 936, 957, 32, 1330, 936, 422, 3221, 56, 2387]

The decoded message is:
Hello, World!

Press ENTER to continue 

==========================
       RSA PROJECT        
   Discrete Structures        
       Connor Brown       
     [ID: XXXXXXXXX]      
==========================

Options:
1. Get Keys
2. Encode a Message
3. Decode a Message
4. Exit
Make a selection: 4

Goodbye.
```

## Author

The application was built by Connor D. Brown in 2024.
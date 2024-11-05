<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$'], ['\\(','\\)']],
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'] // removed 'code' entry
    }
});
MathJax.Hub.Queue(function() {
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-AMS_HTML-full"></script>


# Crypto

## Defs
+ **Plain text**: Original message before it is encoded or enciphered
+ **Cipher text**: Results from applying an encryption technique to the plaintext
+ **Key**: String used for encryption and/or decryption

---

+ **Private Key** Method (a.k.a. **Symmetric** or **Secret** Key): Uses one private key by sender and receiver
+ **Public Key** Method (a.k.a. **Asymmetric**): Uses two paired keys for each person


## Private Key Cryptography

### a. Julius Caesar (shift) cipher

Example of **Private Key**, **Symmetric** or **Secret**

```
Plaintext: Yellow cake
Key: 3
---
Ciphertext: Bhoorz fdnh
```

### b. Mono-alphabetic cipher

**ROT13**: shifts by 13 (mod 26)

| Plaintext | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ROT13     | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M |

```
Plaintext: BARFY
Key: ROT13
---
Ciphertext: ONESL
```

### c. Poly-alphabetic cipher

**Vigenère cipher**

|       | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
|-------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **A** | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
| **B** | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A |
| **C** | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B |
| **D** | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C |
| **E** | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D |

- The column headers represent the letters of the plaintext.
- The row headers represent the letters of the keyword.

```
Plaintext: ATTACKATSIXPM
Key: GOPHER
---
Ciphertext: GHIHFKSXQWBVW
```


### Enigma machine

- Developed in the 1920s for secure communication, especially by German military forces in WWII.

- **Rotors**: Used 3 to 5 rotors with internal wiring to encode messages, with each rotor shifting after every key press to create a new encryption for each letter.

- **Reflector**: Bounced the signal back through the rotors, creating a second layer of encryption by reversing the letter's path.

- **Plugboard**: Swapped certain letters before the final output, adding another layer of security.

- **Dynamic Encoding**: Unlike other ciphers, pressing the same key multiple times could yield different encoded letters due to rotor rotation.

- **Codebreaking**: Initially cracked by Polish cryptanalysts in 1932; later, British cryptographers, including Alan Turing at Bletchley Park, broke updated versions using early computers.

- **Impact on WWII**: Breaking Enigma codes gave the Allies critical insight into German operations, significantly influencing the war's outcome.


### One-Time Pad

Example of **Private Key**, **Symmetric** or **Secret**

- The one-time pad consists of pages filled with random letters or numbers.

```
Plaintext: BARFY
Key: NOWISTHETIME
---

B (1) + N (13)  → 14 % 26 = 14 (O)
A (0) + O (14)  → 14 % 26 = (O)
R (17) + W (22) → 39 % 26 = 13 (N)
F (5) + I (8)   → 13 % 26 = 13 (N)
Y (24) + S (18) → 42 % 26 = 16 (Q)

Ciphertext: OONNQ
```


## Brute Force Attack

- Brute Force Attack: Exhaustively try every possible key to discover the plain text
- **Use extremely large keys** to avoid brute force attack
    - A 256 bit key has $2^{256}$ possible keys


## Block Cipher Chaining

More resistent to certain types of attacks

### Block Cipher Encode

- Choose an **initialization vector** at random
- Choose a **key**

```
    c=3     a=1     k=11    e=5
    00011   00001   01011   00101       - binary plaintext
⊕   01001                               - initialization vector (XOR)
+   00001                               - key
---------------------------------
    01011                               - c encrypted
            00001                       - a
    ⊕       01011                       - previous block (chaining)
    +       00001                       - key
---------------------------------
            01011                       - a encrypted
                    01011               - k
            ⊕       01011               - previous block (chaining)
            +       00001               - key
---------------------------------
                    00001               - k encrypted
                            00001       - e
                    ⊕       00101       - previous block (chaining)
                    +       00001       - key
---------------------------------
                            00101       - e encrypted


    01011   01011   00001   00101       - encrypted ciphertext
```

### Block Cipher Decode

- Reverse the operations
    1. Minus
    2. XOR


## Public Key Cryptography

### Role 1: Encryption / Decryption
1. **A sends to B**: $$ e = E_{\text{Bob's Public Key}}(m) $$
2. **B decrypts**: $$ m = D_{\text{Bob's Private Key}}(e) $$

3. **B sends to A**: $$ e = E_{\text{Alice's Public Key}}(m) $$
4. **A decrypts**: $$ m = D_{\text{Alice's Private Key}}(e) $$

### RSA

- Example of **Public Key** Method
- Based on the difficulty of factoring the product of two large primes

#### RSA Math

1. Choose two large prime numbers $p$ and $q$.
    - Known $n$, difficult to factor into its two unique factors

2. Compute $n = p \times q$ where $p$ and $q$ are two unique prime factors.

3. Choose $e$ such that $e$ and $(p-1) \times (q-1)$ are relatively prime (only common factor is 1).

4. Find an integer $d$ such that $$e \times d \equiv 1 \mod ((p-1) \times (q-1))$$
   - Equivalent to solving $e \times d - 1 = k \times ((p-1) \times (q-1))$ for $d$, where $k$ is some integer.

5. Publish the public key $(e, n)$; keep the private key $(d, n)$ secret.

6. **Encode** message $m$ by computing $c = m^e \mod n$.

7. **Decode** ciphertext $c$ by computing $m = c^d \mod n$.

#### RSA Example
> Use RSA with the prime numbers $p = 3$, $q = 5$ to encode $CAM = 3, 1, 13$ letter-by-letter.

1. Choose primes $p = 3$ and $q = 5$.

2. Calculate $n = p \times q = 15$ and $\phi(n) = (p-1) \times (q-1) = 8$.

3. Select $e = 3$, which is relatively prime to $\phi(n)$.

4. Find $d = 3$ such that $e \times d \equiv 1 \mod \phi(n)$.

5. Public key is $(e, n) = (3, 15)$; private key is $(d, n) = (3, 15)$.

6. Encode each letter in "CAM" as:
    - **$C = 3$:** Calculate $3^3 \mod 15 = 27 \mod 15 = 12$ (so, $C \rightarrow 12$).
    - **$A = 1$:** Calculate $1^3 \mod 15 = 1 \mod 15 = 1$ (so, $A \rightarrow 1$).
    - **$M = 13$:** Calculate $13^3 \mod 15 = 2197 \mod 15 = 7$ (so, $M \rightarrow 7$).

7. Final encoded message: **$C = 12$, $A = 1$, $M = 7$** or **12, 1, 7**.

#### RSA in Java

```java
/* Demonstrate RSA in Java using BigIntegers */
package rsaexample;

import java.math.BigInteger;
import java.util.Random;

/**
 *  RSA Algorithm from CLR
 *
 * 1. Select at random two large prime numbers p and q.
 * 2. Compute n by the equation n = p * q.
 * 3. Compute phi(n)=  (p - 1) * ( q - 1)
 * 4. Select a small odd integer e that is relatively prime to phi(n).
 * 5. Compute d as the multiplicative inverse of e modulo phi(n). A theorem in
 *    number theory asserts that d exists and is uniquely defined.
 * 6. Publish the pair P = (e,n) as the RSA public key.
 * 7. Keep secret the pair S = (d,n) as the RSA secret key.
 * 8. To encrypt a message M compute C = M^e (mod n)
 * 9. To decrypt a message C compute M = C^d (mod n)
 */

public class RSAExample {

    public static void main(String[] args) {
        // Each public and private key consists of an exponent and a modulus
        BigInteger n; // n is the modulus for both the private and public keys
        BigInteger e; // e is the exponent of the public key
        BigInteger d; // d is the exponent of the private key

        Random rnd = new Random();

        // Step 1: Generate two large random primes.
        // We use 400 bits here, but best practice for security is 2048 bits.
        // Change 400 to 2048, recompile, and run the program again and you will
        // notice it takes much longer to do the math with that many bits.
        BigInteger p = new BigInteger(400,100,rnd);
        BigInteger q = new BigInteger(400,100,rnd);

        // Step 2: Compute n by the equation n = p * q.
        n = p.multiply(q);

        // Step 3: Compute phi(n) = (p-1) * (q-1)
        BigInteger phi = (p.subtract(BigInteger.ONE)).multiply(q.subtract(BigInteger.ONE));

        // Step 4: Select a small odd integer e that is relatively prime to phi(n).
        // By convention the prime 65537 is used as the public exponent.
        e = new BigInteger ("65537");

        // Step 5: Compute d as the multiplicative inverse of e modulo phi(n).
        d = e.modInverse(phi);

        System.out.println(" e = " + e);  // Step 6: (e,n) is the RSA public key
        System.out.println(" d = " + d);  // Step 7: (d,n) is the RSA private key
        System.out.println(" n = " + n);  // Modulus for both keys

        // Encode a simple message. For example the letter 'A' in UTF-8 is 65
        BigInteger m = new BigInteger("65");

        // Step 8: To encrypt a message M compute C = M^e (mod n)
        BigInteger c = m.modPow(e, n);

        // Step 9: To decrypt a message C compute M = C^d (mod n)
        BigInteger clear = c.modPow(d, n);
        System.out.println("Cypher text = " + c);
        System.out.println("Clear text = " + clear); // Should be "65"

        // Step 8 (reprise) Encrypt the string 'RSA is way cool.'
        String s = "RSA is way cool.";
        m = new BigInteger(s.getBytes()); // m is the original clear text
        c = m.modPow(e, n);     // Do the encryption, c is the cypher text

        // Step 9 (reprise) Decrypt...
        clear = c.modPow(d, n); // Decrypt, clear is the resulting clear text
        String clearStr = new String(clear.toByteArray());  // Decode to a string

        System.out.println("Cypher text = " + c);
        System.out.println("Clear text = " + clearStr);
    }
}
```


### Role 2: Digital Signatures: Signing / Verification

- **Private key**: Used to *sign* a document for others to verify
- **Public key**: Used to *verify* that a signed document was signed by you


#### Hash Functions

- SHA
    - Designed by the National Security Administration (NSA)

- SHA-1
    - 160 bit digest
        - In Feb 2017, a deliberate collision was demonstrated*
    - Breaking property #2 of "infeasible to create a new message that has a given hash value"

- SHA-2
    - A family of related hash functions
    - SHA-256 (for Project 1) has a 256 bit digest

---

**Scenario 1: No Bad Guy**

```
Send: ("Hi Jeri")
Receive: ("Hi Jeri")
Result: Message is received unchanged.
```

---

**Scenario 2: Bad Guy Intercepts and Changes Message**

```
Send: ("Hi Jeri")
Bad Guy Changes: ("Hi Jerk")
Receive: ("Hi Jerk")
Result: Jeri thinks the message is "Hi Jerk" without realizing it was changed.
```

---

**Scenario 3: Adding a Hash (No Bad Guy)**

```
Send: ("Hi Jeri", 25)
Receive: ("Hi Jeri", 25)
Compute: hash("Hi Jeri") = 25
Result: Hash matches; Jeri is confident the message is unchanged.
```

---

**Scenario 4: Bad Guy Intercepts and Changes Message and Hash**

```
Send: ("Hi Jeri", 25)
Bad Guy Changes: ("Hi Jerk", 32)
Receive: ("Hi Jerk", 32)
Compute: hash("Hi Jerk") = 32
Result: Hash matches, but Jeri unknowingly receives a tampered message.
```

---

**Scenario 5: Digital Signature Added (Problem Solved)**

```
Send: ("Hi Jeri", "05A3")  // "05A3" is Isaac's digital signature for hash 25 using Isaac's private 
Bad Guy Attempts to Change: ("Hi Jerk", "05A3")
Receive: ("Hi Jerk", "05A3")
Compute: hash("Hi Jerk") = 32, D_Isaac_public("05A3") = 25
Result: Hash mismatch detected; Jeri knows the message was altered.
```

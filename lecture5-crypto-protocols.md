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



# Crypto Protocols


## Defs

- A **protocol** defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.

- A **cryptographic protocol** is a protocol that employs cryptography.


## Attacks

- Eavesdropping
- Masquerading
- Tampering by a person in the middle
- Replaying of old messages
- Denial of service



## Needham-Schroeder Symmetric Key Protocol

> A cryptographic protocol that enables secure communication between two parties, Alice and Bob, using a trusted third party (Sarah).

### Message Authentication Code (MAC)

<img src="./res/mac.png" alt="" width="300">


### Procedure

| Header | Message                                 | Description                                                                                      |
|--------|-----------------------------------------|--------------------------------------------------------------------------------------------------|
| 1. A->S | $A, B, N_A$                            | A requests S to supply a key for communication with B.                                          |
| 2. S->A | $\{N_A, B, K_{AB}\}_{K_A}, \{K_{AB}, A\}_{K_B}$ | S sends a session key $K_{AB}$ encrypted for A, along with a "ticket" for B. $N_A$ confirms freshness. |
| 3. A->B | $\{K_{AB}, A\}_{K_B}$                  | A forwards the "ticket" to B.                                                                    |
| 4. B->A | $\{N_B\}_{K_{AB}}$                     | B decrypts the ticket, uses $K_{AB}$ to send a new nonce $N_B$ to A.                             |
| 5. A->B | $\{N_B - 1\}_{K_{AB}}$                 | A returns $N_B - 1$, proving knowledge of $K_{AB}$ and confirming communication.                 |









## Transport Layer Security (TLS)

Without TLS

```
HTTP Client         HTTP Server
TCP                 TCP
IP                  IP
Physical            Physical
```

With TLS

```
HTTP Client         HTTP Server
**TLS**             **TLS**
TCP                 TCP
IP                  IP
Physical            Physical
```





## Diffie-Hellman-Merkle Key Exchange

> A cryptographic protocol used to generate a shared secret, which can then be used as a **symmetric key** for secure communication between two parties.

- Commonly used in the **TLS handshake** to securely establish a symmetric key and provide forward secrecy.

### Procedure

<img src="./res/DiffieHellman.png" alt="" width="500">

- Added the Diffie-Hellman parameters:
    - Modulus `p = 23` and base `g = 5`.

    - Client (Alice) and Server (Bob) choose random secret integers a and b between 2 and 22.
    
    - Computed values `A = g^a mod p` and `B = g^b mod p` are exchanged between Alice and Bob.

- Both parties compute the shared secret:
    - Alice computes `B^a mod p`.
    - Bob computes `A^b mod p`.

- Two parties arrive at a common secret key, without passing the common secret key across the public channel.


### Mathematical Proof

The shared secret $s$ is the same for both Alice and Bob due to the mathematical properties of modular exponentiation:

- Alice computes:
  $$
  s = B^a \mod p = (g^b)^a \mod p = g^{ab} \mod p
  $$
  
- Bob computes:
  $$
  s = A^b \mod p = (g^a)^b \mod p = g^{ab} \mod p
  $$

The shared secret can then be used as a symmetric key for encryption, ensuring that only Alice and Bob (who performed the key exchange) can securely communicate. Even if a third party observes $A$ and $B$, they cannot easily compute $g^{ab} \mod p$ because doing so would require solving the **Discrete Logarithm Problem**, which is computationally difficult for large numbers.




## Worksheets


1. The symmetric key between Bob and Alice is denoted as $$ K_{AB} $$

2. When Bob sends "Hello Alice" to Alice using the symmetric key, it is written as:
   $$
   A \rightarrow B : \{ \text{Hello Alice} \}_{K_{AB}}
   $$

3. When Alice sends "Hello Bob" to Bob:

   $$
   A \rightarrow B : \{ \text{Hello Bob} \}_{K_{B\text{-pub}}}
   $$

4. When Bob sends "Hello Alice" to Alice:

   $$
   B \rightarrow A : \{ \text{Hello Alice} \}_{K_{A\text{-pub}}}
   $$

5. When Bob wants to send a shared key $K_{AB}$ to Alice:

   $$
   B \rightarrow A : \{ K_{AB} \}_{K_{A\text{-pub}}}
   $$

6. For a symmetric encryption function: $$ D(K_{AB}, E(K_{AB}, m)) = m $$

7. For an asymmetric encryption function: $$ D(K_{B\text{-priv}}, E(K_{B\text{-pub}}, m)) = m $$


### Digital Signature

> Suppose `digest(m)` is a SHA-256 hash of the message m.

8. When Alice sends a message **m** and **signature** to Bob:

   $$
   m, \; E(K_{A\text{-priv}}, \text{digest}(m))
   $$

9. **Verifying a Digital Signature**:

   - Bob computes $$ \text{digest}(m) $$

   - Bob decrypts Alice's signature with her public key: $$ D(K_{A\text{-pub}}, E(K_{A\text{-priv}}, \text{digest}(m))) $$

   - Bob verifies if $$ \text{digest}(m) == D(K_{A\text{-pub}}, E(K_{A\text{-priv}}, \text{digest}(m))) $$ to confirm the signature.

10. **Factoring Assumption**: Factorization is easy with small numbers like $91$, which factors to $7$ and $13$.
    - Factoring becomes prohibitively difficult as numbers get larger, which is why RSA requires very large primes for security.

11. **Discrete Log Assumption**: Solving $3^x \mod 7 = 6$ is easy for small numbers; For example, $x = 3$.
    - Becomes significantly harder as numbers get larger, which is why it's widely used in cryptographic systems like Diffie-Hellman and Elliptic Curve Cryptography (ECC).

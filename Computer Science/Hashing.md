# Hashing

## Table of Contents
- [What is Hashing?](#what-is-hashing)
- [Uses of Hashing](#uses-of-hashing)

## What is Hashing?
> Hashing is the process of transforming any given key or a string of characters into another value. This is usually represented by a shorter, fixed-length value or key that represents and makes it easier to find or employ the original string. | TechTarget
### Characteristics of Hashing
- Avalanche Effect
  - A slight change in the input will result to a significant change in the output.
- Deterministic
  - An input must always produce the same hash.

## Uses of Hashing
- Hashing is used in implementing various data indexing and retrieval in data structures such as hashtables and hashset.
- Some other uses include digital signatures, cybersecurity, and cryptography.
### Cybersecurity
#### Dealing with Passwords in General
- Storing passswords in cleartext format is very insecure and prone to risk.
- Also, users may use the same or a variation of the password for other accounts, which may lead to the compromise of those accounts.
- Therefore to mitigate security breaches, it can be crucial to hash and salt passwords.
- *The point is to make the attackers’ gains less than the effort to breach in, to not make the attack worth it.*
#### Hashing Passwords
> Hashing performs a one-way transformation of the password, encrypting it in a way that makes it extremely difficult to reconstruct the original password. | Oracle
- Once a password is hashed, it cannot be "unhashed".
- We should not be able to find a *pre-image* by looking at the hash.
  - Pre-image: a value that produces a specific hash when used as input to a hash function (the plaintext value).
- *The transmission of the password should be encrypted. However, the password hash does not need to be encrypted at rest.*
- *Use a salt to cover the shortcomings of hash functions.*
#### Process of Storing a Password
- Take the plaintext that the user used to sign up and hash it.
- Store the username and hash pair in the DB.
- When the user logs in, hash the password that the user entered and compare it with the hash that is stored.
- If the two hashes match, it is a valid login.
- The plaintext password is never stored in the process. Hash it, then forget it.
#### Commonly Used Hashing Algorithms
- Message Digest (MDx) algorithms.
  - Ex: MD5.
- Secure Hash Algorithms (SHA).
  - Ex: SHA-1 and SHA-2 (SHA-256) family.
  - Transform a random-size input into a fixed-size bit string.
- *Use third-party libraries.*
#### Types of Attacks
##### Brute Force Attack
- Attackers input random passwords into the hash function until a matching hash is found.
- Hashes shared by multiple users may indicate a commonly used password, narrowing down the number of passwords needed for a brute force attack.
###### Counter Measure
  - Hashing passwords need to be slow to make brute-force attacks less feasible.
    - Ex: use a lot of internal iterations, make the hash calculation more memory intensive.
##### Rainbow Table Attack
- Attackers use a large database of precomputed hash chains to find the input of the stolen password hashes.
  - Hash Chain: a row in a rainbow table, which is stored as an initial hash value and a final value obtained after multiple operations on that initial value.
- Rainbow tables are databases of a hash version of a password (stolen hashed passwords) to the plaintext password (precomputed hash).
###### Counter Measure
  - Rainbow table attacks can be mitigated by using salts.
    - Hash the combination of the password and a secret salt.
  - Since a rainbow table is made up of unsalted hashes, adding a unique and unguessable salt value ot it will make the rainbow table useless.
#### Client-side Hashing
- Hash the plaintext password before sending it to the server.
- If the hacker is able to hack the password, they will only read the hashed value. Although it doesn't stop the hacker from logging in to the user's account, it prevents them from obtaining the potentially reused plaintext password.

## Reference
[What is hashing and how does it work? - TechTarget](https://searchsqlserver.techtarget.com/definition/hashing#:~:text=Hashing%20is%20the%20process%20of,the%20implementation%20of%20hash%20tables.)  
[Oracle Commerce Platform - Password Hashing | Oracle](https://docs.oracle.com/cd/F25148_01/Platform.11-3-2/ATGPersProgGuide/html/s0506passwordhashing01.html)  
[How to Hash Passwords: One-Way Road to Enhanced Security - auth0](https://auth0.com/blog/hashing-passwords-one-way-road-to-security/)  

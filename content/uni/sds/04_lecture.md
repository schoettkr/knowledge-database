+++
title = "Security of Distributed Software - Lecture 04"
author = ["eo shiru"]
date = 2019-04-23
lastmod = 2019-05-17T14:36:11+02:00
tags = ["uni", "security-ds"]
draft = false
+++

Not so much related to rest of lecture:


#### OWASP {#owasp}

The Open Web Application Security Project is a worldwide not-for-profit charitable organization focusing on improving the security of software, which issues software tools and knowledge-based documentation on application security


## Security Mechanisms for Distributed Software {#security-mechanisms-for-distributed-software}


### Cryptography {#cryptography}

Cryptography is a broad field, which is only briefly touched in this lecture. The methods we'll use in this lecure are:

-   one key (symmetric algorithms)
    -   {{< figure src="/knowledge-database/images/sym-methods.png" >}}
    -   both participants use the same key (for de- and encryption)
    -   the key therefore has to be transmitted aswell (risk)
-   two keys (asymmetric algorithms)
    -   {{< figure src="/knowledge-database/images/asym-methods.png" >}}
    -   a public key is used to encrypt a message which can only be decrypted with the according private key &rarr; private key is not submitted (thus more secure)
-   hybrid methods
    -   {{< figure src="/knowledge-database/images/hybrid-methods.png" >}}
    -   session key is encrypted with public key and transmitted and then gets decrypted with private key
    -   session key is used to encrypt data/message and now the receiver can decrypt it with the earlier decrypted session key
-   one-way hash functions
    -   **compression**
        -   inputs of arbitrary length are mapped to outputs with fixed length
    -   **irreversibility** (surjective function)
        -   input can not be inferred from the output
    -   **collision-resistant**
        -   a hash function \\(h()\\) is called collision resistant - if it is hard to find to find two inputs \\(a\\) and \\(b\\) such that \\(h(a)=h(b)\\) and \\(a \neq b\\)

[Public key cryptography visualized](https://www.youtube.com/watch?v=YEBfamv-%5Fdo&feature=youtu.be)

Challenge-Response with Public Key:

1.  Client sends identifier ID to server
2.  Server sends generated random number R
3.  Client signs R with a private key & sends the result
4.  Server verifies the result using the public key of the client


### Public Key Infrastructure (PKI) {#public-key-infrastructure--pki}

-   Challenge: management of public keys
-   binding the key to its owner
    -   certificate = digital certificate of public key assignment to a (legal) person (eg X.509 Certificate)
    -   certification authority (CA) = provides certificate issugin services; the certificates are usually signed with the private key of the CA
        -   reduces the problem of authentic key distribution to distribution of authentic keys of CAs
    -   service users must identify themselves to the CA

CA services require the use of a computer which is suitably protected against improper use. In particular, it is recommended to use a computer without any network connection to protect it physically.<br />
The secret keys of the CA must be adequately protected and may not be given to third parties. The responsibility lies with the administrators of the CA, who are, therefore, advised to use external peripheral devices (eg smart card, floppy disk).<br />
The secret signature key of the CA must only be used to sign CA- or Enduser keys or revocation lists (CRLs) or to create cross-signed certificates.<br />
Each CA must generate its asymmetric key pairs by themselves. Asymmetric key pairs of the CA for signature generation must have a minimum length of 2048 bits RSA (or equivalent). In case CA generates asymmetric key pairs fo the end user, CA has to perform it on a dedicated CA computer.<br />
All data obtained during certification must be treaded as confidential by the CA staff. CA legal data protection regulations are to be complied with.<br />

In summary the operation of a Certificate Authority is mainly influenced by the _technical requirements_, the _legal requirements_ and the _organizational requirements_.

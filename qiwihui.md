# qiwihui

Hi, I am qiwihui, a python dev. I am interested in Ethereum protocol. Looking forward to learning from you guys.

- Twitter/X: https://twitter.com/qiwihuix
- Telegram: https://t.me/qiwihui

## Notes

### 2024.4.4

Join the group.

### 2024.4.5

1. Cryptography

    - Hashing
        - A hash function is a versatile one-way cryptographic algorithm that maps an input of any size to a unique output of a fixed length of bits.
        - What hash function can do:
            - Ensure data integrity,
            - Secure against unauthorized modifications,
            - Protect stored passwords, and
            - Operate at different speeds to suit different purposes.
        - example: Secure Hash Algorithm (SHA), Message Digest (MD)
    - Public key Cryptography
        - asymmetric encryption scheme
        - encryption: It allows anyone to encrypt a message that can only be decrypted by the corresponding private key.
        - signing: A digital signature is created by encrypting a hash of the message using the sender’s private key. The recipient can then verify the authenticity of the message by decrypting the digital signature using the sender’s public key and comparing it to the calculated hash of the received message.

2. Merkle tree in Bitcoin

    - how to form merkle tree
      1. Perform hashing to each transaction
      2. put transaction hashes into pairs and perform hash to the pair
      3. pair the hashes again and hash them again until all the hashes meet at a sing hash
      4. the sing hash is call Metkle root and it is the root of the merkle tree
      5. merkle root will be put into the block header
    - make transactions tamper proof, verification donot need whole transactions data

3. p2p network
    - client-server netowrk, central point of storage
    - blockchain p2p network: no central point and no controlling parties, echo nodes/peers keep a copy of the data
    - far less vulnerable being hacked, expoited or lost

4. BitTorrent
    - BitTorrent is designed for a large group of machines to exchange the same file quickly and reliably.
    - Rather than optimizing a single path, BitTorrent tries to take advantage of all available network bandwidth by dividing the transfers needed to retrieve a single large file between multiple cooperating peers. This allows it to frequently outperform the typical model in which clients download only from a central server.

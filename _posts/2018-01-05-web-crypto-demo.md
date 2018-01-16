---
layout: post
title: Unextractable client side keys with the Web Crypto API
excerpt_separator:  <!--more-->
---

{:class="message"}
  Don't like words? Symmetric encryption demo [available here](https://hectorgrebbell.github.io/WebCryptoExample/) (Newer browsers only), or [clone the source](https://github.com/hectorgrebbell/WebCryptoExample).

The [WebCrypto API](https://www.w3.org/TR/WebCryptoAPI/) allows keys to be created as unextractable. This means once created key objects can be used to encrypt, decrypt and sign/verify signatures but the raw private key primatives cannot be extracted.

Combined with [IndexedDB](https://www.w3.org/TR/IndexedDB-2/) key objects can be stored client side for reuse in later sessions.

This has several advantages. For example a client that audits served JavaScript before generating/loading keys can then be certain that whilst code served from the same origin can continue to use the keys this can only occur on authorised machines. Any documents symmetrically encrypted using those keys cannot be decrypted elsewhere. Any signatures generated using that private key came from that machine.

There are of course caveats. For example it is easy to assume that were a hosting service to offer client-side encryption/decryption using this approach that they had no ability to read users files. In reality it would be trivial for the provider to serve JavaScript to a user to download the encrypted file, decrypt it on the clients machine in the background then reupload.

For use cases where encrypt is a common operation and decrypt rare - backup being one - the keys can be marked for encrypt only. When the user wishes to decrypt they would be required to enter a key-derivation passphrase. A less than trusting user could then audit served code every time decryption is required.

Another major benefit is reducing server load by offloading crypto work to the client.

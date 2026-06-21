# Encryptor

A browser-based AES-256 encryption/decryption tool with password-based key 
derivation. Everything runs client-side — no server, no data leaves your 
browser.

## How it works

- You choose a password and enter text to encrypt or a previously encrypted 
  blob to decrypt
- **Key derivation**: your password is run through PBKDF2 (250,000 
  iterations, SHA-256) along with a random 16-byte salt to derive a 256-bit 
  AES key — this makes brute-forcing the password computationally expensive
- **Encryption**: the text is encrypted with AES-GCM using a random 12-byte 
  IV (initialization vector), which ensures that encrypting the same text 
  twice produces different output
- **Output packing**: the salt, IV, and ciphertext are concatenated and 
  encoded as a single base64 string — this is the blob you copy, share, or 
  store
- **Decryption**: paste the base64 blob back in, enter the same password, 
  and the salt/IV are extracted from the blob to re-derive the key and 
  recover the original text

All cryptographic operations use the browser's built-in **Web Crypto API** 
(`crypto.subtle`) — no custom or third-party crypto implementation.

## Features

- Toggle between Encrypt / Decrypt modes
- Self-contained output: the salt and IV travel with the ciphertext, so 
  you only need to remember the password — nothing else to store separately
- Clear error handling (e.g. wrong password or corrupted input is reported 
  instead of silently failing)
- One-click copy to clipboard
- Clean, modern dark UI

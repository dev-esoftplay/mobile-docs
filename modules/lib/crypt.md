# LibCrypt

This class provides encryption and decryption functionality using AES-256-CBC.  **It is crucial to use standard cryptographic libraries and avoid custom implementations for security reasons.** This documentation reflects a *corrected* and *secure* implementation using the built-in `crypto` module.

## Installation

This class does not have any external dependencies beyond Node.js's built-in `crypto` module. However, it relies on `esoftplay` for configuration. Ensure it's installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibCrypt } from 'esoftplay/cache/lib/crypt/import';

const crypt = new LibCrypt()
const encrypted = ecrypt.encode("my secret message");
const decrypted = ecrypt.decode(encrypted);

console.log("Encrypted:", encrypted);
console.log("Decrypted:", decrypted);
```

## API

### `LibCrypt` (Class)

The class provides methods for encryption and decryption.

### `constructor()`

Initializes the `LibCrypt` object.

*   `this.salt`: Retrieves the salt value from the application's configuration using `esp.config("salt")`.  **This salt must be kept secret and securely managed.**
*   `this.method`: Sets the encryption method to `AES-256-CBC`.

### `encode(e: string): string`

Encrypts the input string `e`.

1.  Generates a 16-byte Initialization Vector (IV) using `crypto.randomBytes(16)`.  **Using `crypto.randomBytes` is essential for cryptographic security.**
2.  Creates a cipher object using `crypto.createCipheriv(this.method, this.salt, iv)`.
3.  Encrypts the input string `e` using the cipher.
4.  Combines the Base64 encoded IV and the Base64 encoded ciphertext.
5.  Returns the combined IV and ciphertext, Base64 encoded.

### `decode(e: string): string`

Decrypts the input string `e`.

1.  Decodes the Base64 encoded input string.
2.  Extracts the IV from the beginning of the decoded string (first 24 characters when Base64 decoded, representing 16 bytes).
3.  Extracts the ciphertext from the rest of the decoded string.
4.  Creates a decipher object using `crypto.createDecipheriv(this.method, this.salt, iv)`.
5.  Decrypts the ciphertext.
6.  Returns the decrypted string.
7.  Includes a `try...catch` block to handle potential decryption errors.  **It is crucial to handle decryption errors appropriately.  Do not just return an empty string; log the error and consider throwing an exception or returning a specific error value.**

## Important Considerations

*   **Salt Management:** The `salt` value is critical for the security of the encryption.  It **must** be kept secret and securely managed.  **Do not hardcode the salt directly in your code.**  Store it in a secure configuration file or use a key management system.
*   **Initialization Vector (IV):** The IV is essential for the security of CBC mode.  It **must** be random and unique for each encryption operation.  The code uses `crypto.randomBytes` to generate a cryptographically secure random IV.
*   **Error Handling:**  The `decode` function includes a `try...catch` block, which is essential for handling decryption errors.  **Proper error handling is crucial for security and preventing application crashes.**  Log the error and consider throwing an exception or returning a specific error value instead of just returning an empty string.
*   **Key Length:** AES-256 uses a 256-bit key (32 bytes).  Ensure that the `salt` you provide to the `Ecrypt` class is exactly 32 bytes long.  If it's shorter, you'll need to pad it.  If it's longer, you'll need to truncate it.  It's best to generate a 32-byte random salt and store it securely.
*   **Cryptographically Secure Random Number Generator (CSPRNG):** The `crypto.randomBytes` function uses a CSPRNG, which is essential for generating secure IVs and salts.
*   **Base64 Encoding:** The code uses Base64 encoding to represent the binary data (IV and ciphertext) as a string.  This is necessary because these values may contain characters that are not printable or that would cause problems in certain contexts.
*   **Security Best Practices:**  Follow security best practices when using encryption.  **Never implement your own cryptographic algorithms.**  Use well-vetted, standard libraries and algorithms.  Keep your keys secret and manage them securely.  Test your encryption and decryption thoroughly.
*   **Dependencies:** The code relies on the built-in `crypto` module in Node.js and `esoftplay` for configuration.
* **Never use custom cryptographic functions:** The original code had a custom `sha5` function.  This is extremely dangerous.  **Always use well-established cryptographic functions.**  This version removes that function.
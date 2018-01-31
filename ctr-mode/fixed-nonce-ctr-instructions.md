Counter (CTR) mode of operation is still a block cipher mode
at its core, but it behaves like a stream cipher.  Rather than
passing the plaintext directly to the cipher algorithm, CTR
mode encrypts COUNTER || NONCE, generating a keystream. The
keystream is then XOR'd against the plaintext.

Any CTR mode ciphertexts that have been encrypted using the
same key and nonce have also been encrypted against the same
keystream. The plaintext characters are only an XOR away.
Furthermore, any byte value in a ciphertext at the same
index as in another ciphertext (anything in the same
"column"), has necessarily been XOR'd against the same value.

The process of "manually" (non-statistically) decrypting
bytes can look something like this:

1. Look for patterns, and based on these, venture a guess
as to what plaintext value a particular byte may represent.
In this case it's safe to assume that the plaintext is in
English.

1. Find the ASCII decimal value of your guess. Typing `ascii`
at a terminal will give you the entire table; `ascii c`
will give you just the values for some character `c`.

1. XOR the value of your guess against the encrypted byte (you
can do this in a Python REPL with `GUESS ^ ENCBYTE`).
The result, if correct, is the keystream byte for that index/
column.  Test it against other values at that index/column.
You'll know you've hit gold when you're getting characters
that make sense for an English plaintext.

1. Based on what you find, repeat this process for other
positions in the ciphertexts.
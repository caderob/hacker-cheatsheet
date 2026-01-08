# Cracking Methodology

Lab 1 - Identify the hash function of the following hash "4a41e0fdfb57173f8156f58e49628968a8ba782d0cd251c6f3e2426cb36ced3b647bf83057dabeaffe1475d16e7f62b7"
>``` shell
># 1) Identify the length of the given hash
>#    The hash is 96 hexadecimal characters long
>
># 2) Convert hexadecimal length to bits
>#    Each hexadecimal character represents 4 bits
>#    96 × 4 = 384 bits
>
># 3) Match the bit length to known hash algorithms
>#    SHA-384 produces a 384-bit hash output
>
># 4) Identify the hash function based on the bit length
>#    The hash function is SHA-384
>```
>SHA-384

Lab 2 - Identify the hash function of the following hash "$2y$10$XrrpX8RD6IFvBwtzPuTlcOqJ8kO2px2xsh17f60GZsBKLeszsQTBC"
>``` shell
># 1) Identify the prefix of the hash string
>#    The hash begins with "$2y$"
>
># 2) Map the prefix to a known hash algorithm
>#    The "$2y$" prefix identifies the bcrypt hashing algorithm
>
># 3) Identify the cost (work) factor
>#    The value "10" indicates the bcrypt cost factor (2^10 rounds)
>
># 4) Identify the remaining components of the hash
>#    The remaining string contains the salt and the bcrypt hash value
>
># 5) Identify the hash function
>#    The hash function used is bcrypt
>```
>bcrypt

# Cracking Methodology

Lab 1 - Identify the hash function of the following hash "4a41e0fdfb57173f8156f58e49628968a8ba782d0cd251c6f3e2426cb36ced3b647bf83057dabeaffe1475d16e7f62b7"
>``` shell
># The hash is 96 hexadecimal characters long
>
># Each hex character = 4 bits
>
># 96×4=384 bits
>
># SHA-384 produces a 384-bit hash output
>```
>SHA-384

Lab 2 - Identify the hash function of the following hash "$2y$10$XrrpX8RD6IFvBwtzPuTlcOqJ8kO2px2xsh17f60GZsBKLeszsQTBC"
>``` shell
># The prefix $2y$ identifies bcrypt
>
># 10 is the cost factor (work factor)
>
># The remaining string contains the salt and hash
>```
>bcrypt

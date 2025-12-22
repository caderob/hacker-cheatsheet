# Introduction to Encryption, Hashes and Cracking

Comparing resulting hash values for secret and secret1
>``` shell
>kali@kali:~$ echo -n "secret" | sha256sum
>
># ========== Expected Result ==========
>2bb80d537b1da3e38bd30361aa855686bde0eacd7162fef6a25fe97bf527a25b  -
># =====================================
>
>kali@kali:~$ echo -n "secret" | sha256sum
>
># ========== Expected Result ==========
>2bb80d537b1da3e38bd30361aa855686bde0eacd7162fef6a25fe97bf527a25b  -
># =====================================
>
>kali@kali:~$ echo -n "secret1" | sha256sum
>
># ========== Expected Result ==========
>5b11618c2e44027877d0cd0921ed166b9f176f50587fc91e7534dd2946db77d6  -
># =====================================
>```

Calculating the keyspace for a 5-character password
>``` shell
>kali@kali:~$ echo -n "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789" | wc -c
>
># ========== Expected Result ==========
>62
># =====================================
>
>kali@kali:~$ python3 -c "print(62**5)"
>
># ========== Expected Result ==========
>916132832
># =====================================
>```

Benchmark CPU with MD5, SHA1, and SHA2-256
>``` shell
>kali@kali:~$ hashcat -b
>
># ========== Expected Result ==========
>hashcat (v6.2.5) starting in benchmark mode
>...
>* Device #1: pthread-Intel(R) Core(TM) i9-10885H CPU @ 2.40GHz, 1545/3154 MB (512 MB allocatable), 4MCU
>
>Benchmark relevant options:
>===========================
>* --optimized-kernel-enable
>
>-------------------
>* Hash-Mode 0 (MD5)
>-------------------
>
>Speed.#1.........:   450.8 MH/s (2.19ms) @ Accel:256 Loops:1024 Thr:1 Vec:8
>
>----------------------
>* Hash-Mode 100 (SHA1)
>----------------------
>
>Speed.#1.........:   298.3 MH/s (3.22ms) @ Accel:256 Loops:1024 Thr:1 Vec:8
>
>---------------------------
>* Hash-Mode 1400 (SHA2-256)
>---------------------------
>
>Speed.#1.........:   134.2 MH/s (7.63ms) @ Accel:256 Loops:1024 Thr:1 Vec:8
># =====================================
>```

Benchmark GPU with MD5, SHA1, and SHA2-256
>``` shell
>C:\Users\admin\Downloads\hashcat-6.2.5>hashcat.exe -b
>
># ========== Expected Result ==========
>hashcat (v6.2.5) starting in benchmark mode
>...
>* Device #1: NVIDIA GeForce RTX 3090, 23336/24575 MB, 82MCU
>
>Benchmark relevant options:
>===========================
>* --optimized-kernel-enable
>
>-------------------
>* Hash-Mode 0 (MD5)
>-------------------
>
>Speed.#1.........: 68185.1 MH/s (39.99ms) @ Accel:256 Loops:1024 Thr:128 Vec:8
>
>----------------------
>* Hash-Mode 100 (SHA1)
>----------------------
>
>Speed.#1.........: 21528.2 MH/s (63.45ms) @ Accel:64 Loops:512 Thr:512 Vec:1
>
>---------------------------
>* Hash-Mode 1400 (SHA2-256)
>---------------------------
>
>Speed.#1.........:  9276.3 MH/s (73.85ms) @ Accel:16 Loops:1024 Thr:512 Vec:1
># =====================================
>```

Calculating the cracking time for password length of 5
>``` shell
>kali@kali:~$ python3 -c "print(916132832 / 134200000)"
>
># ========== Expected Result ==========
>6.826623189269746
># =====================================
>
>kali@kali:~$ python3 -c "print(916132832 / 9276300000)"
>
># ========== Expected Result ==========
>0.09876058687192092
># =====================================
>```

Calculating the cracking time for password length of 8 and 10 on a GPU for SHA-256
>``` shell
>kali@kali:~$ python3 -c "print(62**8)"
>
># ========== Expected Result ==========
>218340105584896
># =====================================
>
>kali@kali:~$ python3 -c "print(218340105584896 / 9276300000)"
>
># ========== Expected Result ==========
>23537.41314801117
># =====================================
>
>kali@kali:~$ python3 -c "print(62**10)"
>
># ========== Expected Result ==========
>839299365868340224
># =====================================
>
>kali@kali:~$ python3 -c "print(839299365868340224 / 9276300000)"
>
># ========== Expected Result ==========
>90477816.14095493
># =====================================
>```

Lab 1 - Answer with true or false: In symmetric encryption, one key is used for both the encryption and decryption process.
>true

Lab 2 - Answer with true or false: In asymmetric encryption, we can share the private key freely over the network to another person without risking that a third party can capture our key and then decrypt messages which get sent to us.
>false

Lab 3 - Answer with true or false: A cryptographic hash function is a one-way function. The resulting hash cannot be reversed by reversing the steps used to hash the plain text information.
>true

Lab 4 - Use the MD5 GPU hash rate from the GPU benchmark of this section and calculate the cracking time in minutes with the following conditions. Use a charset of all lower and upper case letters of the English alphabet and use a password length of 8. Enter the answer as full minutes without seconds.
>``` shell
># MD5 GPU hash rate
>Speed.#1: 68185.1 MH/s
>
># Convert MH/s → H/s:
>68185.1 × 1,000,000 = 68,185,100,000 hashes/sec
>
># Charset size:
>26 + 26 = 52
>
># Password length: 8
>52^8 = 53,459,728,531,456
>
># Cracking-time calculation
>kali@kali:~$ python3 -c "print(52**8 / 68185100000)"
>
># ========== Expected Result ==========
>783.98 seconds
># =====================================
>
># Convert seconds → minutes
>kali@kali:~$ python3 -c "print((52**8 / 68185100000) / 60)"
>
># ========== Expected Result ==========
>13.066 minutes
># =====================================
>```
>13

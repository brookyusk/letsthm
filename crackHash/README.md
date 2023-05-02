### Install jumbo john (mac)
```
git clone https://github.com/openwall/john -b bleeding-jumbo john
cd john/src/
./configure
make -s clean && make -sj4
cd ../run
./john --test
```

get hash format list
```
./john --list=formats
```


### Install hash-identifier
```
wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
python3 hash-id.py
```


### Get wordlist
```
git clone https://github.com/danielmiessler/SecLists.git
```


### Example
#### Dictonay mode
* Whirlpool
```
cat hash.txt
> c5a60cc6bbba781c601c5402755ae1044bbf45b78d1183cbf2ca1c865b6c792cf3c6b87791344986c8a832a0f9ca8d0b4afd3d9421a149d57075e1b4e93f90bf

cat hash.txt | python3 hash-id.py
> Possible Hashs:
> [+] SHA-512
> [+] Whirlpool

john --format=Raw-SHA512 --wordlist=$(pwd)/SecLists/Passwords/Leaked-Databases/rockyou.txt hash.txt
> Using default input encoding: UTF-8
> Loaded 1 password hash (Raw-SHA512 [SHA512 256/256 AVX2 4x])
> Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
> 0g 0:00:00:02 DONE (2023-05-03 07:46) 0g/s 5178Kp/s 5178Kc/s 5178KC/s   kaitlynn4..*7¡Vamos!
> Session completed.

john --format=Whirlpool --wordlist=$(pwd)/SecLists/Passwords/Leaked-Databases/rockyou.txt hash.txt
> Using default input encoding: UTF-8
> Loaded 1 password hash (whirlpool [WHIRLPOOL 32/64])
> Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
> colossal         (?)
> 1g 0:00:00:00 DONE (2023-05-03 07:53) 1.923g/s 1301Kp/s 1301Kc/s 1301KC/s columbian..cobreloa
> Use the "--show" option to display all of the cracked passwords reliably
> Session completed.
```

Note)
Results are saved to `john/run/john.pot` file in default.
You can modify the file location by `$JOHN` envvar.

You can see saved result by `--show` option.
```
john --format=Whirlpool --show hash.txt
```

* Other formats
* md5
```
john --format=Raw-md5 --wordlist=path_to_wordlist target_file

```

* SHA1
```
john --format=Raw-SHA1 --wordlist=path_to_wordlist target_file
```

* SHA256
```
john --format=Raw-SHA256 --wordlist=path_to_wordlist target_file
```

* NTHash
```
john --format=NT --wordlist=path_to_wordlist target_file
```

* /etc/passwd and /etc/shadow
```
grep root /etc/passwd >> unshadowed.txt
grep root /etc/shadow >> unshadowed.txt

john --format=sha512crypt --wordlist=$(pwd)/SecLists/Passwords/Leaked-Databases/rockyou.txt unshadowed.txt
> Using default input encoding: UTF-8
> Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
> Cost 1 (iteration count) is 5000 for all loaded hashes
> Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
> 1234             (root)
> 1g 0:00:00:00 DONE (2023-05-03 08:15) 1.053g/s 1212p/s 1212c/s 1212C/s kucing..summer1
> Use the "--show" option to display all of the cracked passwords reliably
> Session completed.
```


#### Single mode
easy password such like able to assume from username.
```
cat hash.txt | python3 hash-id.py
> Possible Hashs:
> [+] MD5
> [+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))

# assuming username is joker
john --single --format=raw-md5 --single-seed=joker hash.txt
> Using default input encoding: UTF-8
> Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x5])
> Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
> Warning: Only 24 candidates buffered for the current salt, minimum 40 needed for performance.
> Jok3r            (?)
> 1g 0:00:00:00 DONE (2023-05-03 08:42) 100.0g/s 19800p/s 19800c/s 19800C/s rekoj0..J0k3r
> Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
> Session completed.
```


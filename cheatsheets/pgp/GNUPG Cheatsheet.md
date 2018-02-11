GNUPG CHEATSHEET
=================

Setting up: key generation
--------------------------

This generates a public/private keypair.

    $ gpg --gen-key
    $ gpg --list-secret-keys


Import a private key (e.g. to copy it from one computer to another):

    $ gpg --allow-secret-key-import --import "my private key.txt"



Importing someone else's public key
-----------------------------------

When someone sends you a PGP-encrypted message, you need that person's public key to decrypt it. When you want to send an encrypted message to someone, you need that person's public key to encrypt it. Public keys often come in an ASCII-encoded format, e.g.:

    $ cat "Alice's public key.txt"
    -----BEGIN PGP PUBLIC KEY BLOCK-----
    Version: GnuPG v1

    mQENBFPUn4MBCAChbhCGGzdPx1ZVWlQo9miXBhq/elOk1FLiK4xQ/dWeBE2VP+wc
    W6YVQKShAhYMjxTuKc4kkVba0qtBDdvz+ZsrxYtmiKSnNv9irvEkoa6wP+xN28Hq
    PTpLnaxGSiVb/5tQ+A6uJLrQZRhyjnjMG8ZAGqTKzhP2JFMTSx0rg2Vo5uV6/ATJ
    yNnDq3dGrObzW4VbLJUyk+xzF9rNRywF2Buo0xV9Q/oQWuGvCgbyKPcSl6TtzqIy
    xwp5LA5NDYC7NciaCL1TlpXpTsPwFR7cMW9O0tdJ4G4Q1/6vGUiPlh26dyJSG/5T
    H6jrMPWaO12CSAbe/2pg6jz3hXcUG7NyJgBNABEBAAG0JkMuQS5QLiBMaW5zc2Vu
    IDxjaGFybEB0dXJpbmdiaXJkcy5jb20+iQE4BBMBAgAiBQJT1J+DAhsDBgsJCAcD
    AgYVCAIJCgsEFgIDAQIeAQIXgAAKCRDmAl90BS4PY8iwB/9OVUIWFGk969s1chQb
    I2e2Ck5uVPGaHdpk/S2QvJDhi2B1rVu1zr49c5ApsGu07q7NZZ5S6jxoXywKDRgI
    idp3A+hZBCHDnnTy8kCuHyCY75KJSvqdKiN/xx1S87pp7y+MmxYosYFtFKtD+Ydg
    jLaksOie1j4PjtSrd3q11W4JRVy6jGAvMUGkuLS57LvzPG7bvOwTYEEB+Ydcygad
    I/EqU1IG7g1DPtFEMXQLz8ZkE+FrGVOFuK5cLwGTdhQR1sAOWJyH5P9mrVhG3X58
    Xn1uQw+gnagBNPp2NiY+VBVGaP+COkbNQuderl77Il0eck5K12TESw4wG/WOj9CL
    vLPiuQENBFPUn4MBCADZ49cOzWy5j97IbvJEdc4PnVOtU0gswE4WMH6ZmhJhVp0t
    IEz1ZdqIub9Ujvne6md68MnqrEmaQGexXB3oMtl1r+9oETPecqxZswGwzkFpXtxA
    08awxyFqW7hVEP5MWNbB9FOvk2MdTMypKyZJNL8KQIPn/SiipwTkIE+i5QN/6DvZ
    GHC5FByHA7NZuYxqxA0Yv+TbVPDSnOo5Qi8s3JpNDiolohzmTTnt7kgL4oSZWfZT
    p1x5apJ9cgpw9ezLjN213Tq2+Znh8+88GIhtD++4wXhqgXeLEnttMAvhqYJVYtLq
    wxQa4var2/l18atMLiTXFp/O2/3yOwzttskNshLbABEBAAGJAR8EGAECAAkFAlPU
    n4MCGwwACgkQ5gJfdAUuD2NyrQgAhILZafp9BZwV2tN+oIy9rvWb/R/mwH093h03
    4/R4e65ZSIv11B+w3JDnzR4EZJKVoiQXFAG09UQihLlNmJ32mQK01TZKV5RlhojE
    iPDW3VJ2W/HJrsYQuyEJYYVuVow2YzjVUDuXZJMswstbwc6ns1B84yNCD0xaXHXx
    8gJjJrk3YcyV0kKJhGLh5MN0Dyb60amR81b1QnjPFj3ricV4zNu/wTET85GBYDH4
    oPwlu/sW0GGP5I2LWF4dXjA4bdGmYhcRYtOuRim8bEcp+Kt6F65LXAUrmDN9SvZc
    ox5tddtqP979yoxpnHdG/z4i+5vPVyVnyxtdlZ3HLW+l82x9AQ==
    =imEZ
    -----END PGP PUBLIC KEY BLOCK-----

Import the key:

    $ gpg --import "Alice's public key.txt"

The key is now in the list:

    $ gpg --list-keys
    /home/archels/.gnupg/pubring.gpg
    --------------------------------
    pub   2048R/052E0F63 2014-07-27
    uid                  Alice <alice@example.com>
    sub   2048R/C91CCABB 2014-07-27

Note that the public key contains their owner's name and e-mail address.

**Warning:** there is no guarantee that a given key actually belongs to that person. For more information on mitigating this issue, see the section `Keyservers' below.


Sending a message
-----------------

Create a message that you want to send.

    $ cat msg.txt
    The Tao that can be spoken is not the eternal Tao
    The name that can be named is not the eternal name
    The nameless is the origin of Heaven and Earth
    The named is the mother of myriad things
    Thus, constantly without desire, one observes its essence
    Constantly with desire, one observes its manifestations
    These two emerge together but differ in name
    The unity is said to be the mystery
    Mystery of mysteries, the door to all wonders

Optional: import the public key of the person you are sending the message to (if it is not already in the database).

    $ gpg --import "Alice's public key.txt"

Encrypt the message. The message is encrypted using your own private key.

    $ gpg -e -a -u "sender username" -r "receiver username" --output msg_encrypted.txt msg.txt

Options:

    -e

Encrypt

    -a

Generate ASCII output (not binary)

    -u

Specifies the secret key to be used.

    -r

Specifies the public key of the recipient.

    --output

The encrypted message will be written to this file. If not specified, the output file name will be the input file name appended with the extension `.asc` (for ASCII) or `.gpg` (for binary).

Note that for both -u and -r, you can type any part of the name or e-mail address to match the database. For example, `-u "Pott"` would match "Harry Potter". Matching is case-insensitive.

The encrpyted message will look something like this:

    -----BEGIN PGP MESSAGE-----
    Version: GnuPG v1

    hQEMA1vzHWkY2kPEAQgAk3AvnOC/w4wc4CaFbYDZGAenak82lN6KT5bUi2RIxYtJ
    MeGrD+ShYER5P9ZV7/YdtLpIhEvwAnof6jrv31xiNnbYWNO9tO9NlbqFUCa1yWaz
    M+WihqY3Zevl4eSnyjdSq4Z6At5jkGZlnz8+bgzaAMqpYofWTAIby/T6DxC3MQ2A
    8L/ka7pPUTlqsrb/rHcAx4nkiJlPQAD53kP52lOUoAKS53VCA+wY8Y0CsXXWNhqc
    RJ3+PQtqE9INrWglziNRaiPexLLNhPC9mk3i+hTMSEew7wFCa4CSsUZuJdDhsxk8
    Ksayth+0q4EhOtMXzbhInT1vuy3SpIQWMlalc5xIUNLAnQFkp33AleLeIi9/NGEr
    oBvRHHxFE6OEJS50QJY4UB3BMzRqxLalSkaRk0zlo23Ydeme8MqPKnkoV1+aepSq
    9AkKdCF1wjLVUbIuEemJwKREoOgj+vC91ep2ZtVyoQ6A0tUMaF4yxY7BFUeybC1z
    8VS3PM4f6BAJbTQ2BPYZfuD27TzcYgqqTVHQH7+zPw6D/qeX/bhKLspZ3xYE7jkr
    8DDty+tNG07HSkTZT2vKa0LfK11NHjoyCJ7RJUJZw7ABu9iMosC9PqgjeHy2EN0v
    lFHG9pHc/3BnTDWNjBiEj3njiS1dwm1/JIiRFEFZnojLIXS6JzTe26teLhULYwBE
    EmPw7+R6dULC9d+/NmmzI7v/M216LBzPR0iHxToI2V4hG4tjRQN5nXrtY/yA5Hd7
    4KRSd5iPlDHuMSocnVWNeFW2idKVSlFLWNupdIzTwT2+GdtqfNXlhVhtF9APP3A=
    =P6dn
    -----END PGP MESSAGE-----

Note that the size of the output follows the size of the input linearly. To prevent giving away information about the size of the message, you can pad it with random data before encrypting:

    $ cat /dev/urandom | od -A n -t x1 | head -n 1000 >> msg.txt

The number (1000) selects the size of the padding. `od` encodes the random data in hex.


Receiving a message
-------------------

First, make sure that the public key of the person you are receiving the message from is in your keyring (see the section `Importing someone else's public key' in this document).

You can decrypt the message as follows:

    $ gpg -d msg.txt


Sharing/backing up keys
-----------------------

Export a public key into file:

    $ gpg -ao publickey.asc --export joe@example.com

outputs your public key as an ASCII armored file (-a) with name (-o) ``publickey.asc``. The key is identified here by its email address ``joe@example.com``.

Export a private key into file:

    $ gpg -ao privatekey.asc --export-secret-keys joe@example.com

outputs your private key as an ASCII armored file (-a) with name (-o) ``privatekey.asc``. The key is identified here by its email address ``joe@example.com``.

**You'll want to back up your private (and public) key for obvious reasons.**

Keyservers
----------

Because someone who wants to send information to you has no guarantee that a given public key actually belongs to you, it is a good idea to disseminate your public key as broadly as possible. This includes submitting your public key to a number of keyservers.

Send public key to a keyserver:

    $ gpg --keyserver serverurl --send-keys XXXXXXXX

sends a key with the ID ``XXXXXXXX`` to a keyserver with the optional URL ``serverurl`` (for example hkp://pool.sks-keyservers.net).

Get public key from a keyserver:

    $ gpg --keyserver serverurl --recv-key XXXXXXXX

Gets a key with the ID ``XXXXXXXXâ`` from a keyserver with the URL ``serverurl`` (for example hkp://pool.sks-keyservers.net).


Deleting/revoking keys
----------------------

    $ gpg --delete-secret-key "username"
    $ gpg --delete-key "username"

Generate a revocation certificate:

    $ gpg -ao certificate.asc --gen-revoke XXXXXXXX

Starts the process for creating a revocation certificate for the entered ID (XXXXXXXX).


*source* https://gist.github.com/turingbirds/3df43f1920a98010667a

# VIDA Sample
This sample directory contains an unsigned image (kangaroo-unsigned.jpg) and an already-signed image (kangaroo-signed.jpg).

This code is provided as an *example* of how to sign and validate a VIDA record. It relies on exiftool, openssl, and common Linux command-line tools.

## Dependencies
Your system should already include programs like `grep`, `tr`, and `openssl`. (Although OpenSSL 1.x is obsolete, it is still widely deployed. This code works with both OpenSSL 1.x and 3.x.)

For ExifTool, we *strongly* recommend getting the latest version from the developer. The version found in most Linux repositories is *very old*.
1. Go to [https://exiftool.org/](https://exiftool.org/) and download the latest version.
2. Follow the installation instructions.
```
# Unpack it.
# cd into the Image-ExifTool directory.
perl Makefile.PL
make
make test
sudo make install
```

## Verifying the Signed Image
The signed image contains a VIDA record that specifies a domain that contains a public key.  The verify script will extract the VIDA record from the file, retrieve the public key from DNS, and validate the signature.
```
./vida-verify.sh kangaroo-vida.jpg
```
To see a verbose step-by-step explanation of the script, add `-v`:
```
./vida-verify.sh -v kangaroo-vida.jpg
```

## Create Your Own Signatures
To sign your own images:
1. Edit the Makefile and specify your domain name and file to sign.
2. Generate your public and private key: `make keys`
3. Add your public key to your DNS TXT record. Then wait until it propagates. (for most DNS providers, this is usually 10-30 minutes.)  Use `make testdns` to see if the DNS is ready.
4. Try encoding your file: `make sign`
5. Try validating your file: `make check`

After generating your keys, you can also sign using:
```
./vida-sign *domain* *private.key* *unsigned.jpg* *signed.jpg*
```
This will use your private key and domain for signing the unsigned image. This will generate the signed image.

For validating, you only need the signed image:
```
./vida-verify.sh *signed.jpg*
```
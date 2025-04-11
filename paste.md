## Summary: Package Containing `libcrypto.so` <br>
---<br>
The `libcrypto.so` file is part of the OpenSSL library, which provides cryptographic functions and protocols. This shared library is essential for various applications that require secure communication and data encryption.

1  
---  
The `libcrypto.so` file is typically included in the OpenSSL package. Depending on your operating system, the package name may vary. For example:  
- On **Debian-based systems** (like Ubuntu), you can install it using:  
  ```bash
  sudo apt-get install libssl-dev
  ```
  This command installs the development files for OpenSSL, which include `libcrypto.so`.

- On **Red Hat-based systems** (like CentOS or Fedora), you can use:  
  ```bash
  sudo yum install openssl-devel
  ```
  This command installs the OpenSSL development package, which also contains `libcrypto.so`.

2  
---  
The `libcrypto.so` file is usually located in the `/usr/lib` or `/usr/lib64` directory, depending on whether you are using a 32-bit or 64-bit system. You can verify its presence by running:  
```bash
ls /usr/lib | grep libcrypto.so
```
This command lists the files in the `/usr/lib` directory and filters for `libcrypto.so`.

3  
---  
To check the version of OpenSSL and confirm that `libcrypto.so` is correctly installed, you can run:  
```bash
openssl version
```
This command will display the version of OpenSSL currently installed on your system, ensuring that the library is available for use.

---  
Example: If you are developing an application that requires SSL/TLS support, you would link against `libcrypto.so` to utilize its cryptographic functions. For instance, in a C program, you might include the OpenSSL headers and compile your code with:  
```bash
gcc myapp.c -o myapp -lssl -lcrypto
```
This command compiles `myapp.c`, linking it against both the SSL and Crypto libraries.

---  
References:  
## https://www.openssl.org/  
## https://packages.ubuntu.com/  
## https://www.rpmfind.net/  
# pq-mlkem768x25519-openssl-debian
 Post-quantum key encapsulation on Debian: OpenSSL, oqs-provider and Python 3.  
  
![alt text[]()](https://raw.githubusercontent.com/ran-sama/postquantum-mlkem768x25519-openssl-debian/refs/heads/master/python_oqs_openssl.png)  
Checking if the module is loaded is easy, but I recommend working with a copy and not edit your system wide conf:  
![alt text](https://raw.githubusercontent.com/ran-sama/pq-mlkem768x25519-openssl-debian/refs/heads/master/quantum_safe.png)  

## Compile
Go to the release artifacts and pick the latest stable or candidate:  
https://github.com/open-quantum-safe/oqs-provider/releases  
I am building on 32-bit Exynos 5422, so if you are on arm64 or amd64 locate your own binaries for the µarch:  
```
whereis openssl
whereis libcrypto.so
```
These are used here:  
```
wget https://github.com/open-quantum-safe/oqs-provider/archive/refs/tags/0.8.0.tar.gz
tar -xzf 0.8.0.tar.gz
cd oqs-provider-0.8.0
env OPENSSL_ROOT=/usr/bin/openssl CMAKE_PARAMS="-DOPENSSL_CRYPTO_LIBRARY=/usr/lib/arm-linux-gnueabihf/libcrypto.so" bash scripts/fullbuild.sh
sudo cmake --install _build
scripts/runtests.sh
cd ..
```

## Configure services
```
[Unit]
Description=server_DL
#Wants=network.target
After=network.target

[Service]
Environment="OPENSSL_CONF=/home/ran/mlkem768x25519.cnf"
Type=idle
#Type=notify
Restart=on-failure
#ExecStartPre=/bin/sleep 10
ExecStart=/usr/bin/python3 /home/ran/net/server_DL.py
#WorkingDirectory=/home/pi
#Environment=PYTHONUNBUFFERED=1
StandardOutput=journal
StandardError=journal
User=ran
Group=ran

[Install]
WantedBy=multi-user.target
```
## Comment out the default KEX if you are using my Python 3 server repo
```
#sslcontext.set_ecdh_curve("secp521r1")#works well on firefox but you can also use secp384r1
```

## Restart server
```
sudo systemctl restart fox1.service
```

## License
Licensed under the WTFPL license.

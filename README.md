# pq-mlkem768x25519-openssl-debian
 Post-quantum key encapsulation on Debian: OpenSSL 3.50. and Python 3.  
  
![alt text[]()](https://raw.githubusercontent.com/ran-sama/postquantum-mlkem768x25519-openssl-debian/refs/heads/master/python_oqs_openssl.png)  
  
Formerly this would require the Open Quantum Safe (OQS) project as OpenSSL module, but it runs on plain OpenSSL 3.5.0 now without any external dependencies and compilation steps! You only need Python 3.14+ and OpenSSL 3.5.0+ which are already shipped in Debian 13 Trixie as of April 8th, 2025.  
  
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

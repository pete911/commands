# Public key

Generate public key from private key
- PEM format `openssl rsa -in <key.pem> -pubout -out <pubkey.pem>`
- ssh format `ssh-keygen -y -f <key.pem> > <pubkey.pub>`

# SSH

## config
- [.ssh config](config) example

# Public key

Generate public key from private key
- PEM format `openssl rsa -in <key.pem> -pubout -out <pubkey.pem>`
- ssh format `ssh-keygen -y -f <key.pem> > <pubkey.pub>`

# SSH

To be able to use agent forwarding (able to ssh from one host to another
without copying keys), you need to:
- add ssh key `ssh-add --apple-use-keychain <key>`
- verify key is loaded `ssh-add -l`
- add `ForwardAgent yes` to your ssh config, or use `-A` option with ssh command

## config
- [.ssh config](config) example

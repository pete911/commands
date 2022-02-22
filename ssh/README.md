# Public key

Generate public key from private key
- PEM format `openssl rsa -in <key.pem> -pubout -out <pubkey.pem>`
- ssh format `ssh-keygen -y -f <key.pem> > <pubkey.pub>`

# SSH

To be able to use agent forwarding (able to ssh from one host to another
without copying keys), you need to:
- add ssh key `ssh-add --apple-use-keychain <private-key>`
- verify key is loaded `ssh-add -l`
- add `ForwardAgent yes` to your ssh config, or use `-A` option with ssh command

## ssh-add

- `ssh-add --apple-use-keychain <private-key>` add ssh key
- `ssh-add -l` list fingerprints of all identities
- `ssh-add -L` list public key parameters
- `ssh-add -d <private-key>` remove identity
- `ssh-add -D` remove all identities

## config
- [.ssh config](config) example

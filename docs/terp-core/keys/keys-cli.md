# Basic Key Management

Create, import, export and delete keys using the CLI keyring. 

For this guide, The Terp Core Binary is required which you can install from here.

## Create a new key

```
keys add <wallet_name>
```

You can create a new key with the name `Default` as in the following example:
::: details Example: Create a new address

```bash
$ terpd keys add Default
- name: Default
  type: local
  address: terp1quw5r22pxy8znjtdkgqc65atrm3x5hg6vycm5n
  pubkey: terppub1addwnpepqdct05khsxvtaaj0stuvayrpw0j8t6styr7vu05k3y63d5540ftuz8x6tsq
  mnemonic: ""
  threshold: 0
  pubkeys: []

**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

spare leopard potato hospital series salt model myself bronze print despair please mutual rival battle lumber crater brain food artwork goose west talent ritual
```

:::
The key comes with a "mnemonic phrase", which is serialized into a human-readable 24-word mnemonic. User can recover their associated addresses with the mnemonic phrase.

:::danger
It is important that you keep the mnemonic for address secure, as there is **no way** to recover it. You would not be able to recover and access the funds in the wallet if you forget the mnemonic phrase.
:::

## Restore existing key by seed phrase

```
terpd keys add <key_name> --recover
```

You can restore an existing key with the mnemonic.

::: details Example: Restore an existing key

```bash
$ terpd keys add Default_restore --recover
> Enter your bip39 mnemonic
## Enter your 24-word mnemonic here ##
```

:::


## List your keys

```
keys list
```

Multiple keys can be created when needed. You can list all keys saved under the storage path.

::: details Example: List all of your keys

```bash
$ terpd keys list
    - name: Default
    type: local
    address: ## Address of "Default" ##
    pubkey: ## Pubkey of "Default" ##
    mnemonic: ""
    threshold: 0
    pubkeys: []
  - name: Default_restore
    type: local
    address: ## Address of "Default_restore" ##
    pubkey: ## Pubkey of "Default_restore" ##
    mnemonic: ""
    threshold: 0
    pubkeys: []
```

:::

## Retrieve key information

```
keys show <key_name>
```

You can retrieve key information by its name:

::: details Example: Retrieve key information - Account Address and its public key

```bash
$ terpd keys show Default --bech acc
- name: Default
  type: local
  address: terp1quw5r22pxy8znjtdkgqc65atrm3x5hg6vycm5n
  pubkey: terppub1addwnpepqdct05khsxvtaaj0stuvayrpw0j8t6styr7vu05k3y63d5540ftuz8x6tsq
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

:::

::: details Example: Retrieve key information - Validator Address and its public key

```bash
$ terpd keys show Default --bech val
- name: Default
  type: local
  address: osmocncl1zdlttjrqh9jsgk2l8tgn6f0kxlfy98s3prz35z
  pubkey: osmocnclpub1addwnpepq0ua07k8p3vrv5dap4pl77n4gjyyqsqrndzu0tdrr60ddhfg6ah0ck5ad5l
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

:::

::: details Example: Retrieve key information - Consensus nodes Address and its public key

```bash
$ terpd keys show Default --bech cons
- name: Default
  type: local
  address: osmocnclcons1zdlttjrqh9jsgk2l8tgn6f0kxlfy98s34pfmlc
  pubkey: osmocnclconspub1addwnpepq0ua07k8p3vrv5dap4pl77n4gjyyqsqrndzu0tdrr60ddhfg6ah0ch6kdrc
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

:::


## Delete a key

```
keys delete <key_name>
```

You can delete a key in your storage path.

:::danger
Make sure you have backed up the key mnemonic before removing any of your keys, as there will be no way to recover your key without the mnemonic.
:::

::: details Example: Remove a key

```bash
$ terpd keys delete Default_restore1
Key reference will be deleted. Continue? [y/N]: y
Key deleted forever (uh oh!)
```

:::

## Export private keys

```
keys export <key_name>
```

You can export and backup your key by using the `export` subcommand:

::: details Example: Export your keys
Exporting the key _Default_ :

```bash
$ terpd keys export Default
Enter passphrase to encrypt the exported key: ## Insert passphrase (must be at least 8 characters)##
-----BEGIN TENDERMINT PRIVATE KEY-----
kdf: bcrypt
salt: ## Salt of the key ##
type: secp256k1

## Tendermint private key ##
-----END TENDERMINT PRIVATE KEY-----
```

:::

## The keyring-backend option
Interacting with a node requires a public-private key pair. Keyring is the place holding the keys. The keys can be stored in different locations with specified backend type. 
```
$ terpd keys [subcommands] --keyring-backend [backend type]
```
### os backend
The default `os` backend stores the keys in operating system's credential sub-system, which are comfortable to most users, yet without compromising on security. 

Here is a list of the corresponding password managers in different operating systems:
- macOS (since Mac OS 8.6): [Keychain](https://support.apple.com/en-gb/guide/keychain-access/welcome/mac)
- Windows: [Credentials Management API](https://docs.miosmosoft.com/en-us/windows/win32/secauthn/credentials-management)
- GNU/Linux:
  - [libsecret](https://gitlab.gnome.org/GNOME/libsecret)
  - [kwallet](https://api.kde.org/frameworks/kwallet/html/index.html)

### file backend
The `file` backend stores the encrypted keys inside the app's configuration directory. A password entry is required everytime a user access it, which may also occur multiple times of repeated password prompts in one single command.

### test backend
The `test` backend is a password-less variation of the `file` backend. It stores unencrypted keys inside the app's configuration directory. It should only be used in testing environments and never be used in production.
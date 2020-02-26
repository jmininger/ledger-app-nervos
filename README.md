# Development #

## Developing on the Ledger Nano S ##

### Build ###

``` sh
$ nix-shell -A wallet.s --run 'make SHELL=sh all'
```

### Load ###

``` sh
$ nix-shell -A wallet.s --run 'make SHELL=sh load'
```

## Developing on the Ledger Nano X ##

### Build ###

``` sh
$ nix-shell -A wallet.x --run 'make SHELL=sh all'
```

### Load ###

Ledger Nano X does not currently have a way to load unsigned apps.
Testing need to be done through Ledger Live.


# Testing #


## Automated Testing ##

You can run automated tests via the ./test.sh script. To do this,
first load the wallet app:

``` sh
$ nix-shell -A wallet.s --run 'make SHELL=sh load'
```

You have to accept a few prompts on the Ledger. Then you must select
and load the Nervos app.

Now, make sure the Ledger is:

- connected
- unlocked
- has the “Nervos” app open (shows “Use wallet to view accounts”)

Then, just run this:

``` sh
$ nix-shell -A wallet.s --run 'make SHELL=sh test'
```

## Manual Testing ##

Not everything is coverd by the automated tests, so manual testing is
also necessary. This enables end-to-end testing of the ckb 

Here is how to set up an environment to test the Ledger app on Nano S
using the ckb command line:

Load the latest version of the Nervos app, confirming the unsafe
prompt:

``` sh
$ nix-shell -A wallet.s --run 'make SHELL=sh load'
```

You have to accept a few prompts on the Ledger. Then you must select
and load the Nervos app, confirming the unsafe prompt.

Build a version of the ckb-cli:

``` sh
$ nix-shell -p '(import ./nix/dep/ckb-cli {})'
```

Now, make sure the Ledger is:

- connected
- unlocked
- has the “Nervos” app open (shows “Use wallet to view accounts”)

### Get Public Key ###

Get the public key:

``` sh
$ ckb-cli account extended-address-ledger --path "m/44'/309'/0'/0/0"
```

This should show up on the ledger as “Provide Public Key” for
44’/309’/0’/0/0. Accept it on the Ledger and verify ckb prints the
resulting address. The result should lookk like:

``` text
account_source: on-disk password-protected key store
address:
  mainnet: ckb1qyqxxtzygxvjwhgqklqlkedlqqwhp0rqjkvsqltkvh
  testnet: ckt1qyqxxtzygxvjwhgqklqlkedlqqwhp0rqjkvsa64fqt
lock_arg: 0x632c444199275d00b7c1fb65bf001d70bc609599
```

### List Ledger Wallets ###

List the wallets:

``` sh
$ ckb-cli account list
- "#": 0
  account_source: ledger hardware wallet
  ledger_id: 0x69c46b6dd072a2693378ef4f5f35dcd82f826dc1fdcc891255db5870f54b06e6
```

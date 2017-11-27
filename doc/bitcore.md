Bitcore Integration
===================

We implemented bitcore features in this repo by researching the difference between the official bitcoin and bitpay-bitcoin. See:

https://github.com/bitcoin/bitcoin/compare/9779e1e...bitpay:597c9b4

Basically, it adds "address index" feature.

But the difficulty of integrating it into Crown is that Crown is based on v0.10, while bitpay-bitcoin is based on v0.12. Details:

All files in `qa/rpc-tests` are not added yet, because the test framework seems different.

The predecessor of `CDBWrapper` is `CLevelDBWrapper`. So bitpay-bitcoin's `CDBWrapper` should be renamed.

Obfuscation isn't supported in v0.10. So bitpay-bitcoin's obfuscation-related code should be removed. `CLevelDBWrapper` function shouldn't take `obfuscate` parameter.

In `src/main.cpp`, v0.10 uses `state.Abort`, but v0.12 doesn't use this -- it uses `AbortNode`. My inserted code uses `AbortNode`.

`GetTransaction` function in v0.10 takes 4 parameters. In v0.12 it takes 5 parameters. My inserted code uses 4 parameters.

In `src/rpcserver.cpp`, each element in `vRPCCommands` array has 2 additional booleans (`threadSafe` and `reqWallet`) in v0.10, but v0.12 lacks them. My inserted code has those booleans. What I inserted is all `false` -- very likely that's incorrect.

Note: v0.10 and v0.12 in this article mean the versions of Bitcoin Core, not Crown Core.
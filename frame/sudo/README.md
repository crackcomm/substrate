# Sudo Module

- [`sudo::Trait`](./trait.Trait.html)
- [`Call`](./enum.Call.html)

## Overview

The Sudo module allows for a single account (called the "sudo key")
to execute dispatchable functions that require a `Root` call
or designate a new account to replace them as the sudo key.
Only one account can be the sudo key at a time.

## Interface

### Dispatchable Functions

Only the sudo key can call the dispatchable functions from the Sudo module.

* `sudo` - Make a `Root` call to a dispatchable function.
* `set_key` - Assign a new account to be the sudo key.

## Usage

### Executing Privileged Functions

The Sudo module itself is not intended to be used within other modules.
Instead, you can build "privileged functions" (i.e. functions that require `Root` origin) in other modules.
You can execute these privileged functions by calling `sudo` with the sudo key account.
Privileged functions cannot be directly executed via an extrinsic.

Learn more about privileged functions and `Root` origin in the [`Origin`] type documentation.

### Simple Code Snippet

This is an example of a module that exposes a privileged function:

```rust
use frame_support::{decl_module, dispatch};
use frame_system::ensure_root;

pub trait Trait: frame_system::Trait {}

decl_module! {
    pub struct Module<T: Trait> for enum Call where origin: T::Origin {
		#[weight = 0]
        pub fn privileged_function(origin) -> dispatch::DispatchResult {
            ensure_root(origin)?;

            // do something...

            Ok(())
        }
    }
}
```

## Genesis Config

The Sudo module depends on the [`GenesisConfig`](./struct.GenesisConfig.html).
You need to set an initial superuser account as the sudo `key`.

## Related Modules

* [Democracy](../pallet_democracy/index.html)

[`Call`]: ./enum.Call.html
[`Trait`]: ./trait.Trait.html
[`Origin`]: https://docs.substrate.dev/docs/substrate-types

License: Apache-2.0
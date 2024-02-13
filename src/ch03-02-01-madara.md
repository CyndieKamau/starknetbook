# Madara

Madara is a Starknet sequencer that operates on the Substrate framework, executing Cairo programs and Starknet smart contracts with the Cairo VM. Madara enables the launch and control of Starknet Appchains or L3s.

## Get Started with Madara

Madara has a starter app template

## Pre-requisites

Before setting up and developing on madara you need to setup your development environment, defined [here.](https://docs.substrate.io/install/)

## Build the App Chain

Madara has a starter app template to help you quickly bootstrap your project, clone the repo [here:](https://github.com/keep-starknet-strange/madara-app-chain-template?tab=readme-ov-file)

```sh
git clone git@github.com:keep-starknet-strange/madara-app-chain-template.git

```

Change directory:

```sh
cd madara-app-chain-template
```

Use the following command to build the node without launching it:

```sh
cargo build --release
```

Note that the initial build may take some time to finish compiling.

## Single-Node Development Chain

Set up the chain with the genesis config. More about defining the genesis state is mentioned below.

```sh
./target/release/app-chain-node setup --chain dev --from-local ./configs
```

The following command starts a single-node development chain.

```sh
./target/release/app-chain-node --dev
```

Your node is launched successfully when you see this:

```sh
2024-02-12 11:44:14 Madara initialized w/o DA layer
2024-02-12 11:44:14 Madara Node
2024-02-12 11:44:14 ✌️  version 0.1.0-dbf0c366a4f
2024-02-12 11:44:14 ❤️  by Abdelhamid Bakhta <@keep-starknet-strange>:Substrate DevHub <https://github.com/substrate-developer-hub>, 2017-2024
2024-02-12 11:44:14 📋 Chain specification: Development
2024-02-12 11:44:14 🏷  Node name: Alice
2024-02-12 11:44:14 👤 Role: AUTHORITY
2024-02-12 11:44:14 💾 Database: RocksDb at /tmp/substratelcKEoL/chains/dev/db/full
2024-02-12 11:44:14 ⛓  Native runtime: madara-100 (madara-1.tx1.au1)
2024-02-12 11:44:15 🧪 Using the following development accounts:
2024-02-12 11:44:15 🧪 NO VALIDATE with address: 0x1 and no pk
2024-02-12 11:44:15 🧪 ARGENT with address: 0x2 and pk: 0xc1cf1490de1352865301bb8705143f3ef938f97fdf892f1090dcb5ac7bcd1d
2024-02-12 11:44:15 🧪 OZ with address: 0x3 and pk: 0xc1cf1490de1352865301bb8705143f3ef938f97fdf892f1090dcb5ac7bcd1d
2024-02-12 11:44:15 🧪 CAIRO 1 with address: 0x4 and no pk
2024-02-12 11:44:15 🔨 Initializing Genesis block/state (state: 0x6663…11d4, header-hash: 0x579b…ebef)
2024-02-12 11:44:15 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2024-02-12 11:44:16 Using default protocol ID "sup" because none is configured in the chain specs
2024-02-12 11:44:16 🏷  Local node identity is: 12D3KooWS2d7Y4r96efZQYr59cLeyeZZRN3xmpBJx4NeZPKHQeFC
...
```

Note that the current node (Alice):

- Is initialized without a Data Availability Layer
- Is using the RocksDb database
- Uses GRANDPA consensus protocol
- The genesis state is `0x6663…11d4`
- `🏷  Local node identity is: 12D3KooWS2d7Y4r96efZQYr59cLeyeZZRN3xmpBJx4NeZPKHQeFC` uniquely identifies our node

You'll know that your local node is succesfully producing blocks when you see this:

```sh
2024-02-12 11:44:18 ✨ Imported #1 (0xa3f4…4f0a)
2024-02-12 11:44:18 [1] 🐺 Running offchain worker at block 1.
2024-02-12 11:44:18 [1] 🐺 No last known Ethereum block number found. Skipping execution of L1 messages.
2024-02-12 11:44:21 💤 Idle (0 peers), best: #1 (0xa3f4…4f0a), finalized #0 (0x579b…ebef), ⬇ 0 ⬆ 0
2024-02-12 11:44:24 🩸 Starting consensus session on top of parent 0xa3f4b608c39a436166211069764b5c4cfb6fc29254dcda861e092296dff44f0a
2024-02-12 11:44:24 🥷 Prepared block for proposing at 2 (7 ms) [hash: 0x68c13cd562a5081777ebf42f944468b7faf550beae096e2c6eceec589b01c5be; parent_hash: 0xa3f4…4f0a; extrinsics (2)
2024-02-12 11:44:24 🔖 Pre-sealed block for proposal at 2. Hash now 0xabdbbc7ac6193d349d33957fb9c227deb86c8be63fb603e73406ed53bd7a6cb6, previously 0x68c13cd562a5081777ebf42f944468b7faf550beae096e2c6eceec589b01c5be.

```

If the number after `finalized` is increasing, your blockchain is producing new blocks and reaching consensus about the state they describe.

Note that the current node has 0 peers in the network.

You can specify the folder where you want to store the genesis state as follows

```sh
./target/release/app-chain-node setup --chain dev --from-local ./configs --base-path=<path>
```

If you used a custom folder to store the genesis state, you need to specify it when running

```sh
./target/release/app-chain-node --base-path=<path>
```

Please note, Madara overrides the default dev flag in substrate to meet its requirements. The following flags are automatically enabled with the `--dev` argument:

```sh
--chain=dev, --force-authoring, --alice, --tmp, --rpc-external, --rpc-methods=unsafe
```

To store the chain state in the same folder as the genesis state, run the following command.

You cannot combine the base-path command with `--dev` as `--dev` enforces `--tmp` which will store the db at a temporary folder.

You can, however, manually specify all flags that the dev flag adds automatically. Keep in mind, the path must be the same as the one you used in the setup command.

```sh
./target/release/app-chain-node --base-path <path>
```

To start the development chain with detailed logging, run the following command:

```sh
RUST_BACKTRACE=1 ./target/release/app-chain-node -ldebug --dev
```

## TODO! Multi-Node Development Chain

## Pallet and Runtime Configuration

The current madara app template is configured as follows, in the `crates` directory:

```sh
.
├── node
│   ├── build.rs
│   ├── Cargo.toml
│   └── src
│       ├── benchmarking.rs
│       ├── chain_spec.rs
│       ├── cli.rs
│       ├── command.rs
│       ├── commands
│       │   ├── mod.rs
│       │   ├── run.rs
│       │   └── setup.rs
│       ├── configs.rs
│       ├── constants.rs
│       ├── genesis_block.rs
│       ├── main.rs
│       ├── rpc
│       │   ├── mod.rs
│       │   └── starknet.rs
│       ├── service.rs
│       └── starknet.rs
├── pallets
│   └── template
│       ├── Cargo.toml
│       └── src
│           ├── benchmarking.rs
│           ├── lib.rs
│           ├── mock.rs
│           └── tests.rs
└── runtime
    ├── build.rs
    ├── Cargo.toml
    └── src
        ├── config.rs
        ├── lib.rs
        ├── opaque.rs
        ├── pallets.rs
        ├── runtime_tests.rs
        └── types.rs

```

- The `node` directory contains essential files for building and configuring the blockchain node while integrating StarkNet functionalities. Key components include the build script `build.rs`, the node's configuration `Cargo.toml`, and source files that handle everything from benchmarking and chain specifications to custom RPC definitions and the node's main logic.
- The `pallets` directory is where you add your own custom pallets, with a `template` pallet provided as a starting point. Within each pallet, there's a `Cargo.toml` file for setting up dependencies. The source files include:

  - `lib.rs`: For the core logic
  - `benchmarking.rs`: For performance checks
  - `mock.rs` and `tests.rs`: For thorough testing

- The `runtime` directory holds your appchain's runtime logic. The source files include:

  - `config.rs`: Configuration settings for the runtime.
  - `lib.rs`: The main entry point for the runtime, typically includes the `construct_runtime!` macro to compose the runtime with various pallets.
  - `opaque.rs`: Defines opaque types, used for interoperability between the runtime and Substrate's node template. It allows the CLI and other external tools to interact with the blockchain without needing detailed knowledge of the runtime's internal specifics.
  - `pallets.rs`: Acts as the heart of runtime configuration, where we dial in the specifics for each pallet, from consensus details to how they mesh with Starknet.

  - `runtime_tests.rs`: Contains tests specific to the runtime.
  - `types.rs`: Custom types used within the runtime.

## Configuring the Starknet Pallet

Let's first have a quick overview of what `crates/runtime/src/pallets.rs` file contains:

```rust
//! Configuration of the pallets used in the runtime.
//! The pallets used in the runtime are configured here.
//! This file is used to generate the `construct_runtime!` macro.
pub use frame_support::traits::{
    ConstBool, ConstU128, ConstU32, ConstU64, ConstU8, KeyOwnerProofSystem, OnTimestampSet, Randomness, StorageInfo,
};
pub use frame_support::weights::constants::{
    BlockExecutionWeight, ExtrinsicBaseWeight, RocksDbWeight, WEIGHT_REF_TIME_PER_SECOND,
};
pub use frame_support::weights::{IdentityFee, Weight};
pub use frame_support::{construct_runtime, parameter_types, StorageValue};
pub use frame_system::Call as SystemCall;
pub use mp_chain_id::SN_GOERLI_CHAIN_ID;
/// Import the StarkNet pallet.
pub use pallet_starknet;
pub use pallet_timestamp::Call as TimestampCall;
use sp_consensus_aura::sr25519::AuthorityId as AuraId;
use sp_runtime::generic;
use sp_runtime::traits::{AccountIdLookup, BlakeTwo256};
#[cfg(any(feature = "std", test))]
pub use sp_runtime::BuildStorage;
pub use sp_runtime::{Perbill, Permill};
use sp_std::marker::PhantomData;

use crate::*;
```

The code snippet above first imports the various traits, constants and types necessary for runtime configuration, including those related to weights, system calls, and consensus mechanisms.

Note that `frame_support` simplifies pallet structure creation by providing Rust macros, types, traits, and modules that automate boilerplate code generation during compilation..

Conversely, `frame_system` lays out fundamental types for Substrate's primitives, storage items, and core blockchain functions. It's relied upon by all other pallets, granting access to critical data types and shared utilities essential for various node operations within the runtime.

```rust
// Configure FRAME pallets to include in runtime.

// --------------------------------------
// CUSTOM PALLETS
// --------------------------------------

/// Configure the Starknet pallet in pallets/starknet.
impl pallet_starknet::Config for Runtime {
    type RuntimeEvent = RuntimeEvent;
    type SystemHash = StarknetHasher;
    type TimestampProvider = Timestamp;
    type UnsignedPriority = UnsignedPriority;
    type TransactionLongevity = TransactionLongevity;
    #[cfg(not(feature = "disable-transaction-fee"))]
    type DisableTransactionFee = ConstBool<false>;
    #[cfg(feature = "disable-transaction-fee")]
    type DisableTransactionFee = ConstBool<true>;
    type DisableNonceValidation = ConstBool<false>;
    type InvokeTxMaxNSteps = InvokeTxMaxNSteps;
    type ValidateMaxNSteps = ValidateMaxNSteps;
    type ProtocolVersion = ProtocolVersion;
    type ChainId = ChainId;
    type MaxRecursionDepth = MaxRecursionDepth;
}
```

The provided code snippet configures the Starknet pallet within the runtime. The pallet allows the app chain to fine tune specific parameters to meet their own needs.

```rust
/// --------------------------------------
/// FRAME SYSTEM PALLET
/// --------------------------------------

/// Configuration of `frame_system` pallet.
impl frame_system::Config for Runtime {
    /// The basic call filter to use in dispatchable.
    type BaseCallFilter = frame_support::traits::Everything;
    /// Block & extrinsics weights: base values and limits.
    type BlockWeights = BlockWeights;
    /// The maximum length of a block (in bytes).
    type BlockLength = BlockLength;
    /// The identifier used to distinguish between accounts.
    type AccountId = AccountId;
    /// The aggregated dispatch type that is available for extrinsics.
    type RuntimeCall = RuntimeCall;
    /// The lookup mechanism to get account ID from whatever is passed in dispatchers.
    type Lookup = AccountIdLookup<AccountId, ()>;
    /// The index type for storing how many extrinsics an account has signed.
    type Index = Index;
    /// The index type for blocks.
    type BlockNumber = BlockNumber;
    /// The type for hashing blocks and tries.
    type Hash = Hash;
    /// The hashing algorithm used.
    type Hashing = BlakeTwo256;
    /// The header type.
    type Header = generic::Header<BlockNumber, BlakeTwo256>;
    /// The ubiquitous event type.
    type RuntimeEvent = RuntimeEvent;
    /// The ubiquitous origin type.
    type RuntimeOrigin = RuntimeOrigin;
    /// Maximum number of block number to block hash mappings to keep (oldest pruned first).
    type BlockHashCount = BlockHashCount;
    /// The weight of database operations that the runtime can invoke.
    type DbWeight = RocksDbWeight;
    /// Version of the runtime.
    type Version = Version;
    /// Converts a module to the index of the module in `construct_runtime!`.
    ///
    /// This type is being generated by `construct_runtime!`.
    type PalletInfo = PalletInfo;
    /// What to do if a new account is created.
    type OnNewAccount = ();
    /// What to do if an account is fully reaped from the system.
    type OnKilledAccount = ();
    /// The data to be stored in an account.
    type AccountData = ();
    /// Weight information for the extrinsics of this pallet.
    type SystemWeightInfo = ();
    /// This is used as an identifier of the chain. 42 is the generic substrate prefix.
    type SS58Prefix = SS58Prefix;
    /// The set code logic, just the default since we're not a parachain.
    type OnSetCode = ();
    type MaxConsumers = frame_support::traits::ConstU32<16>;
}

```

This snippet above configures the `frame_system` pallet within the runtime. It specifies various types and constants related to system operations, such as account identifiers, block headers, event types, and origin types.

Additionally, it defines parameters for system-level functionality, such as block hashing, account management, and extrinsic weight information.

```rust
// --------------------------------------
// CONSENSUS RELATED FRAME PALLETS
// --------------------------------------
// Notes:
// Aura is the consensus algorithm used for block production.
// Grandpa is the consensus algorithm used for block finalization.
// We want to support multiple flavors of consensus algorithms.
// Specifically we want to implement some proposals defined in the Starknet community forum.
// For more information see: https://community.starknet.io/t/starknet-decentralized-protocol-i-introduction/2671
// You can also follow this issue on github: https://github.com/keep-starknet-strange/madara/issues/83

/// Authority-based consensus protocol used for block production.
/// TODO: Comment and explain the rationale behind the configuration items.
impl pallet_aura::Config for Runtime {
    type AuthorityId = AuraId;
    type DisabledValidators = ();
    type MaxAuthorities = ConstU32<32>;
}

/// Deterministic finality mechanism used for block finalization.
/// TODO: Comment and explain the rationale behind the configuration items.
impl pallet_grandpa::Config for Runtime {
    type RuntimeEvent = RuntimeEvent;

    type WeightInfo = ();
    type MaxAuthorities = ConstU32<32>;
    type MaxSetIdSessionEntries = ConstU64<0>;

    type KeyOwnerProof = sp_core::Void;
    type EquivocationReportSystem = ();
}
```

This snippet configures the consensus-related FRAME pallets, namely `pallet_aura` and `pallet_grandpa`, which respectively handle block production and finalization.

For `pallet_aura`, it defines parameters such as the authority ID type and maximum number of authorities.

For `pallet_grandpa`, it specifies details like weight information, maximum authorities, and settings related to equivocation reports.

```rust
/// --------------------------------------
/// OTHER 3RD PARTY FRAME PALLETS
/// --------------------------------------

/// Timestamp manipulation.
/// For instance, we need it to set the timestamp of the Starknet block.
impl pallet_timestamp::Config for Runtime {
    /// A timestamp: milliseconds since the unix epoch.
    type Moment = u64;
    type OnTimestampSet = ConsensusOnTimestampSet<Self>;
    type MinimumPeriod = ConstU64<{ SLOT_DURATION / 2 }>;
    type WeightInfo = ();
}

parameter_types! {
    pub const UnsignedPriority: u64 = 1 << 20;
    pub const TransactionLongevity: u64 = u64::MAX;
    pub const InvokeTxMaxNSteps: u32 = 1_000_000;
    pub const ValidateMaxNSteps: u32 = 1_000_000;
    pub const ProtocolVersion: u8 = 0;
    pub const ChainId: Felt252Wrapper = SN_GOERLI_CHAIN_ID;
    pub const MaxRecursionDepth: u32 = 50;
}

/// Implement the OnTimestampSet trait to override the default Aura.
/// This is needed to suppress Aura validations in case of non-default sealing.
pub struct ConsensusOnTimestampSet<T>(PhantomData<T>);
impl<T: pallet_aura::Config> OnTimestampSet<T::Moment> for ConsensusOnTimestampSet<T> {
    fn on_timestamp_set(moment: T::Moment) {
        if Sealing::get() != SealingMode::Default {
            return;
        }
        <pallet_aura::Pallet<T> as OnTimestampSet<T::Moment>>::on_timestamp_set(moment)
    }
}

```

This code snippet configures the `pallet_timestamp` FRAME pallet, which handles timestamp manipulation within the runtime.


For configuration parameters you want to tweak to suit your needs, you normally tweak the Starknet pallet.

Some of the parameters to be tweaked include:

- `DisableTransactionFee`: If true, calculate and store the Starknet state commitments
- `DisableNonceValidation`: If true, check and increment nonce after a transaction
- `InvokeTxMaxNSteps`: Maximum number of Cairo steps for an invoke transaction
- `ValidateMaxNSteps`: Maximum number of Cairo steps when validating a transaction
- `MaxRecursionDepth`: Maximum recursion depth for transactions
- `ChainId`: The chain id of the app chain


## Adding a new pallet
To add a new pallet to our app chain template, we will use [Nicks pallet](https://paritytech.github.io/substrate/master/pallet_nicks/index.html) as an example.  The Nicks pallet allows blockchain users to pay a deposit to reserve a nickname for an account they control. 

It implements the following functions:

- The `set_name` function to collect a deposit and set the name of an account if the name is not already taken.
- The `clear_name` function to remove the name associated with an account and return the deposit.
- The `kill_name` function to forcibly remove an account name without returning the deposit.

## Add the Nicks Pallet Dependencies

Before you can use a new pallet, you must add some information about it to the configuration file that the compiler uses to build the runtime binary.

- Change into the root directory of the template
- Open the `Cargo.toml` configuration file in a text editor
- Locate the `[dependencies]` section and note how other pallets are imported.
- Copy an existing pallet dependency description and replace the pallet name with `pallet-nicks` to make the pallet available to the node template runtime.

The result should be like this:

```toml
# Substrate Frame pallet
pallet-aura = { default-features = false, git = "https://github.com/paritytech/substrate", branch = "polkadot-v0.9.43" }
pallet-grandpa = { default-features = false, git = "https://github.com/paritytech/substrate", branch = "polkadot-v0.9.43" }
pallet-timestamp = { default-features = false, git = "https://github.com/paritytech/substrate", branch = "polkadot-v0.9.43" }
pallet-nicks = { default-features = false, git = "https://github.com/paritytech/substrate", branch = "polkadot-v0.9.43" }

```


<p align="center">
  <a href="https://metaplex.com">
    <img alt="Metaplex" src="https://metaplex.com/meta.svg" width="250" />
  </a>
</p>

Metaplex is a protocol built on top of Solana that allows:

- **Creating/Minting** non-fungible tokens;
- **Starting** a variety of auctions for primary/secondary sales;
- and **Visualizing** NFTs in a standard way across wallets and applications.

Metaplex is comprised of two core components: an on-chain program, and a self-hosted front-end web3 application.

## In Depth Developer's Guide

If you want to deep dive on the Architecture, you can do so here:

https://www.notion.so/Metaplex-Developer-Guide-afefbc19841744c28587ab948a08cfac

## Installing

Clone the repo, and run `yarn start` to deploy.

```bash
$ git clone https://github.com/metaplex-foundation/metaplex.git
$ cd metaplex
$ cd js
$ yarn install
$ yarn bootstrap
$ yarn start
```

Navigate to `http://localhost:3000/` to explore the deployed application.

## Rust Programs

The Rust programs will soon be added to this repo with JavaScript
bindings that allow interactivity.

## Community

We have a few channels for contact:

- [Discord](https://discord.gg/metaplex)
- [@metaplex](https://twitter.com/metaplex) on Twitter
- [GitHub Issues](https://github.com/metaplex-foundation/metaplex/issues)

# Protocol

## Non-fungible tokens

Metaplex's non-fungible-token standard is a part of the Solana Program Library (SPL), and can be characterized as a unique token with a fixed supply of 1 and 0 decimals. We extended the basic definition of an NFT on Solana to include additional metadata such as URI as defined in ERC-721 on Ethereum.

Below are the types of NFTs that can be created using the Metaplex protocol.

### **Master Edition**

A master edition token, when minted, represents both a non-fungible token on Solana and metadata that allows creators to control the provenance of prints created from the master edition.

Rights to create prints are tokenized itself, and the owner of the master edition can distribute tokens that allow users to create prints from master editions. Additionally, the creator can set the max supply of the master edition just like a regular mint on Solana, with the main difference being that each print is a numbered edition created from it.

A notable and desirable effect of master editions is that as prints are sold, the artwork will still remain visible in the artist's wallet as a master edition, while the prints appear in the purchaser's wallets.

### **Print**

A **print** represents a copy of an NFT, and is created from a Master Edition. Each print has an edition number associated with it.

Usually, prints are created as a part of an auction that has happened on Metaplex, but they could also be created by the creator manually.

For limited auctions, each print number is awarded based on the bid placement.

Prints can be created during [Open Edition](#open-edition) or [Limited Edition](#limited-edition) auction.

### Normal NFT

A normal NFT (like a Master Edition) when minted represents a non-fungible token on Solana and metadata, but lacks rights to print.

An example of a normal NFT would be an artwork that is a one-of-a-kind that, once sold, is no longer within the artist's own wallet, but is in the purchaser's wallet.

## Types of Auctions

Metaplex currently supports four types of auctions that are all derived from English auctions.

Basic parameters include:

- Auction start time
- Auction end time
- Reservation price

Additionally, Metaplex includes a novel concept of the participation NFT. Each bidding participant can be rewarded a unique NFT for participating in the auction.

The creator of an auction also has the ability to configure a minimal price that should be charged for redemption, with the option to set it as "free".

### Single Item

This type of auction can be used to sell normal NFTs and re-sell Prints, as well as the sale of Master Edition themselves (and the associated printing rights) if the artist so wishes. While this last behavior is not exposed in the current UI, it does exist in the protocol.

### Open Edition

An open edition auction requires the offering of a Master Edition NFT that specifically has no set supply. The auction will only create Prints of this item for bidders: each bidder is guaranteed to get a print, as there are no true "winners" of this auction type.

An open edition auction can either have a set fixed price (equivalent to a Buy Now sale), can be set to the bid price (Pay what you want), or can be free (Make any bid to get it for free).

### Limited Edition

For a limited edition auction, a Master Edition NFT (of limited or unlimited supply) may be provided to the auction with a number of copies as the set amount of winning places.

For each prize place, a Print will be minted in order of prize place, and awarded to the winning bidder of that place.

For example, the first place winner will win Print #1; the second place winner Print #2; and so on.

It is required for limited supply NFTs that there is at least as much supply remaining as there are desired winners in the auction.

### Tiered Auction

A tiered auction can contain a mix of the other three auction types as winning placements. For instance, the first place winner could win a Print of Limited Edition NFT A, while the second-place winner could win Normal NFT, and so on. Additionally, all participants who did not win any place could get a Participation NFT Print from a Master Edition (if the Master Edition had no supply limit).

## Royalties

Metaplex can seamlessly create on-chain artist splits that remove the awkwardness out of collaboration.

Tag each collaborator, set custom percentages, and you’re off to the races. Each NFT can also be minted with configurable royalty payments that are then sent automatically back to the original creators whenever an artwork is resold on a Metaplex marketplace in the future.

## Storefronts

Metaplex's off-chain component allows creators to launch a custom storefront, similar to Shopify or WordPress. This open-source project provides a graphical interface to the on-chain Metaplex program, for creators, buyers, and curators of NFTs. The design and layout of storefronts can be customized to suit the needs of the entity creating it, either as a permanent storefront or an auction hub for a specific auction or collection.

All identification on the Storefront is based on wallet addresses. Creators and store admins sign through their wallets, and users place bids from connected wallets. Custom storefronts allow creators to create unique experiences per auction. Additionally, the Metaplex Foundation is working on multiple partnerships that will enable building immersive storefronts using VR/AR.


## Using the CLI

If you have created the master using a wallet (phantom) with metaplex, the mint authority of the master is the wallet address, so this mint authority keypair must be transferred to a file wallet so the CLI can use it.

./target/debug/spl-token-metadata-test-client --keypair ~/.console/solana/id.json mint_new_edition_from_master_edition_via_token --mint <MINT ADDRESS OF MASTER> --url https://api.metaplex.solana.com/
 ./target/debug/spl-token-metadata-test-client show --keypair ~/.console/solana/id.json  --mint <MINT ADDRESS> --url https://api.metaplex.solana.com/

## Transferring a phantom wallet to the CLI file wallet.

Go to phantom wallet, settings, Export private key (it is base58 :s)
2. https://www.dcode.fr/base-58-cipher convert to decimal, then insert comma (use python ",".join("107 226 38 36 208 86 37 .. ".split(' ')) )
3. create keypair file .json with [121, 1, 123, ..., 154]
4. verify pubkey matches with solana-keygen pubkey <KEYPAIR_FILE>



## Mint Copy of Day 1 Pioneer NFT
./target/debug/spl-token-metadata-test-client --keypair ~/phantom_wallet.json mint_new_edition_from_master_edition_via_token --mint 7xPah9bPtBwEkFWvnBL3jLAGi7gSUpjjnar9oMmtjCu6 --url https://api.metaplex.solana.com/

## Mint Copy of Day 1 Team NFT
./target/debug/spl-token-metadata-test-client --keypair ~/phantom_wallet.json mint_new_edition_from_master_edition_via_token --mint 7Vg4S1yuFPziZaBcpNyB8LDmKiUBW11WSxfYcU1jwF4Z --url https://api.metaplex.solana.com/

## Mint Copy of Day 1 Standard NFT
./target/debug/spl-token-metadata-test-client --keypair ~/phantom_wallet.json mint_new_edition_from_master_edition_via_token --mint 6hakUtw98W5S7tpEfNMNBVmU8uup8cu2z5CxB46592An --url https://api.metaplex.solana.com/

## Mint Copy of Mercurial x Terra
./target/debug/spl-token-metadata-test-client --keypair ~/phantom_wallet.json mint_new_edition_from_master_edition_via_token --mint 4pNnsWJJRRWbbL1Sur42dfKi8WrXPvfgSbUWRKBrHm5W --url https://api.metaplex.solana.com/

## Mint Copy of Mercurial x Parrot
./target/debug/spl-token-metadata-test-client --keypair ~/phantom_wallet.json mint_new_edition_from_master_edition_via_token --mint 4kMJVcVuoxNPZaiQbxno46ebtvW81rFSXKy2BfpHWV3h --url https://api.metaplex.solana.com/

## Mint Copy of danku_r
./target/debug/spl-token-metadata-test-client --keypair ~/phantom_wallet.json mint_new_edition_from_master_edition_via_token --mint AHtzwxL5jHyCKmnq3X6SyETHEy12gLW1mfT5FwucQRfC --url https://api.metaplex.solana.com/

## Mint Copy of Dead Eye Degen
./target/debug/spl-token-metadata-test-client --keypair ~/phantom_wallet.json mint_new_edition_from_master_edition_via_token --mint 2LQS4NDJdUPfPFAbXz9Ejgo1CFUN2R43jQhfhKKHbmxR --url https://api.metaplex.solana.com/


## show metadata
./target/debug/spl-token-metadata-test-client show --keypair ~/phantom_wallet.json  --mint CMqFN86io8zra3J5zawfzEP3unyHAnyJAg55zZ2tgnXT --url https://api.metaplex.solana.com/

## guide to metaplex candy machine
https://hackmd.io/KZZxAt3BSr6L75JRGCQmDw?view
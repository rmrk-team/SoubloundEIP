---
eip: x
title: Minimalistic Souldbound interface for NFTs
description: An interface for Soulbound Non-Fungible Tokens extension allowing for tokens to be non-transferrable.
author: Bruno Škvorc (@Swader), Francesco Sullo(@sullof), Steven Pineda (@steven2308), Stevan Bogosavljevic (@stevyhacker), Jan Turk (@ThunderDeliverer)
discussions-to: https://ethereum-magicians.org/t/minimalistic-transferable-interface/12517
status: Draft
type: Standards Track
category: ERC
created: 2023-01-31
requires: 165, 721
---

## Abstract

The Minimalistic Souldbound interface for Non-Fungible Tokens standard extends [EIP-721](./eip-721.md) by preventing NFTs to be transferred.

This proposal introduces the ability to prevent a token to be transferred from their owner, making them bound to the externally owned account, smart contract or token that owns it.

## Motivation

With NFTs being a widespread form of tokens in the Ethereum ecosystem and being used for a variety of use cases, it is time to standardize additional utility for them. Having the ability to prevent the tokens to be transferred introduces new possibilities of NFT utility and evolution.

This proposal is designed in a way to be as minimal as possible in order to be compatible with any usecases that wish to utilize this proposal.

This EIP introduces new utilities for [EIP-721](./eip-721.md) based tokens in the following areas:

- [Verifiable attribution](#verifiable-attribution)
- [Immutable properties](#immutable-properties)

### Verifiable attribution

Personal achievements can be represented by non-fungible tokens. These tokens can be used to represent a wide range of accomplishments, including scientific advancements, philanthropic endeavors, athletic achievements, and more. However, if these achievement-indicating NFTs can be easily transferred, their authenticity and trustworthiness can be called into question. By binding the NFT to a specific account, it can be ensured that the account owning the NFT is the one that actually achieved the corresponding accomplishment. This creates a secure and verifiable record of personal achievements that can be easily accessed and recognized by others in the network. The ability to verify attribution helps to establish the credibility and value of the achievement-indicating NFT, making it a valuable asset that can be used as a recognition of the holder's accomplishments.

### Immutable properties

NFT properties are a critical aspect of non-fungible tokens, serving to differentiate them from one another and establish their scarcity. Centralized control of NFT properties by the issuer, however, can undermine the uniqueness of these properties.

By tying NFTs to specific properties, the original owner is ensured that the NFT will always retain these properties and its uniqueness.

In a blockchain game that employs soulbound NFTs to represent skills or abilities, each skill would be a unique and permanent asset tied to a specific player or token. This would ensure that players retain ownership of the skills they have earned and prevent them from being traded or sold to other players. This can increase the perceived value of these skills, enhancing the player experience by allowing for greater customization and personalization of characters.

## Specification

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

```solidity
/// @title EIP-x Minimalistic Souldbound interface for NFTs
/// @dev See https://eips.ethereum.org/EIPS/eip-x
/// @dev Note: the ERC-165 identifier for this interface is 0x0.

pragma solidity ^0.8.16;

interface IERCx is IERC165 {
    /**
     * @notice Used to check whether the given token is soulbound or not.
     * @dev If this function returns `true`, the transfer of the token MUST revert execution
     * @param tokenId ID of the token being checked
     * @return Boolean value indicating whether the given token is soulbound
     */
    function isSoulbound(uint256 tokenId) external view returns (bool);
}
```

## Rationale

Designing the proposal, we considered the following questions:

1. **Should we propose another Soulbound NFT proposal given the existence of existing ones, some even final, and how does this proposal compare to them?**\
   This proposal aims to provide the minimum necessary specification for the implementation of soulbound NFTs, we feel none of the existing proposals have presented the minimal required interface. Unlike other proposals that address the same issue, this proposal requires fewer methods in its specification, providing a more streamlined solution.
2. **Why is there no event marking the token as Soulbound in this interface?**\
   The token can become soulbound either at its creation, after being marked as soulbound, or after a certain condition is met. This means that some cases of tokens becoming soulbound cannot emit an event, such as if the token becoming soulbound is determined by a block number. Requiring an event to be emitted upon the token becoming soulbound is not feasible in such cases.
3. **Should the soulbound state management function be included in this proposal?**\
   A function that marks a token as soulbound or releases the binding is referred to as the soulbound management function. To maintain the objective of designing an agnostic soulbound proposal, we have decided not to specify the soulbound management function. This allows for a variety of custom implementations that require the tokens to be non-transferable.

## Backwards Compatibility

The Emotable token standard is fully compatible with [EIP-721](./epi-721.md) and with the robust tooling available for implementations of EIP-721 as well as with the existing EIP-721 infrastructure.

## Test Cases

Tests are included in [`soulbound.ts`](../assets/eip-x/test/soulbound.ts).

To run them in terminal, you can use the following commands:

```
cd ../assets/eip-x
npm install
npx hardhat test
```

## Reference Implementation

See [`Soulbound.sol`](../assets/eip-x/contracts/Soulbound.sol).

## Security Considerations

The same security considerations as with [EIP-721](./eip-721.md) apply: hidden logic may be present in any of the functions, including burn, add asset, accept asset, and more.

Caution is advised when dealing with non-audited contracts.

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).

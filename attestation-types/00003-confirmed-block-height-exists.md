# 00003 - Confirmed Block Height Exists

- id: 3
- name: `ConfirmedBlockHeightExists`
- [Definition file](https://github.com/flare-foundation/attestation-client/blob/main/lib/verification/attestation-types/t-00003-confirmed-block-height-exists.ts)

## Description

The purpose of this attestation type is to prove that a block on a certain height exists and it is confirmed.
The attestation uses `upperBoundProof` as a hash of the confirmation block for the highest confirmed block in the query.

A successful attestation is provided by providing the following data:

- Block number
- Block timestamp
- Number of confirmations used
- Average block production time

## Request format

| Name                   | Size (bytes) | Internal type      | Description                                                                  |
| ---------------------- | ------------ | ------------------ | ---------------------------------------------------------------------------- |
| `attestationType`      | 2            | `AttestationType`  | Attestation type id for this request, see `AttestationType` enum.            |
| `sourceId`             | 4            | `SourceId`         | The ID of the underlying chain, see `SourceId` enum.                         |
| `messageIntegrityCode` | 32           | `ByteSequenceLike` | The hash of the expected attestation response appended by string 'Flare'. Used to verify consistency of the attestation response against the anticipated result, thus preventing wrong (forms of) attestations. |
| `blockNumber`          | 4            | `NumberLike`       | Block number to be proved to be confirmed.                         |
| `queryWindow`          | 4            | `NumberLike`       | Period in seconds considered for sampling block production. The block with number `lowestQueryWindowBlockNumber` in the attestation response is defined as the last block with the timestamp strictly smaller than `block.timestamp - queryWindow`.|

## Verification rules

If the block with the number `blockNumber` is confirmed the block number and the timestamp are provided in the response. In addition, the number of confirmations that were used to determine the confirmation is provided. The block with number `lowestQueryWindowBlockNumber` in the attestation response is defined as the last block with the timestamp strictly smaller than `block.timestamp - queryWindow`. Attestation is valid only if this block can be obtained.

## Response format

| Name                              | Type         | Description                                                          |
| --------------------------------- | ------------ | -------------------------------------------------------------------- |
| `blockNumber`                     | `uint64`     | Number of the transaction block on the underlying chain.             |
| `blockTimestamp`                  | `uint64`     | Timestamp of the transaction block on the underlying chain.          |
| `numberOfConfirmations`           | `uint8`      | Number of confirmations for the blockchain.                          |
| `lowestQueryWindowBlockNumber`    | `uint64`     | Lowest query window block number.                                    |
| `lowestQueryWindowBlockTimestamp` | `uint64`     | Lowest query window block timestamp.                                 |

Next: [00004 - Referenced Payment Nonexistence](./00004-referenced-payment-nonexistence.md)

[Back to Home](../README.md)

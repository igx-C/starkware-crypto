# starkware-crypto [![npm version](https://badge.fury.io/js/starkware-crypto.svg)](https://badge.fury.io/js/starkware-crypto)

Starkware Crypto Library

## Description

This library is a port from [starkex-resources/\*\*/signature.js](https://github.com/starkware-libs/starkex-resources/blob/master/crypto/starkware/crypto/signature/signature.js).

## Example

```typescript
import * as starkwareCrypto from 'starkware-crypto';

const privateKey =
  '0x659d82c1cc4c3e6fead938999322116a3dc7854b415b822dbea42630ecd90b5e';

const keyPair = starkwareCrypto.getKeyPair(privateKey);

const publicKey = starkwareCrypto.getPublic(keyPair);

const starkKey = starkwareCrypto.getStarkKey(publicKey);

const msgParams = {
  amount: '2154549703648910716',
  nonce: '1',
  senderVaultId: '34',
  token: {
    type: 'ETH' as starkwareCrypto.TokenTypes,
    data: {
      quantum: '1',
      tokenAddress: '0x89b94e8C299235c00F97E6B0D7368E82d640E848',
    },
  },
  receiverVaultId: '21',
  receiverPublicKey:
    '0x5fa3383597691ea9d827a79e1a4f0f7949435ced18ca9619de8ab97e661020',
  expirationTimestamp: '438953',
};

const message = starkwareCrypto.getTransferMsg(
  msgParams.amount,
  msgParams.nonce,
  msgParams.senderVaultId,
  msgParams.token,
  msgParams.receiverVaultId,
  msgParams.receiverPublicKey,
  msgParams.expirationTimestamp
);

const signature = starkwareCrypto.sign(keyPair, message);

const verified = starkwareCrypto.verify(keyPair, message, signature);
```

## API

```typescript
interface StarkwareCrypto {
  getKeyPairFromPath(seed: string, path: string): KeyPair;

  getKeyPair(privateKey: string): KeyPair;

  getStarkKey(publicKey: string): string;

  getPrivate(keyPair: KeyPair): string;

  getPublic(keyPair: KeyPair): string;

  hashTokenId(token: Token);

  hashMessage(w1: string, w2: string, w3: string);

  deserializeMessage(serialized: string): MessageParams;

  serializeMessage(
    instructionTypeBn: BN,
    vault0Bn: BN,
    vault1Bn: BN,
    amount0Bn: BN,
    amount1Bn: BN,
    nonceBn: BN,
    expirationTimestampBn: BN
  ): string;

  formatMessage(
    instruction: 'transfer' | 'order',
    vault0: string,
    vault1: string,
    amount0: string,
    amount1: string,
    nonce: string,
    expirationTimestamp: string
  ): string;

  getLimitOrderMsg(
    vaultSell: string,
    vaultBuy: string,
    amountSell: string,
    amountBuy: string,
    tokenSell: Token,
    tokenBuy: Token,
    nonce: string,
    expirationTimestamp: string
  ): string;

  getTransferMsg(
    amount: string,
    nonce: string,
    senderVaultId: string,
    token: Token,
    receiverVaultId: string,
    receiverPublicKey: string,
    expirationTimestamp: string
  ): string;

  sign(keyPair: KeyPair, msg: string): Signature;

  verify(keyPair: KeyPair, msg: string, sig: SignatureInput): boolean;
}
```

## Typings

```typescript
type KeyPair = elliptic.ec.KeyPair;

type MessageParams = {
  instructionTypeBn: BN;
  vault0Bn: BN;
  vault1Bn: BN;
  amount0Bn: BN;
  amount1Bn: BN;
  nonceBn: BN;
  expirationTimestampBn: BN;
};

class Signature {
  r: BN;
  s: BN;
  recoveryParam: number | null;

  constructor(options: SignatureInput, enc?: string);

  toDER(enc?: string | null): any;
}

interface SignatureOptions {
  r: BNInput;
  s: BNInput;
  recoveryParam?: number;
}

type BNInput =
  | string
  | BN
  | number
  | Buffer
  | Uint8Array
  | ReadonlyArray<number>;

type SignatureInput =
  | Signature
  | SignatureOptions
  | Uint8Array
  | ReadonlyArray<number>
  | string;
```

## License

[Apache License 2.0](LICENSE.md)

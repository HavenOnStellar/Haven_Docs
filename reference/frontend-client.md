# Frontend Client

The frontend client lives in:

```text
src/app/lib/havenClient.ts
```

It currently provides configuration, utility functions, and contract interaction stubs.

## Network configuration

The client defines testnet and mainnet configuration values. A planned improvement is environment-based network selection.

## Contract ID

The client currently needs to be configured with the deployed Haven Registry contract ID.

Current documented testnet contract:

```text
CAT2TDBXGW6GETW52MQB725PLWN2CBVO3TXJ7PRJ73YSKLHRA7SRN6FC
```

## hashIMEI

```ts
hashIMEI(imei: string): Promise<string>
```

Hashes an IMEI using SHA-256 and returns a hex string.

## registerDevice

```ts
registerDevice(hashedImei: string, deviceModel: string): Promise<void>
```

Stub for registering a device.

## reportStolen

```ts
reportStolen(hashedImei: string, bountyAmount: number, recoveryContact: string): Promise<void>
```

Stub for marking a device stolen.

## confirmRecovery

```ts
confirmRecovery(hashedImei: string, finderAddress: string): Promise<void>
```

Stub for confirming recovery.

## getDeviceStatus

```ts
getDeviceStatus(hashedImei: string): Promise<DeviceStatus | null>
```

Stub for reading device state.

# Vendor Verification

Vendor verification lets secondary-market participants check whether a device is clean before buying it.

## Goal

Reduce stolen-device resale by making device status easy to verify.

## Planned frontend route

```text
/verify
```

## Verification flow

1. Vendor enters a device identifier or hashed IMEI.
2. The frontend hashes the IMEI if needed.
3. The frontend queries the contract using `get_device` or a safer optional lookup function.
4. The UI displays whether the device is registered, stolen, recovered, or claimed by insurance.

## UX states

The verification page should support:

- Empty state
- Loading state
- Not found state
- Registered and clean state
- Stolen state
- Insurance-claimed state
- Error state

## Current status

The vendor verification page is planned. The frontend has an issue open for scaffolding this route.

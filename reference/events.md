# Events

Contract events make Haven easier to index, monitor, and connect to frontend applications.

## Planned events

The current contract code includes TODOs for the following events.

## DeviceRegistered

Emitted when a device is registered.

Suggested payload:

- hashed IMEI
- owner address
- device model or device state summary

## DeviceStolen

Emitted when a device is reported stolen.

Suggested payload:

- hashed IMEI
- owner address
- bounty amount

## DeviceRecovered

Emitted when a stolen device is recovered.

Suggested payload:

- hashed IMEI
- finder address
- bounty amount

## InsuranceClaimed

Emitted when an insurance claim is filed.

Suggested payload:

- hashed IMEI
- owner address
- insurer address

## Notes for contributors

When implementing events, keep event names and payload formats stable. Frontend pages, notification services, and indexers may depend on them.

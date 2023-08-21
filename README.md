# esp-openthread - WORK IN PROGRESS

This is using the [Rust open source IEEE-802.15.4 driver](https://github.com/esp-rs/esp-ieee802154) and [OpenThread](https://openthread.io/) to provide a Thread implementation for ESP32 bare-metal Rust.

The OpenThread libraries are pre-compiled for convenience. Find the build script in `build_openthread`

## Goals

- Provide MTD functionality.

## May-be later goals

- Provide sleepy end-device support
- Provide FTD functionality

## Non-Goals

- BR functionality

## Status

This is only tested on ESP32-C6. Only very basic functionality is tested.

There are no Rust-wrappers yet. You need to use the raw bindings. (like the example does)

## Testing

Build and flash the [OT-CLI](https://github.com/espressif/esp-idf/tree/master/examples/openthread/ot_cli) on ESP32-C6 or ESP32-H2.

```
> dataset set active 0e080000000000010000000300000b35060004001fffe002083a90e3a319a904940708fd1fa298dbd1e3290510fe0458f7db96354eaa6041b880ea9c0f030f4f70656e5468726561642d35386431010258d10410888f813c61972446ab616ee3c556a5910c0402a0f7f8

> dataset active
Active Timestamp: 1
Channel: 11
Channel Mask: 0x07fff800
Ext PAN ID: 3a90e3a319a90494
Mesh Local Prefix: fd1f:a298:dbd1:e329::/64
Network Key: fe0458f7db96354eaa6041b880ea9c0f
Network Name: OpenThread-58d1
PAN ID: 0x58d1
PSKc: 888f813c61972446ab616ee3c556a591
Security Policy: 672 onrc
Done

> ifconfig up

> thread start

```

Flash the example to an ESP32-C6. It should output something like
```
otInstanceInitSingle done....
otSetStateChangedCallback 0
otDatasetSetActive 0
otIp6SetEnabled 0
otThreadSetEnabled 0
Link local address fe80:0000:0000:0000:8cfb:9cc1:bc88:c204
change_callback !!!!!!!!!!!!    10001000001111101001100011101
change_callback !!!!!!!!!!!!   100000000000000000001010100100
```

Please note the link-local address.

Back in the OT-CLI ping the device (using the address from above)
```
> ping fe80:0000:0000:0000:8cfb:9cc1:bc88:c204

16 bytes from fe80:0:0:0:8cfb:9cc1:bc88:c204: icmp_seq=28 hlim=64 time=19ms
1 packets transmitted, 1 packets received. Packet loss = 0.0%. Round-trip min/avg/max = 19/19.0/19 ms.
Done
```

So it connected and you can successfully ping the device 🎉
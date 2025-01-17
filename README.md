# esp-openthread - WORK IN PROGRESS

This is using the [Rust open source IEEE-802.15.4 driver](https://github.com/esp-rs/esp-ieee802154) and [OpenThread](https://openthread.io/) to provide a Thread implementation for ESP32 bare-metal Rust.

The OpenThread libraries are pre-compiled for convenience. Find the build script in `build_openthread`

## Goals

- Provide MTD functionality.

## Maybe later goals

- Provide sleepy end-device support
- Provide FTD functionality

## Non-Goals

- BR functionality

## Status

Currently only basic functionality is tested.

There are only few Rust-wrapper. Until everything is wrapped you need to use the raw bindings for more advanced functionality.

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

Flash the example to an ESP32-C6 or ESP32-H2 - use a feature for choosing the build-target.

It should output something like
```
Initializing
Currently assigned addresses
fdde:ad00:beef:0:f9ee:5d6d:7fe6:daab
fe80::6cec:6ace:f5ff:30bc

ChangedFlags(Ipv6AddressAdded | ThreadRoleChanged | ThreadLlAddressChanged | ThreadMeshLocalAddressChanged | ThreadKeySequenceChanged | ThreadNetworkDataChanged | Ipv6MulticastSubscribed | ThreadPanIdChanged | ThreadNetworkNameChanged | ThreadExtendedPanIdChanged | ThreadNetworkKeyChanged | ThreadNetworkInterfaceStateChanged | ActiveDatasetChanged)
Currently assigned addresses
fdde:ad00:beef:0:f9ee:5d6d:7fe6:daab
fe80::6cec:6ace:f5ff:30bc

ChangedFlags(ThreadRoleChanged | ThreadRlocAdded | ThreadPartitionIdChanged | ThreadNetworkDataChanged | PendingDatasetChanged)
Currently assigned addresses
fdde:ad00:beef::ff:fe00:8019
fdde:ad00:beef:0:f9ee:5d6d:7fe6:daab
fe80::6cec:6ace:f5ff:30bc
```

Please note the link-local address. (fe80::...)

Back in the OT-CLI ping the device (using the address from above)
```
> ping fe80::6cec:6ace:f5ff:30bc

16 bytes from fe80:0:0:0:906d:1cce:1bc9:8d07: icmp_seq=15 hlim=64 time=13ms
1 packets transmitted, 1 packets received. Packet loss = 0.0%. Round-trip min/avg/max = 13/13.0/13 ms.
Done
```

Now send and receive a UDP message
```
> udp open

Done
> udp bind :: 1212

Done
> udp send fe80::6cec:6ace:f5ff:30bc 1212 Hello

Done
5 bytes from fe80::6cec:6ace:f5ff:30bc 1212 Hello
```

So it connected and you can successfully ping the device. Receiving and sending UDP packets also works. 🎉

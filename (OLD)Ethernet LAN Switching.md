---
tags:
  - CCNA
---
- **Ethernet Header Summary**
	- **Preamble**
	- **SFD (Start Frame Delimiter):** are used for synchronization and to allow the receiving device to be prepared to receive the rest of the data in the frame.
	- **Destination**: the layer 2 address to which the frame is being sent
	- **Source**: the layer 2 address of the device which sent the frame
	- **Type**: indicates the layer 3 protocol used in the encapsulated packet, which is almost always Internet Protocol, IP v4 or v6.
		- Sometimes this is a length field, indicating the length of the encapsulated data, depending on the version of ethernet
- **Ethernet Trailer Summary**
	- **FCS (frame check sequence):** used by the receiving device to detect any errors that might have occurred in transmission.  

![](_anexos_/Screenshot%20from%202023-11-22%2008-46-30.png)

#### Preamble & SFD 
- **Preamble**
	- Length: 7 bytes (56 bits)
	- Pattern: 10101010 * 7
	- Allow devices to synchronize their receiver clocks, make sure they're ready to receive the rest of the frame and the data inside. 
- **SFD (Start Frame Delimiter)**
	- Length: 1 byte (8 bits)
	- Pattern: 10101011 (similar to each byte of the preamble but the last bit is a 1, not a 0)
	- Indicates the end of the preamble, and the beginning of the rest of the frame
#### Destination & Source
- Indicate the devices sending and receiving the frame
- Consist of the destination and source 'MAC Address'
- MAC = Media Access Control
- 6 bytes (48 bits) address of the physical device
#### Type / Length
- 2 bytes (16 bits) field
- A value of **1500 or less** in this field indicates the **LENGTH** of the encapsulated packet (in bytes)
	- For example, if the value in this field is 1400, it means that the encapsulated packet is 1400 bytes in length
- A value of **1536 or greater** in this field  indicates the **TYPE** of the encapsulated packet (usually IPv4 or IPv6), and the length is determined via other methods.
``` text
IPv4 = 0x0800 (hexadecimal) IPv6 = 0x86DD (hexadecimal)
(2048 in decimal)           (34525 in decimal)
```

![](_anexos_/Screenshot%20from%202023-11-22%2018-19-11.png)

#### Frame Check Sequence (FCS)
- 4 bytes (32 bits) in length
- Detects corrupted data by running a "CRC" algorithm over the received data
	- `CRC = "Cyclic Redundancy Check"`
		- Cyclic refers to `cyclic codes`
		- Redundancy refers to the fact that these 4 bytes at the end of the message enlarge the message without adding any new information
		- Check refers to the fact that is `CHECKS or Verifies` the data for errors
> Remember that the ethernet frame, Frame Check Sequence is a Cyclic Redundancy Check

![](_anexos_/Screenshot%20from%202023-11-22%2018-47-15.png)
> Focus in source and destination MAC address filed

### MAC address
- A.K.A `Burned-In Address (BIA)`
- 6 bytes (48 bits) physical address assigned to the device when it is made
- Is globally unique
- The first 3 bytes are the OUI (Organizationally Unique Identifier), which is assigned to the company making the device
- The last 3 bytes are unique to the device itself
- Written as 12 **hexadecimal** characters
- **Unicast frame:** a frame destined for a single target
- Associate Interface and MAC address is known are **Dynamic MAC address**
- **Unknown unicast frame**, a frame for which the switch  doesn't have an entry in its MAC address table. 
	- **FLOOD**, means to forward the frame out of ALL of its interfaces, except the one it received the packet on.
		- The switch SW1 copies the frame and sends it out its F0/2 and F0/3 interfaces
		 ![](_anexos_/Screenshot%20from%202023-11-22%2019-26-48.png)
	 - PC3 ignores the packet, because the destination MAC address doesn't match its  own MAC address
- If PC2 want send some data to PC1, maybe a reply to what PC1 sent. _The destination and source address of the frame are reversed_.
	- It doesn't flood the frame, **KNOWN UNICAST FRAME**. The destination is already in its MAC address table.
- Whereas UNKNOWN unicast frames are flooded, KNOWN unicast frames are simply forwarded to the destination.
- On the **Cisco Switches**, dynamic MAC addresses are removed from The MAC address table after **5 minutes** of inactivity.

> The **Interface** in the MAC address table, it doesn't mean the device is directly connected to this interface.

![](_anexos_/Screenshot%20from%202023-11-23%2001-22-08.png)
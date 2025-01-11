# CRC16-XMODEM

<small>Generate CRC-16/XMODEM codes consistently</small>

## Why

While a fairly simple task, I was implementing this throughout multiple projects
of mine. Now if there's a bug, speed improvement to be had, or simple because
typescript's typing system changes, I want to be able to update it in 1 location
instead of having to fix it in every individual project.

TL;DR; I'm lazy, and want a single version of the code

## Usage

This package does only one thing.
Usage is really simple:

```ts
import { crc16, crc16b } from '@finwo/crc16-xmodem';

// Note the 2 zero bytes at the end, they're useful later
const message = Buffer.from('Hi!\0\0');

// Output 12797 and <Buffer 31 fd>
console.log(crc16(message));
console.log(crc16b(message));

// Either copy in the check code with buffer manipulation:
const check            = crc16b(message);
const checkableMessage = Buffer.concat([message]);
check.copy(checkableMessage, checkableMessage.length - check.length);

// Or, be lazy and just copy the crc over the zeroes directly as an uint16be
message.writeUInt16BE(crc16(message), message.length - 2);

// Now if you want to check if your message has travelled the network without errors:
if (crc16(message) != 0) {
  throw new Error('Message got corrupted in transit');
}
```

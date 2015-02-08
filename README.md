# keccak-cryptol
keccak implementation in cryptol

## legal notes

this work is in the public domain. no rights reserved.

if you want to blame anyone, you can blame:
Krisztián Pintér, pinterkr@gmail.com 

## examples

to calculate SHA3-256 as of FIPS202 May-2014:

    to_bytes ( sha3`{256} (0:[0]) )

this will give you:

    [0xa7, 0xff, 0xc6, 0xf8, 0xbf, 0x1e, 0xd7, 0x66, 0x51, 0xc1, 0x47,
     0x56, 0xa0, 0x61, 0xd6, 0x62, 0xf5, 0x80, 0xff, 0x4d, 0xe4, 0x3b,
     0x49, 0xfa, 0x82, 0xd8, 0x0a, 0x4b, 0x80, 0xf8, 0x43, 0x4a]

to calculate SHAKE-128, use

    take`{16} (to_bytes (shake`{128} (to_bits "the quick brown fox jumps over the lazy dog")))

and you get:

    [0x9b, 0x1b, 0x08, 0x5d, 0x51, 0x60, 0xbc, 0xce, 0xcb, 0x04, 0x9b,
     0xa5, 0xf4, 0x86, 0xd0, 0xa8]

there are low level functions as well. you can calculate 200 bit keccak sponge with 192 bit capacity:

    join sponge`{200, 192} (split 0x123456789abcdef0)

which gives you:

    [0xd0, 0xcd, 0xd4, 0x5f, 0xf8, ...]

please note that this looks like a byte list, but in fact it is the output block list of the sponge, 
which happens to be 8 bit each, since 200-192=8.

## bit order

the bit order is little endian as defined in the reference. that is, the number
0xf1 is represented as 10001111. if you want to use the regular bit order, you
need to transform your byte stream back and forth with to_bits and to_bytes. 
or you can construct a state immediately using state_of_bytes and state_of_bits, 
and convert a state to streams with state_bytes and state_bits.

## reference and fast versions

numerous functions are defined in two versions, one verbatim from the 
reference, and one optimized. the reference versions are marked with a 
prime. the fast versions are used in higher level functions.

## properties

some properties are provided to check the implementation. in particular
all opimized versions are checked against the reference implementations.
beware, some of the properties will probably be not provable in reasonable
time and memory. you can check them though.

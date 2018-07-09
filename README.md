
# Difficulty Targets

Target is a bit hard to work with. We know that this is the number that the hash must be below, but as humans, it's hard to fathom the difference between a 180-bit number and a 190-bit number. The first is a thousand times smaller, but from looking at targets, it's not easy to contextualize.

To make different targets easier to compare, the concept of difficulty was born. Essentialy, difficulty is inversely proportional to target to make comparisons easier.

The difficulty on testnet when there haven't been any blocks found in 20 minutes resets to 1. This gives us context for how difficult mainnet is. Generally, the difficulty number can be thought of as how much more difficult mainnet is than testnet.

This is the number that gets shown in block explorers and so on as difficulty is a much more intuitive way to understand what's going on in terms of effort required to create a new block.


```python
# Calculating Target from Bits Example

from helper import little_endian_to_int

bits = bytes.fromhex('e93c0118')
exponent = bits[-1]
coefficient = little_endian_to_int(bits[:-1])
target = coefficient*2**(8*(exponent-3))
print('{:x}'.format(target).zfill(64))
```


```python
# Calculating Difficulty from Target Example

from helper import little_endian_to_int

bits = bytes.fromhex('e93c0118')
exponent = bits[-1]
coefficient = little_endian_to_int(bits[:-1])
target = coefficient*2**(8*(exponent-3))

bits_min = bytes.fromhex('ffff001d')
exponent_min = bits_min[-1]
coefficient_min = little_endian_to_int(bits_min[:-1])
min = coefficient_min*2**(8*(exponent_min-3))

difficulty = min // target
print(difficulty)
```

### Try it

#### Calculate the target and difficulty for these bits:
```
f2881718
```

Bits to target formula is 

\\(coefficient\cdot256^{(exponent-3)}\\) 

where coefficient is the first three bytes in little endian and exponent is the last byte.

Target to Difficulty formula is 

\\(difficulty = min / target\\)

where \\(min = 0xffff\cdot256^{(0x1d-3)}\\)


```python
from helper import little_endian_to_int

hex_bits = 'f2881718'

# bytes.fromhex to get the bits
# last byte is exponent
# first three bytes are the coefficient in little endian
# plug into formula coefficient * 256^(exponent-3) to get the target
# print target using print('{:x}'.format(target).zfill(64))

hex_min_bits = 'ffff001d'
# do the same for the min_bits for min target

# difficulty is min target / target
```

### Test Driven Exercise


```python
from io import BytesIO
from block import Block

class Block(Block):

    def target(self):
        '''Returns the proof-of-work target based on the bits'''
        # last byte is exponent
        # the first three bytes are the coefficient in little endian
        # the formula is:
        # coefficient * 2**(8*(exponent-3))
        pass

    def difficulty(self):
        '''Returns the block difficulty based on the bits'''
        # note difficulty is (target of lowest difficulty) / (self's target)
        # lowest difficulty has bits that equal 0xffff001d
        pass
```

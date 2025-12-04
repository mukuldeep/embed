# Bit Manipulation Tricks

## Basics

-   **Check if k-th bit is set**\
    `(x >> k) & 1`

-   **Set k-th bit**\
    `x | (1 << k)`

-   **Clear k-th bit**\
    `x & ~(1 << k)`

-   **Toggle k-th bit**\
    `x ^ (1 << k)`

-   **Extract lowest set bit**\
    `x & -x`

-   **Remove lowest set bit**\
    `x & (x - 1)`

-   **Isolate highest set bit**

        x |= x >> 1
        x |= x >> 2
        x |= x >> 4
        x |= x >> 8
        x |= x >> 16
        high = (x + 1) >> 1

-   **Check if number is power of two**\
    `(x & (x - 1)) == 0 && x != 0`

-   **Flip all bits**\
    `~x`

## Counting / Parity

-   **Count set bits (Kernighan)**

        count = 0
        while (x) {
          x &= (x - 1)
          count++
        }

-   **Parity of number**

        x ^= x >> 32
        x ^= x >> 16
        x ^= x >> 8
        x ^= x >> 4
        x ^= x >> 2
        x ^= x >> 1
        parity = x & 1

## Swapping / Sign

-   **Swap without temp**\
    `a ^= b; b ^= a; a ^= b;`

-   **Absolute value (branchless)**\
    `mask = x >> 31; abs = (x ^ mask) - mask`

-   **Max/min (branchless)**

        diff = a - b
        mask = diff >> 31
        max = a - (diff & mask)
        min = b + (diff & mask)

## Masking / Ranges

-   **Mask of n LSBs**\
    `(1 << n) - 1`

-   **Clear bits LSB to k**\
    `x & ~((1 << (k + 1)) - 1)`

-   **Clear bits MSB to k**\
    `x & ((1 << k) - 1)`

-   **Extract bits \[l..r\]**\
    `(x >> l) & ((1 << (r - l + 1)) - 1)`

## Arithmetic Tricks

-   **Multiply by 2\^k**\
    `x << k`

-   **Divide by 2\^k**\
    `x >> k`

-   **Check even/odd**\
    `x & 1`

-   **Modulo 2\^k**\
    `x & ((1 << k) - 1)`

-   **Round down to nearest power of 2**\
    `x & -x` (LSB)   

## Bit Reversal

-   **Reverse 32-bit**

        x = (x >> 1) & 0x55555555 | (x & 0x55555555) << 1
        x = (x >> 2) & 0x33333333 | (x & 0x33333333) << 2
        x = (x >> 4) & 0x0F0F0F0F | (x & 0x0F0F0F0F) << 4
        x = (x >> 8) & 0x00FF00FF | (x & 0x00FF00FF) << 8
        x = (x >> 16) | x << 16

-   **Reverse bytes**\
    `__builtin_bswap32(x)` or manual shifts.

## Bit Positions

-   **Position of lowest set bit**\
    `__builtin_ctz(x)`

-   **Position of highest set bit**\
    `__builtin_clz(x)`

-   **Next power of two**

        x--
        x |= x >> 1
        x |= x >> 2
        x |= x >> 4
        x |= x >> 8
        x |= x >> 16
        x++

## Logic Patterns

-   **XOR duplicate removal**\
    `a ^ a = 0`

-   **Turn off rightmost 1**\
    `y = x & (x - 1)`

## Gray Code

-   **Generate**\
    `g = x ^ (x >> 1)`

-   **Recover**

        x ^= x >> 1
        x ^= x >> 2
        x ^= x >> 4
        x ^= x >> 8
        x ^= x >> 16

## Advanced

-   **Branchless clamp 0--255**

        x = x & -(x >= 0)
        x = x | ((255 - x) >> 31)

-   **Sign extend k-bit**

        mask = 1 << (k - 1)
        x = (x ^ mask) - mask

-   **Detect overflow** `( (a ^ sum) & (b ^ sum) ) < 0`

-   **Rotate left/right**

        rol = (x << k) | (x >> (W - k))
        ror = (x >> k) | (x << (W - k))




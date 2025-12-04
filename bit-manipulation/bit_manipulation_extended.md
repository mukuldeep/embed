## BIT EXTRACTION & INJECTION

-  **Insert bits into a field**

        x &= ~(((1 << n) - 1) << pos)     // clear field
        x |= (val & ((1 << n) - 1)) << pos


-  **Replace k-th bit with a value b (0/1)**
        
        x = (x & ~(1 << k)) | (b << k)

-  **Merge bits from two numbers using mask**

        res = (a & mask) | (b & ~mask)

-  **Extract odd/even bits**
   - Odd bits: `(x >> 1) & 0x55555555`
   - Even bits: `x & 0x55555555`

## CARRY/ARITHMETIC TRICKS

-  **Add without using +**

        while (b) {
            carry = (a & b) << 1
            a = a ^ b
            b = carry
        }


-  **Subtract without using -**

        while (b) {
                borrow = (~a) & b
                a = a ^ b
                b = borrow << 1
        }


-  **Multiply using bit-operations (Russian peasant)**

        res = 0
        while (b) {
                if (b & 1) res += a
                a <<= 1
                b >>= 1
        }


-  **Detect carry-out from addition**

        carry = ((a & b) | ((a ^ b) & ~sum)) >> 31

## BITSET MANIPULATIONS

-  **Extract next combination of n bits in bitmask**

        t = m | (m - 1)
        next = (t + 1) | (((~t & -~t) - 1) >> (__builtin_ctz(m) + 1))


-  **Iterate all submasks of a mask**

        for (s = mask; s; s = (s - 1) & mask)


-  **Iterate all supersets of a mask within universe U**

        for (s = mask; s < (1<<U); s = (s + 1) | mask)


-  **Largest power of two ≤ x**
        
        1 << (31 - __builtin_clz(x))

-  **Smallest power of two ≥ x**

        (x <= 1) ? 1 : 1 << (32 - __builtin_clz(x - 1))

## BYTE/WORD OPERATIONS

-  **Detect zero byte in 32-bit integer**

        x = x - 0x01010101
        y = ~x & 0x80808080
        zero_present = y != 0


-  **Check if any byte is 0xFF**

        ((x & 0x80808080) ^ 0x80808080) & (x & 0x7F7F7F7F)


-  **Turn each byte into 0x00 or 0xFF depending on if MSB was set**

        x = x & 0x80808080 ? 0xFFFFFFFF : 0

## BRANCHLESS LOGIC

-  **Branchless equality comparison**

        mask = ~(x ^ y)
        mask &= mask >> 16
        mask &= mask >> 8
        eq = (mask & 0xFF) != 0


-  **Branchless boolean to 0/1**

        b = !!x

-  **Conditional select without branch**

        result = (cond_mask & a) | (~cond_mask & b)

-  **Clamp to range [0, max]**

        x = x & -(x >= 0); x = max ^ ((max ^ x) & -(x < max))

## BIT TABLE / LOOKUP TRICKS

-  **Integer log2 via table for 8-bit**

        static const char log2_8[256] = {...}
        log = log2_8[x]


-  **Use de Bruijn for fast index of LSB**

        idx = debruijn[(x & -x) * 0x077CB531 >> 27]

## BIT REORDERING & SHUFFLING

-  **Shuffle bits**

        x = ((x & 0xAAAAAAAA) >> 1) | ((x & 0x55555555) << 1)

-  **Compress bits to remove zeros (bit compaction)**

        // removes unset bits, packs set bits to the right
        c = x
        x = (x & 0x55555555) | ((x & 0xAAAAAAAA) >> 1)
        x = (x & 0x33333333) | ((x & 0xCCCCCCCC) >> 2)
        x = (x & 0x0F0F0F0F) | ((x & 0xF0F0F0F0) >> 4)
        x = (x & 0x00FF00FF) | ((x & 0xFF00FF00) >> 8)
        x = (x & 0x0000FFFF) | ((x & 0xFFFF0000) >> 16)


- **Parallel bit deposit/extract (software PDEP/PEXT)**

   - PEXT: collect bits selected by mask

                r = 0
                k = 0
                while (mask) {
                        if (x & (mask & -mask))
                                r |= 1 << k
                        mask &= mask - 1
                        k++
                }

   - PDEP: scatter bits into masked positions

## SPECIALIZED INTEGER TRICKS

-  **Round up to nearest multiple of power of 2**\
    `(x + (p - 1)) & -p` where p is power of 2

-  **Compute average of two ints without overflow**

        (a & b) + ((a ^ b) >> 1)

-  **Saturating addition**

        sum = a + b
        mask = ((sum ^ a) & ~(a ^ b)) >> 31
        sum = (sum & ~mask) | ((0x7FFFFFFF + (a < 0)) & mask)


-  **Divide by 3 using multiply-shift**

        (x * 0xAAAAAAAB) >> 33

-  **Fast absolute difference**

        diff = a - b; diff ^ (diff >> 31) - (diff >> 31)

## BITWISE STATE MACHINES

-  **2-bit saturating counter update**

        next = counter + inc
        next = (next | -(next >= 0)) & 3


-  **Toggle between 0,1,2 cyclically**

        x = (x + 1) & 3

## HARDWARE-FRIENDLY TRICKS

-  **Mark all bits below highest set bit as 1**\
Same spreading technique:

        x |= x >> 1
        x |= x >> 2
        x |= x >> 4
        x |= x >> 8
        x |= x >> 16


-  **Count leading/trailing zeros without builtin (SWAR)**
Multiple SWAR implementations exist.

-  **Hamming distance (`popcount(x ^ y)`)**
# XOR_2_Elements_Recovery
How to recover 2 missing integers from a list using XOR operations.

Source [Xor trick](https://florian.github.io//xor-trick/).

XOR Truth table (x ^ y)
```
0 ^ 0 = 0
0 ^ 1 = 1
1 ^ 0 = 1
1 ^ 1 = 0
```

By leveraging 3 important properties of XOR operations we can recover a missing integer of a list.

[XOR and 0](https://florian.github.io//xor-trick/#xor-and-0-x--0--x): x ^ 0 = x

[XOR on the same argument](https://florian.github.io//xor-trick/#xor-on-the-same-argument-x--x--0): x ^ x = 0

[Commutativity](https://florian.github.io//xor-trick/#commutativity-x--y--y--x): x ^ y = y ^ x

We can use those properties to find a missing integer inside a list.

The trick: "The XOR trick: If we have a sequence of XOR operations a ^ b ^ c ^ ..., then we can remove all pairs of duplicated values without affecting the result."

```js
  a ^ b ^ c ^ a ^ b     # Commutativity
= a ^ a ^ b ^ b ^ c     # Using x ^ x = 0
= 0 ^ 0 ^ c             # Using x ^ 0 = x (and commutativity)
= c
```
that means if we have a complete list of [a, b, c] and a missing list of [a, b] we can find the missing element by applying XOR to all elements. Every duplicated element will cancel out and the remaning will be the value of the missing value.


If we have a list of elements of n-1, which each element appear just once and there are just one missing element, we can create a complete list of n elements and apply XOR to both list, all the dupilcated elements will cancel out leaving just the missing element as result.

```js
function xor(arr) {
    let res = 0
    for (let i = 0; i < arr.length; i++) {
        res ^= arr[i]
    }
    return res
}

function find_missing_value() {
    const total = [1, 2, 3, 4, 5, 6, 7, 8]
    const missing = [1, 2, 3, 5, 6, 7, 8]
    const res = xor(total) ^ xor(missing)
    console.log(res) // 4
}
```

We can also use the same algorithm to find 2 missing elements. (Check out the reference for more in depth explanation.)

```js
function xor(arr) {
    let res = 0
    for (let i = 0; i < arr.length; i++) {
        res ^= arr[i]
    }
    return res
}

function lsb(n) {
    return n & ~(n - 1)
}

function filter_by_bit_0(arr, n) {
    let res = []
    for (let i = 0; i < arr.length; i++) {
        if (!(arr[i] & n)) {
            res.push(arr[i])
        }
    }
    return res
}

function filter_by_bit_1(arr, n) {
    let res = []
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] & n) {
            res.push(arr[i])
        }
    }
    return res
}

function recover_u_and_v() {
    const total = [1, 2, 3, 4, 5, 6, 7, 8] // n elements list
    const missing = [1, 2, 3, 5, 6, 7]
    const res = xor(total) ^ xor(missing) // res =  u ^ v
    const idx = lsb(res) // find on wich index they differ, since the least significant bit is the first bit with value of 1, and we know that for the XOR operation result in 1 the bits must be different.
    console.log(res)
    console.log(idx) 
    const P0t = filter_by_bit_0(total, idx) 
    const P0a = filter_by_bit_0(missing, idx)
    console.log("P0t: ", P0t)
    console.log("P0a: ", P0a)
    const u = xor(P0t) ^ xor(P0a) // first missing element
    const P1t = filter_by_bit_1(total, idx)
    const P1a = filter_by_bit_1(missing, idx)
    const v = xor(P1t) ^ xor(P1a) // second missing element
    console.log("P1t: ", P1t)
    console.log("P1a: ", P1a)
    console.log("u: ", u)
    console.log("v: ", v)
}

```



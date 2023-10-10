# PNG File Structure

c.f. [PNG Specification](http://www.libpng.org/pub/png/spec/1.2/PNG-Structure.html)

```
PNG file = PNG Signature + Chunks
```
```
Chunks = IHDR + (other chunks) + IEND
```

## PNG Signature

8 bytes.

In hexadecimal

```
89 50 4e 47 0d 0a 1a 0a
```

In ASCII C notation

```
\211 P N G \r \n \032 \n
```

## Chunk Layout

```
Chunk = Length + Chunk Type + Chunk Data + CRC
```

### Length

4-byte unsigned integer.

Number of bytes in `Chunk Data` field.

The value should $< 2^{31}$.

### Chunk Type

4-byte 

consist of A-Z and a-z (i.e. 65-90 and 97-122)

e.g. `bLoc`, `IHDR`, `IEND`, ...

`bit 5` can distinguish uppercase and lowercase, and can be used to convey some properties.

- Ancillary bit (bit 5 of 1st byte)

    ```
    0 = critical; 1 = ancillary
    ```

- Private bit (bit 5 of 2nd byte)

    ```
    0 = public; 1 = private
    ```
    just ensure public and private names will not conflict

- Reserved bit (bit 5 fo 3rd byte)

    always zero (i.e. uppercase)

- Safe-to-copy bit (bit 5 of 4th byte)
    ```
    0 = unsafe to copy; 1 = safe to copy
    ```
    for critical chunk, this bit always 0

### Chunk Data

can be of zero length

the format is determined by `Chunk Type`

### CRC

4-byte CRC of bytes in `Chunk Type + Chunk Data`

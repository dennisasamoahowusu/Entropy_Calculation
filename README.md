# Entropy_Calculation
Proof of concept, calculation entropy of data stream

I've writen C/C++ realization of:
- AVG meaning entropy
- Shannon entropy
- Shannon entropy only with integer logic (for kernel)

log2_generator - can generate precalculated values of log2 (and function)
with assumption that arg will be left shifted by 16 bit
and return value of log2_lshift16() will be left shifted by 6 bit
All that shifts used for avoid of using floating point in calculation

log2_lshift16.h - store precalculated function (log2(arg << 24)) << 6

Values are precalculated only for:
```
1/1073741824 ... 1073741824/1073741824
```

performance_test.sh - stupid but simple methods to compare algorithms

```
# Compile:
~$ make

# Example of test
~$ dd if=/dev/urandom bs=4M count=1 of=indata.bin
~$ ./shannon_int_entropy ./indata.bin
Schanon int entropy: 510/512 ~= 99.660975%

# Run performance test script
./performance_test.sh
Test good compressible data
avg_meaning_entropy 10s
shannon_entropy 12s
shannon_int_entropy 11s
gzip -f -k -3 ./indata.bin 16s
lzop -f -k -3 ./indata.bin 15s
- - - - -
Test half compressible data
avg_meaning_entropy 8s
shannon_entropy 11s
shannon_int_entropy 9s
gzip -f -k -3 ./indata.bin 36s
lzop -f -k -3 ./indata.bin 12s
- - - - -
Test bad compressible data
avg_meaning_entropy 9s
shannon_entropy 11s
shannon_int_entropy 15s
gzip -f -k -3 ./indata.bin 64s
lzop -f -k -3 ./indata.bin 23s
```

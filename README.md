# Rotor-Cuda 

This is a modified version of KeyHunt v1.7 by [kanhavishva](https://github.com/kanhavishva/KeyHunt-Cuda).
A lot of gratitude to all the developers whose codes has been used here.

# Features
- For Bitcoin use ```--coin BTC``` and for Ethereum use ```--coin ETH```
- Single address(rmd160 hash) for BTC or ETH address searching with mode ```-m ADDREES```
- Multiple addresses(rmd160 hashes) or ETH addresses searching with mode ```-m ADDREESES```
- XPoint[s] mode is applicable for ```--coin BTC``` only
- Single xpoint searching with mode ```-m XPOINT```
- Multiple xpoint searching with mode ```-m XPOINTS```
- For XPoint[s] mode use x point of the public key, without 02 or 03 prefix(64 chars).
- Cuda only.

# Usage
- For multiple addresses or xpoints, file format must be binary with sorted data.
- To convert Bitcoin addresses list(text format) to rmd160 hashes binary file use provided python script ```addresses_to_hash160.py```
- To convert pubkeys list(text format) to xpoints binary file use provided python script ```pubkeys_to_xpoint.py```
- To convert Ethereum addresses list(text format) to keccak160 hashes binary file use provided python script ```eth_addresses_to_bin.py```
- After getting binary files from python scripts, use ```BinSort``` tool provided with KeyHunt-Cuda to sort these binary files.
- Don't use XPoint[s] mode with ```uncompressed``` compression type.
- CPU and GPU can not be used together, because the program divides the whole input range into equal parts for all the threads, so use either CPU or GPU so that the whole range can increment by all the threads with consistency.
- Minimum entries for bloom filter is >= 2.
- For Multi GPUs use ```Rotor-Cuda.exe -t 0 --gpui 0,1,2 --gpux 256,256,256,256,256,256 -m addresses --coin BTC --range 1:1fffffffff -i test.bin```
- If you have a weak GPU or driver error, remove **--gpux 256,256** the Grid will automatically assign.

## addresses_to_hash160.py
```
python3 addresses_to_hash160.py addresses_in.txt hash160_out.bin
```

## pubkeys_to_xpoint.py
```
python3 pubkeys_to_xpoint.py pubkeys_in.txt xpoints_out.bin
```

## eth_addresses_to_bin.py
```
python3 eth_addresses_to_bin.py eth_addresses_in.txt eth_addresses_out.bin
```

## BinSort
For hash160 and keccak160 ```length``` is ```20``` and for xpoint ```length``` is ```32```.
```
BinSort.exe
Usage: BinSort.exe length in_file out_file
```

## Rotor-Cuda
Run ```Rotor-Cuda.exe -h```

```
Rotor-Cuda [OPTIONS...] [TARGETS]
Where TARGETS is one address/xpont, or multiple hashes/xpoints file

-h, --help                               : Display this message
-c, --check                              : Check the working of the codes
-u, --uncomp                             : Search uncompressed points
-b, --both                               : Search both uncompressed or compressed points
-g, --gpu                                : Enable GPU calculation
--gpui GPU ids: 0,1,...                  : List of GPU(s) to use, default is 0
--gpux GPU gridsize: g0x,g0y,g1x,g1y,... : Specify GPU(s) kernel gridsize, default is 8*(Device MP count),128
-t, --thread N                           : Specify number of CPU thread, default is number of core
-i, --in FILE                            : Read rmd160 hashes or xpoints from FILE, should be in binary format with sorted
-o, --out FILE                           : Write keys to FILE, default: Found.txt
-m, --mode MODE                          : Specify search mode where MODE is
                                               ADDRESS  : for single address
                                               ADDRESSES: for multiple hashes/addresses
                                               XPOINT   : for single xpoint
                                               XPOINTS  : for multiple xpoints
--coin BTC/ETH                           : Specify Coin name to search
                                               BTC: available mode :-
                                                   ADDRESS, ADDRESSES, XPOINT, XPOINTS
                                               ETH: available mode :-
                                                   ADDRESS, ADDRESSES
-l, --list                               : List cuda enabled devices
--range KEYSPACE                         : Specify the range:
                                               START:END
                                               START:+COUNT
                                               START
                                               :END
                                               :+COUNT
                                               Where START, END, COUNT are in hex format
-r, --rkey Rkey                          : Reloads random start Private key every (-r 10 = 10.000.000.000), default is disabled
-v, --version                            : Show version
```

### For random search add -r 1 to parameters (-r 1-1000)

# CPU mode :
Run ```Rotor-Cuda.exe -t 6 -m addresses --coin BTC --range 1:1fffffffff -i puzzle_1_37_hash160_out_sorted.bin```

```
C:\Users\user>Rotor-Cuda.exe -t 6 -m addresses --coin BTC --range 1:1fffffffff -i puzzle_1_37_hash160_out_sorted.bin

  Rotor-Cuda v1.00

  COMP MODE    : COMPRESSED
  COIN TYPE    : BITCOIN
  SEARCH MODE  : Multi Address
  DEVICE       : CPU
  CPU THREAD   : 6
  SSE          : YES
  MAX FOUND    : 65536
  BTC HASH160s : puzzle_1_37_hash160_out_sorted.bin
  OUTPUT FILE  : Found.txt


  Loaded       : 37 Bitcoin addresses

  Bloom at     : 000001B00ED08E30
  Version      : 2.1
  Entries      : 74
  Error        : 0.0000010000
  Bits         : 2127
  Bits/Elem    : 28.755175
  Bytes        : 266 (0 MB)
  Hash funcs   : 20
  
  Site         : https://github.com/phrutis/Rotor-Cuda
  Donate       : bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

  Start Time   : Thu Oct 21 23:43:22 2021
  Global start : 1 (1 bit)
  Global end   : 1FFFFFFFFF (37 bit)
  Global range : 1FFFFFFFFE (37 bit)


  CPU Core     : Start from range 1 ->

  CPU Core     : Start from range 555555556 ->

  CPU Core     : Start from range AAAAAAAAB ->

  CPU Core     : Start from range 1000000000 ->

  CPU Core     : Start from range 1555555555 ->

  CPU Core     : Start from range 1AAAAAAAAA ->
  
  =================================================================================
  PubAddress: 1BgGZ9tcN4rm9KBzDn7KprQz87SZ26SAMH
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7rFU73sVHnoWn
  Priv (HEX): 1
  PubK (HEX): 0279BE667EF9DCBBAC55A06295CE870B07029BFCDB2DCE28D959F2815B16F81798
  =================================================================================
  ....
  =================================================================================
  PubAddress: 1CfZWK1QTQE3eS9qn61dQjV89KDjZzfNcv
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7rP9Ja2dhtxoh
  Priv (HEX): 2DE40F
  PubK (HEX): 023ED96B524DB5FF4FE007CE730366052B7C511DC566227D929070B9CE917ABB43
  =================================================================================
  [00:00:02] [CPU+GPU: 13.57 Mk/s] [GPU: 0.00 Mk/s] [C: 0.020149 %] [R: 0] [T: 27,693,056 (25 bit)] [F: 22]
  =================================================================================
  PubAddress: 1L2GM8eE7mJWLdo3HZS6su1832NX2txaac
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7rVkthFNsQ6i7
  Priv (HEX): 556E52
  PubK (HEX): 03F82710361B8B81BDEDB16994F30C80DB522450A93E8E87EEB07F7903CF28D04B
  =================================================================================
  [00:00:06] [CPU+GPU: 12.14 Mk/s] [GPU: 0.00 Mk/s] [C: 0.053988 %] [R: 0] [T: 74,201,088 (27 bit)] [F: 23]
  =================================================================================
  PubAddress: 1rSnXMr63jdCuegJFuidJqWxUPV7AtUf7
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7rtHyNcFoApRd
  Priv (HEX): DC2A04
  PubK (HEX): 036EA839D22847EE1DCE3BFC5B11F6CF785B0682DB58C35B63D1342EB221C3490C
  =================================================================================
  [00:00:15] [CPU+GPU: 11.77 Mk/s] [GPU: 0.00 Mk/s] [C: 0.130808 %] [R: 0] [T: 179,781,632 (28 bit)] [F: 24]
  =================================================================================
  PubAddress: 15JhYXn6Mx3oF4Y7PcTAv2wVVAuCFFQNiP
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7siAXycwkwRQg
  Priv (HEX): 1FA5EE5
  PubK (HEX): 03057FBEA3A2623382628DDE556B2A0698E32428D3CD225F3BD034DCA82DD7455A
  =================================================================================
  [00:00:27] [CPU+GPU: 11.42 Mk/s] [GPU: 0.00 Mk/s] [C: 0.233321 %] [R: 0] [T: 320,673,792 (29 bit)] [F: 25]
```

### For random search add -r 1 to parameters (-r 1-1000) Example
Run ```Rotor-Cuda.exe -t 6 -m addresses --coin BTC --range 1:1fffffffff -i puzzle_1_37_hash160_out_sorted.bin -r 2```
```
C:\Users\user>Rotor-Cuda.exe -t 6 -m addresses --coin BTC --range 1:1fffffffff -i puzzle_1_37_hash160_out_sorted.bin -r 2

  Rotor-Cuda v1.00

  COMP MODE    : COMPRESSED
  COIN TYPE    : BITCOIN
  SEARCH MODE  : Multi Address
  DEVICE       : CPU
  CPU THREAD   : 6
  SSE          : YES
  RKEY         : Reload every 2000000000
  MAX FOUND    : 65536
  BTC HASH160s : puzzle_1_37_hash160_out_sorted.bin
  OUTPUT FILE  : Found.txt


  Loaded       : 37 Bitcoin addresses

  Bloom at     : 000002036EF35770
  Version      : 2.1
  Entries      : 74
  Error        : 0.0000010000
  Bits         : 2127
  Bits/Elem    : 28.755175
  Bytes        : 266 (0 MB)
  Hash funcs   : 20
  
  Site         : https://github.com/phrutis/Rotor-Cuda
  Donate       : bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

  Start Time   : Fri Oct 22 00:13:33 2021


  Base Key     : Randomly changes 6 Private keys every 2.000.000.000 on the counter

  [00:00:40] [R: 0] [89B7A47117DDB7E9F1C81852C06A3B3F29F6F6ECE4621917A469E7A7AB493ABF] [F: 0] [CPU+GPU: 11.93 Mk/s] [GPU: 0.00 Mk/s] [T: 491,051,008]
 ```

### For random search add -r 1 to parameters (-r 1-1000)
Run ```Rotor-Cuda.exe -t 6 -m address --coin eth --range 8000000:fffffff 0xfda5c442e76a95f96c09782f1a15d3b58e32404f```
```
C:\Users\user>Rotor-Cuda.exe -t 6 -m address --coin eth --range 8000000:fffffff 0xfda5c442e76a95f96c09782f1a15d3b58e32404f

  Rotor-Cuda v1.00

  COIN TYPE    : ETHEREUM
  SEARCH MODE  : Single Address
  DEVICE       : CPU
  CPU THREAD   : 6
  SSE          : NO
  MAX FOUND    : 65536
  ETH ADDRESS  : 0xfda5c442e76a95f96c09782f1a15d3b58e32404f
  OUTPUT FILE  : Found.txt

  Start Time   : Thu Oct 21 23:49:04 2021
  Global start : 8000000 (28 bit)
  Global end   : FFFFFFF (28 bit)
  Global range : 7FFFFFF (27 bit)


  CPU Core     : Start from range 8000000 ->

  CPU Core     : Start from range 9555555 ->

  CPU Core     : Start from range D555554 ->

  CPU Core     : Start from range AAAAAAA ->

  CPU Core     : Start from range BFFFFFF ->

  CPU Core     : Start from range EAAAAA9 ->

  [00:00:02] [CPU+GPU: 8.40 Mk/s] [GPU: 0.00 Mk/s] [C: 12.734985 %] [R: 0] [T: 17,092,608 (25 bit)] [F: 0]
  =================================================================================
  PubAddress: 0xfda5c442e76a95f96c09782f1a15d3b58e32404f
  Priv (HEX): D916CE8
  PubK (HEX): E9E661838A96A65331637E2A3E948DC0756E5009E7CB5C36664D9B72DD18C0A709F531540A4CA59F50F93B8BF7B0C060045754AAAE7CE1BEA5A136C0D5874B97
  =================================================================================
  [00:00:04] [CPU+GPU: 7.53 Mk/s] [GPU: 0.00 Mk/s] [C: 22.798157 %] [R: 0] [T: 30,599,168 (25 bit)] [F: 1]

  BYE
```


# GPU mode:
## Multiple addresses mode:
Run ```Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m addresses --coin BTC -o Found.txt --range 1:1fffffffff -i puzzle_1_37_hash160_out_sorted.bin```
```
C:\Users\user>Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m addresses --coin BTC -o Found.txt --range 1:1fffffffff -i puzzle_1_37_hash160_out_sorted.bin

  Rotor-Cuda v1.00

  COMP MODE    : COMPRESSED
  COIN TYPE    : BITCOIN
  SEARCH MODE  : Multi Address
  DEVICE       : GPU
  CPU THREAD   : 0
  GPU IDS      : 0
  GPU GRIDSIZE : 256x256
  SSE          : YES
  MAX FOUND    : 65536
  BTC HASH160s : puzzle_1_37_hash160_out_sorted.bin
  OUTPUT FILE  : Found.txt


  Loaded       : 37 Bitcoin addresses

  Bloom at     : 000001EFD5488A40
  Version      : 2.1
  Entries      : 74
  Error        : 0.0000010000
  Bits         : 2127
  Bits/Elem    : 28.755175
  Bytes        : 266 (0 MB)
  Hash funcs   : 20
  
  Site         : https://github.com/phrutis/Rotor-Cuda
  Donate       : bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

  Start Time   : Thu Oct 21 23:51:48 2021
  Global start : 1 (1 bit)
  Global end   : 1FFFFFFFFF (37 bit)
  Global range : 1FFFFFFFFE (37 bit)

  GPU          : GPU #0 NVIDIA GeForce RTX 2070 (36x64 cores) Grid(256x256)

  Divide the range 1FFFFFFFFE into 65536 threads for fast parallel search
  Thread 00000: 0000000000000000000000000000000000000000000000000000000000000001 -> 0000000000000000000000000000000000000000000000000000000000200000
  Thread 00001: 0000000000000000000000000000000000000000000000000000000000200000 -> 00000000000000000000000000000000000000000000000000000000003FFFFF
  Thread 00002: 00000000000000000000000000000000000000000000000000000000003FFFFF -> 00000000000000000000000000000000000000000000000000000000005FFFFE
  Thread 00003: 00000000000000000000000000000000000000000000000000000000005FFFFE -> 00000000000000000000000000000000000000000000000000000000007FFFFD
          ... :
  Thread 65534: 0000000000000000000000000000000000000000000000000000001FFFBF0003 -> 0000000000000000000000000000000000000000000000000000001FFFDF0002
  Thread 65535: 0000000000000000000000000000000000000000000000000000001FFFDF0002 -> 0000000000000000000000000000000000000000000000000000001FFFFF0001
  Thread 65536: 0000000000000000000000000000000000000000000000000000001FFFFF0001 -> 00000000000000000000000000000000000000000000000000000020001F0000


  =================================================================================
  PubAddress: 1PgQVLmst3Z314JrQn5TNiys8Hc38TcXJu
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7rFUGxXgtm63M
  Priv (HEX): 483
  PubK (HEX): 038B05B0603ABD75B0C57489E451F811E1AFE54A8715045CDF4888333F3EBC6E8B
  =================================================================================

  ....
  =================================================================================
  PubAddress: 1JVnST957hGztonaWK6FougdtjxzHzRMMg
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7tefTXkqGMNis
  Priv (HEX): 340326E
  PubK (HEX): 024E4F50A2A3ECCDB368988AE37CD4B611697B26B29696E42E06D71368B4F3840F
  =================================================================================
  [00:00:02] [CPU+GPU: 669.59 Mk/s] [GPU: 669.59 Mk/s] [C: 0.976563 %] [R: 0] [T: 1,342,177,280 (31 bit)] [F: 15]
  =================================================================================
  PubAddress: 1QCbW9HWnwQWiQqVo5exhAnmfqKRrCRsvW
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7rFY5iMZbuRxj
  Priv (HEX): 68F3
  PubK (HEX): 02FEA58FFCF49566F6E9E9350CF5BCA2861312F422966E8DB16094BEB14DC3DF2C
  =================================================================================
  [00:00:04] [CPU+GPU: 669.56 Mk/s] [GPU: 669.56 Mk/s] [C: 1.953125 %] [R: 0] [T: 2,684,354,560 (32 bit)] [F: 16]
  =================================================================================
  PubAddress: 1BDyrQ6WoF8VN3g9SAS1iKZcPzFfnDVieY
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7rFbjHrFMWzJp
  Priv (HEX): C936
  PubK (HEX): 029D8C5D35231D75EB87FD2C5F05F65281ED9573DC41853288C62EE94EB2590B7A
  =================================================================================
  [00:00:08] [CPU+GPU: 669.50 Mk/s] [GPU: 669.50 Mk/s] [C: 3.906250 %] [R: 0] [T: 5,368,709,120 (33 bit)] [F: 17]
  =================================================================================
  PubAddress: 1HduPEXZRdG26SUT5Yk83mLkPyjnZuJ7Bm
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7rFiHkRsp99uC
  Priv (HEX): 1764F
  PubK (HEX): 033F688BAE8321B8E02B7E6C0A55C2515FB25AB97D85FDA842449F7BFA04E128C3
  =================================================================================
  [00:00:14] [CPU+GPU: 668.74 Mk/s] [GPU: 668.74 Mk/s] [C: 6.835938 %] [R: 0] [T: 9,395,240,960 (34 bit)] [F: 18]
  =================================================================================
  PubAddress: 19EEC52krRUK1RkUAEZmQdjTyHT7Gp1TYT
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M89tAnjFUUDRtJ
  Priv (HEX): 17E2551E
  PubK (HEX): 026CAAD634382D34691E3BEF43ED4A124D8909A8A3362F91F1D20ABAAF7E917B36
  =================================================================================

  =================================================================================
  PubAddress: 1Be2UF9NLfyLFbtm3TCbmuocc9N1Kduci1
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9Mg1Upu7eJAtiDr
  Priv (HEX): 9DE820A7C
  PubK (HEX): 02B3E772216695845FA9DDA419FB5DACA28154D8AA59EA302F05E916635E47B9F6
  =================================================================================
  [00:00:16] [CPU+GPU: 668.82 Mk/s] [GPU: 668.82 Mk/s] [C: 7.812500 %] [R: 0] [T: 10,737,418,240 (34 bit)] [F: 20]

....

```
### For random search add -r 1 to parameters (-r 1-1000) Example
Run ```Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m addresses --coin BTC -o Found.txt --range 1:1fffffffff -i puzzle_1_37_hash160_out_sorted.bin -r 5```
```
C:\Users\user>Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m addresses --coin BTC -o Found.txt --range 1:1fffffffff -i puzzle_1_37_hash160_out_sorted.bin -r 5

  Rotor-Cuda v1.00

  COMP MODE    : COMPRESSED
  COIN TYPE    : BITCOIN
  SEARCH MODE  : Multi Address
  DEVICE       : GPU
  CPU THREAD   : 0
  GPU IDS      : 0
  GPU GRIDSIZE : 256x256
  SSE          : YES
  RKEY         : Reload every 5000000000
  MAX FOUND    : 65536
  BTC HASH160s : puzzle_1_37_hash160_out_sorted.bin
  OUTPUT FILE  : Found.txt


  Loaded       : 37 Bitcoin addresses

  Bloom at     : 0000024E5CCE9980
  Version      : 2.1
  Entries      : 74
  Error        : 0.0000010000
  Bits         : 2127
  Bits/Elem    : 28.755175
  Bytes        : 266 (0 MB)
  Hash funcs   : 20

  Site         : https://github.com/phrutis/Rotor-Cuda
  Donate       : bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

  Start Time   : Fri Oct 22 00:32:25 2021

  GPU          : GPU #0 NVIDIA GeForce RTX 2070 (36x64 cores) Grid(256x256)

  Base Key     : Randomly changes 65536 start Private keys every 5.000.000.000 on the counter

  [00:00:30] [R: 3] [6C085CF52B1536ACADB81D2700E6B806D8AA97ED589B4ADAE9388DDEBBF33A91] [F: 0] [CPU+GPU: 612.22 Mk/s] [GPU: 612.22 Mk/s] [T: 19,193,135,104]
```

## Multiple addresses mode with ethereum
Run ```Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m addresses --coin eth --range 1:1fffffffff -i puzzle_1_37_addresses_eth_sorted.bin -o Found_Eth.txt```
```
C:\Users\user>Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m addresses --coin eth --range 1:1fffffffff -i puzzle_1_37_addresses_eth_sorted.bin -o Found_Eth.txt

  Rotor-Cuda v1.00

  COIN TYPE    : ETHEREUM
  SEARCH MODE  : Multi Address
  DEVICE       : GPU
  CPU THREAD   : 0
  GPU IDS      : 0
  GPU GRIDSIZE : 256x256
  SSE          : NO
  MAX FOUND    : 65536
  ETH ADDRESSES: puzzle_1_37_addresses_eth_sorted.bin
  OUTPUT FILE  : Found_Eth.txt


  Loaded       : 37 Ethereum addresses

  Bloom at     : 000001B769F2A030
  Version      : 2.1
  Entries      : 74
  Error        : 0.0000010000
  Bits         : 2127
  Bits/Elem    : 28.755175
  Bytes        : 266 (0 MB)
  Hash funcs   : 20

  Site         : https://github.com/phrutis/Rotor-Cuda
  Donate       : bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

  Start Time   : Thu Oct 21 23:54:45 2021
  Global start : 1 (1 bit)
  Global end   : 1FFFFFFFFF (37 bit)
  Global range : 1FFFFFFFFE (37 bit)

  GPU          : GPU #0 NVIDIA GeForce RTX 2070 (36x64 cores) Grid(256x256)

  Divide the range 1FFFFFFFFE into 65536 threads for fast parallel search
  Thread 00000: 0000000000000000000000000000000000000000000000000000000000000001 -> 0000000000000000000000000000000000000000000000000000000000200000
  Thread 00001: 0000000000000000000000000000000000000000000000000000000000200000 -> 00000000000000000000000000000000000000000000000000000000003FFFFF
  Thread 00002: 00000000000000000000000000000000000000000000000000000000003FFFFF -> 00000000000000000000000000000000000000000000000000000000005FFFFE
  Thread 00003: 00000000000000000000000000000000000000000000000000000000005FFFFE -> 00000000000000000000000000000000000000000000000000000000007FFFFD
          ... :
  Thread 65534: 0000000000000000000000000000000000000000000000000000001FFFBF0003 -> 0000000000000000000000000000000000000000000000000000001FFFDF0002
  Thread 65535: 0000000000000000000000000000000000000000000000000000001FFFDF0002 -> 0000000000000000000000000000000000000000000000000000001FFFFF0001
  Thread 65536: 0000000000000000000000000000000000000000000000000000001FFFFF0001 -> 00000000000000000000000000000000000000000000000000000020001F0000


  =================================================================================
  PubAddress: 0x2c559fed4b8a8e12e170740d4dabb4fc7d3f9b49
  Priv (HEX): 483
  PubK (HEX): 8B05B0603ABD75B0C57489E451F811E1AFE54A8715045CDF4888333F3EBC6E8B1D10F88145DB40FB889E2DDCE81BDA7C27F5B615ACD6179DBB30F4FE7F40FB39
  =================================================================================
  
  ....
  =================================================================================
  PubAddress: 0xe392229bd7664f4ce539303ce60a2235d915caca
  Priv (HEX): 340326E
  PubK (HEX): 4E4F50A2A3ECCDB368988AE37CD4B611697B26B29696E42E06D71368B4F3840F076D012B98C9E265DDA1A9028AE37F1A4548505FAC40524D507B5675C74714CA
  =================================================================================
  [00:00:02] [CPU+GPU: 465.12 Mk/s] [GPU: 465.12 Mk/s] [C: 0.683594 %] [R: 0] [T: 939,524,096 (30 bit)] [F: 15]
  =================================================================================
  PubAddress: 0x8b7696dca505e006408f9b0e8bdffdd20993e15e
  Priv (HEX): 68F3
  PubK (HEX): FEA58FFCF49566F6E9E9350CF5BCA2861312F422966E8DB16094BEB14DC3DF2CC71136E9F21EC86870C3A999F045D712F848C6FD6ED9582521C3F7444C8F182E
  =================================================================================
  [00:00:06] [CPU+GPU: 486.46 Mk/s] [GPU: 486.46 Mk/s] [C: 2.148438 %] [R: 0] [T: 2,952,790,016 (32 bit)] [F: 16]
  =================================================================================
  PubAddress: 0xf1527b97640c3935df498110dccd4ce9181c7743
  Priv (HEX): C936
  PubK (HEX): 9D8C5D35231D75EB87FD2C5F05F65281ED9573DC41853288C62EE94EB2590B7A83C227263DECE3D7E8AFE4005EFF667BB90DD33CE17C663B1E83B303B12C0348
  =================================================================================
  [00:00:12] [CPU+GPU: 477.57 Mk/s] [GPU: 477.57 Mk/s] [C: 4.199219 %] [R: 0] [T: 5,771,362,304 (33 bit)] [F: 17]
  ...

```

## Single address mode 
Run ```Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m address --coin BTC --range 400000000:7ffffffff 1PWCx5fovoEaoBowAvF5k91m2Xat9bMgwb```
```
C:\Users\user>Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m address --coin BTC --range 400000000:7ffffffff 1PWCx5fovoEaoBowAvF5k91m2Xat9bMgwb

  Rotor-Cuda v1.00

  COMP MODE    : COMPRESSED
  COIN TYPE    : BITCOIN
  SEARCH MODE  : Single Address
  DEVICE       : GPU
  CPU THREAD   : 0
  GPU IDS      : 0
  GPU GRIDSIZE : 256x256
  SSE          : YES
  MAX FOUND    : 65536
  BTC ADDRESS  : 1PWCx5fovoEaoBowAvF5k91m2Xat9bMgwb
  OUTPUT FILE  : Found.txt

  Start Time   : Thu Oct 21 23:56:13 2021
  Global start : 400000000 (35 bit)
  Global end   : 7FFFFFFFF (35 bit)
  Global range : 3FFFFFFFF (34 bit)

  GPU          : GPU #0 NVIDIA GeForce RTX 2070 (36x64 cores) Grid(256x256)

  Divide the range 3FFFFFFFF into 65536 threads for fast parallel search
  Thread 00000: 0000000000000000000000000000000000000000000000000000000400000000 -> 000000000000000000000000000000000000000000000000000000040003FFFF
  Thread 00001: 000000000000000000000000000000000000000000000000000000040003FFFF -> 000000000000000000000000000000000000000000000000000000040007FFFE
  Thread 00002: 000000000000000000000000000000000000000000000000000000040007FFFE -> 00000000000000000000000000000000000000000000000000000004000BFFFD
  Thread 00003: 00000000000000000000000000000000000000000000000000000004000BFFFD -> 00000000000000000000000000000000000000000000000000000004000FFFFC
          ... :
  Thread 65534: 00000000000000000000000000000000000000000000000000000007FFF70002 -> 00000000000000000000000000000000000000000000000000000007FFFB0001
  Thread 65535: 00000000000000000000000000000000000000000000000000000007FFFB0001 -> 00000000000000000000000000000000000000000000000000000007FFFF0000
  Thread 65536: 00000000000000000000000000000000000000000000000000000007FFFF0000 -> 000000000000000000000000000000000000000000000000000000080002FFFF

  [00:00:12] [CPU+GPU: 769.93 Mk/s] [GPU: 769.93 Mk/s] [C: 53.906250 %] [R: 0] [T: 9,261,023,232 (34 bit)] [F: 0]
  =================================================================================
  PubAddress: 1PWCx5fovoEaoBowAvF5k91m2Xat9bMgwb
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9MP7J9oTbu6KuRr
  Priv (HEX): 4AED21170
  PubK (HEX): 02F6A8148A62320E149CB15C544FE8A25AB483A0095D2280D03B8A00A7FEADA13D
  =================================================================================
  [00:00:14] [CPU+GPU: 774.68 Mk/s] [GPU: 774.68 Mk/s] [C: 63.281250 %] [R: 0] [T: 10,871,635,968 (34 bit)] [F: 1]

  BYE
```

## Single address mode with ethereum
Run ```Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m address --coin eth --range 800000000:fffffffff 0x1ffbb8f1dfc7e2308c39637e3f4b63c2362ddc6c```
```
C:\Users\user>Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m address --coin eth --range 800000000:fffffffff 0x1ffbb8f1dfc7e2308c39637e3f4b63c2362ddc6c

  Rotor-Cuda v1.00

  COIN TYPE    : ETHEREUM
  SEARCH MODE  : Single Address
  DEVICE       : GPU
  CPU THREAD   : 0
  GPU IDS      : 0
  GPU GRIDSIZE : 256x256
  SSE          : NO
  MAX FOUND    : 65536
  ETH ADDRESS  : 0x1ffbb8f1dfc7e2308c39637e3f4b63c2362ddc6c
  OUTPUT FILE  : Found.txt

  Start Time   : Thu Oct 21 23:58:06 2021
  Global start : 800000000 (36 bit)
  Global end   : FFFFFFFFF (36 bit)
  Global range : 7FFFFFFFF (35 bit)

  GPU          : GPU #0 NVIDIA GeForce RTX 2070 (36x64 cores) Grid(256x256)

  Divide the range 7FFFFFFFF into 65536 threads for fast parallel search
  Thread 00000: 0000000000000000000000000000000000000000000000000000000800000000 -> 000000000000000000000000000000000000000000000000000000080007FFFF
  Thread 00001: 000000000000000000000000000000000000000000000000000000080007FFFF -> 00000000000000000000000000000000000000000000000000000008000FFFFE
  Thread 00002: 00000000000000000000000000000000000000000000000000000008000FFFFE -> 000000000000000000000000000000000000000000000000000000080017FFFD
  Thread 00003: 000000000000000000000000000000000000000000000000000000080017FFFD -> 00000000000000000000000000000000000000000000000000000008001FFFFC
          ... :
  Thread 65534: 0000000000000000000000000000000000000000000000000000000FFFEF0002 -> 0000000000000000000000000000000000000000000000000000000FFFF70001
  Thread 65535: 0000000000000000000000000000000000000000000000000000000FFFF70001 -> 0000000000000000000000000000000000000000000000000000000FFFFF0000
  Thread 65536: 0000000000000000000000000000000000000000000000000000000FFFFF0000 -> 000000000000000000000000000000000000000000000000000000100006FFFF

  [00:00:24] [CPU+GPU: 484.73 Mk/s] [GPU: 484.73 Mk/s] [C: 34.375000 %] [R: 0] [T: 11,811,160,064 (34 bit)] [F: 0]
  =================================================================================
  PubAddress: 0x1ffbb8f1dfc7e2308c39637e3f4b63c2362ddc6c
  Priv (HEX): AAAAAAAAA
  PubK (HEX): 191E2BD4A789E873AFFA936C7F65AB7831C503556F7CF6753F190D5A1C4A91E046AC098E6ECCC7E0D380DE6F1A5748E1FC4FC515669A8AA084D25085392DA510
  =================================================================================
  [00:00:26] [CPU+GPU: 493.74 Mk/s] [GPU: 493.74 Mk/s] [C: 37.500000 %] [R: 0] [T: 12,884,901,888 (34 bit)] [F: 1]

  BYE
```

## Multiple XPoints mode
Run ```Rotor-Cuda -t 0 -g --gpui 0 --gpux 256,256 -m xpoints --coin BTC --range 1:1fffffffff -i xpoints_1_37_out_sorted.bin```
```
C:\Users\user>Rotor-Cuda -t 0 -g --gpui 0 --gpux 256,256 -m xpoints --coin BTC --range 1:1fffffffff -i xpoints_1_37_out_sorted.bin

  Rotor-Cuda v1.00

  COMP MODE    : COMPRESSED
  COIN TYPE    : BITCOIN
  SEARCH MODE  : Multi X Points
  DEVICE       : GPU
  CPU THREAD   : 0
  GPU IDS      : 0
  GPU GRIDSIZE : 256x256
  SSE          : NO
  MAX FOUND    : 65536
  BTC XPOINTS  : xpoints_1_37_out_sorted.bin
  OUTPUT FILE  : Found.txt


  Loaded       : 37 Bitcoin xpoints

  Bloom at     : 000001BC8D5B9410
  Version      : 2.1
  Entries      : 74
  Error        : 0.0000010000
  Bits         : 2127
  Bits/Elem    : 28.755175
  Bytes        : 266 (0 MB)
  Hash funcs   : 20

  Site         : https://github.com/phrutis/Rotor-Cuda
  Donate       : bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

  Start Time   : Thu Oct 21 23:59:58 2021
  Global start : 1 (1 bit)
  Global end   : 1FFFFFFFFF (37 bit)
  Global range : 1FFFFFFFFE (37 bit)

  GPU          : GPU #0 NVIDIA GeForce RTX 2070 (36x64 cores) Grid(256x256)

  Divide the range 1FFFFFFFFE into 65536 threads for fast parallel search
  Thread 00000: 0000000000000000000000000000000000000000000000000000000000000001 -> 0000000000000000000000000000000000000000000000000000000000200000
  Thread 00001: 0000000000000000000000000000000000000000000000000000000000200000 -> 00000000000000000000000000000000000000000000000000000000003FFFFF
  Thread 00002: 00000000000000000000000000000000000000000000000000000000003FFFFF -> 00000000000000000000000000000000000000000000000000000000005FFFFE
  Thread 00003: 00000000000000000000000000000000000000000000000000000000005FFFFE -> 00000000000000000000000000000000000000000000000000000000007FFFFD
          ... :
  Thread 65534: 0000000000000000000000000000000000000000000000000000001FFFBF0003 -> 0000000000000000000000000000000000000000000000000000001FFFDF0002
  Thread 65535: 0000000000000000000000000000000000000000000000000000001FFFDF0002 -> 0000000000000000000000000000000000000000000000000000001FFFFF0001
  Thread 65536: 0000000000000000000000000000000000000000000000000000001FFFFF0001 -> 00000000000000000000000000000000000000000000000000000020001F0000

  ...
  =================================================================================
  PubAddress: 1BDyrQ6WoF8VN3g9SAS1iKZcPzFfnDVieY
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7rFbjHrFMWzJp
  Priv (HEX): C936
  PubK (HEX): 029D8C5D35231D75EB87FD2C5F05F65281ED9573DC41853288C62EE94EB2590B7A
  =================================================================================
  [00:00:02] [CPU+GPU: 1.71 Gk/s] [GPU: 1.71 Gk/s] [C: 2.539063 %] [R: 0] [T: 3,489,660,928 (32 bit)] [F: 17]
  =================================================================================
  PubAddress: 1HduPEXZRdG26SUT5Yk83mLkPyjnZuJ7Bm
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7rFiHkRsp99uC
  Priv (HEX): 1764F
  PubK (HEX): 033F688BAE8321B8E02B7E6C0A55C2515FB25AB97D85FDA842449F7BFA04E128C3
  =================================================================================
  [00:00:06] [CPU+GPU: 1.64 Gk/s] [GPU: 1.64 Gk/s] [C: 7.226563 %] [R: 0] [T: 9,932,111,872 (34 bit)] [F: 18]
  =================================================================================
  PubAddress: 19EEC52krRUK1RkUAEZmQdjTyHT7Gp1TYT
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M89tAnjFUUDRtJ
  Priv (HEX): 17E2551E
  PubK (HEX): 026CAAD634382D34691E3BEF43ED4A124D8909A8A3362F91F1D20ABAAF7E917B36
  =================================================================================
  ...

```

## Single XPoint mode
Run ```Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m xpoint --coin BTC --range 8000000000:ffffffffff a2efa402fd5268400c77c20e574ba86409ededee7c4020e4b9f0edbee53de0d4```
```
C:\Users\user>Rotor-Cuda.exe -t 0 -g --gpui 0 --gpux 256,256 -m xpoint --coin BTC --range 8000000000:ffffffffff a2efa402fd5268400c77c20e574ba86409ededee7c4020e4b9f0edbee53de0d4

  Rotor-Cuda v1.00

  COMP MODE    : COMPRESSED
  COIN TYPE    : BITCOIN
  SEARCH MODE  : Single X Point
  DEVICE       : GPU
  CPU THREAD   : 0
  GPU IDS      : 0
  GPU GRIDSIZE : 256x256
  SSE          : NO
  MAX FOUND    : 65536
  BTC XPOINT   : a2efa402fd5268400c77c20e574ba86409ededee7c4020e4b9f0edbee53de0d4
  OUTPUT FILE  : Found.txt

  Start Time   : Fri Oct 22 00:04:32 2021
  Global start : 8000000000 (40 bit)
  Global end   : FFFFFFFFFF (40 bit)
  Global range : 7FFFFFFFFF (39 bit)

  GPU          : GPU #0 NVIDIA GeForce RTX 2070 (36x64 cores) Grid(256x256)

  Divide the range 7FFFFFFFFF into 65536 threads for fast parallel search
  Thread 00000: 0000000000000000000000000000000000000000000000000000008000000000 -> 00000000000000000000000000000000000000000000000000000080007FFFFF
  Thread 00001: 00000000000000000000000000000000000000000000000000000080007FFFFF -> 0000000000000000000000000000000000000000000000000000008000FFFFFE
  Thread 00002: 0000000000000000000000000000000000000000000000000000008000FFFFFE -> 00000000000000000000000000000000000000000000000000000080017FFFFD
  Thread 00003: 00000000000000000000000000000000000000000000000000000080017FFFFD -> 0000000000000000000000000000000000000000000000000000008001FFFFFC
          ... :
  Thread 65534: 000000000000000000000000000000000000000000000000000000FFFEFF0002 -> 000000000000000000000000000000000000000000000000000000FFFF7F0001
  Thread 65535: 000000000000000000000000000000000000000000000000000000FFFF7F0001 -> 000000000000000000000000000000000000000000000000000000FFFFFF0000
  Thread 65536: 000000000000000000000000000000000000000000000000000000FFFFFF0000 -> 00000000000000000000000000000000000000000000000000000100007EFFFF

  [00:02:22] [CPU+GPU: 2.19 Gk/s] [GPU: 2.19 Gk/s] [C: 57.348633 %] [R: 0] [T: 315,277,443,072 (39 bit)] [F: 0]
  =================================================================================
  PubAddress: 1EeAxcprB2PpCnr34VfZdFrkUWuxyiNEFv
  Priv (WIF): p2pkh:KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9aFJuCJDo5F6Jm7
  Priv (HEX): E9AE4933D6
  PubK (HEX): 03A2EFA402FD5268400C77C20E574BA86409EDEDEE7C4020E4B9F0EDBEE53DE0D4
  =================================================================================
  [00:02:24] [CPU+GPU: 2.17 Gk/s] [GPU: 2.17 Gk/s] [C: 58.007813 %] [R: 0] [T: 318,901,321,728 (39 bit)] [F: 1]

  BYE
```

## Building
##### Windows
- Microsoft Visual Studio Community 2019
- CUDA version [**10.22**](https://developer.nvidia.com/cuda-10.2-download-archive?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exenetwork)
##### Linux
 - Edit the makefile and set up the appropriate CUDA SDK and compiler paths for nvcc. Or pass them as variables to `make` command.
 - Install libgmp: ```sudo apt install -y libgmp-dev```

    ```make
    CUDA       = /usr/local/cuda-11.0
    CXXCUDA    = /usr/bin/g++
    ```
 - To build CPU-only version (without CUDA support):
    ```sh
    $ make all
    ```
 - To build with CUDA: pass CCAP value according to your GPU compute capability
    ```sh
    $ cd Rotor-Cuda
    $ make gpu=1 CCAP=75 all
    ```
#### BinSort
```sh
$ cd BinSort
$ make
```
#### Python scripts
```
python3 -m pip install base58
```

## License
Rotor-Cuda is licensed under GPLv3.

## Donation
- BTC: bc1qh2mvnf5fujg93mwl8pe688yucaw9sflmwsukz9

## __Disclaimer__
ALL THE CODES, PROGRAM AND INFORMATION ARE FOR EDUCATIONAL PURPOSES ONLY. USE IT AT YOUR OWN RISK. THE DEVELOPER WILL NOT BE RESPONSIBLE FOR ANY LOSS, DAMAGE OR CLAIM ARISING FROM USING THIS PROGRAM.


# jank-BPCS
BPCS steganography, written in very messy C++

Basically, we are hiding data inside a file by modifying only the bits with the least impact on image quality.



LSB is an implementation where you store the data on the lowest bit of each byte of data in a file, leading to minimal observable change. The observable changes increase significantly as you use a higher bit.

BPCS improves upon this by determining the noiseiness of specific portions of the file, and then avoiding those which are too uniform. The written data is also obfuscated to make it noisy if it isn't noisy enough. At higher bits, this reduces the observable changes relative to LSB.

Below, 100KB of lorem ipsum text was written to the an 291KB image file. Values under 100kb indicate running out of available blocks:

| Original | 100KB LSB | 85KB BPCS 8 x 2 pixel 0.3A | 94KB BPCS 8 x 16 pixel 0.3A |
|---|---|---|---|
| ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/original_image.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/100kb_LSB_output.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/85kb_2pxh_0.3A.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/94kb_16pxh_0.3A.bmp) |

Of course, this selective behavior comes with the consequence of being able to store less, alongside more space used for headers. 

Both BPCS implementations had to resort to using higher bits than the LSB, leading to obvious artifacting. This can be improved upon through modifying the algorithm parameters



At lower sizes, the result becomes much more accceptable:

| Original | 25KB LSB | 25KB BPCS 8x16 pixel 0.4A |
|---|---|---|
| ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/original_image.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/25kb_LSB_output.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/25kb_16pxh_0.4A.bmp) |

To get a better idea of what steganography actually looks like, this is 100KB of lorem ipsum on a white background, with no threshold, and starting from the 2nd most significant bit:

| 100KB LSB | 100KB BPCS 8 x 6 pixel -0.1A (no obfuscation) | 100KB BPCS 8 x 6 pixel 0.7A |
|---|---|---|
| ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/LSB_100kb_WB.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/BPCS_6px_-0.1A_WB.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/BPCS_6px_0.7A_WB.bmp) |

## Update 1
Added seperated read and write A factors. While written data generally achieves A factors around the 0.4 range, the highest bits were often left at 0 leading to the banding observed in the 100KB test. Additionally, increased density can be achieved by lowering the read A factor, while keeping visual quality via obsfuscating written data.

The same 100KB lorem ipsum text was used on the same 291KB file. This time, we were able to write all 100KB: 

| Original | 94KB BPCS 8 x 16 pixel 0.3A(r/d) | 100KB BPCS 8 x 16 pixel 0.05A(read) 0.45A(data) |
|---|---| ---|
| ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/original_image.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/94kb_16pxh_0.3A.bmp) | ![alt_text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/100kb_16pxh_D0.05A_W0.045A.bmp) |


## How to use
Go to the [releases page](https://github.com/plebbbb/jank-BPCS/releases) and download, then run it using a command line.

## Future improvements

- [ ] Improve application of A factor on image

  - [x] Seperate Read/Write A factors
  
  - [ ] Add different A factors for each layer
  
- [ ] Support more file formats

  - [ ] PNGs (oh no) 

- [ ] Refactor everything

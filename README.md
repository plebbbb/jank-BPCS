# jank-BPCS
BPCS steganography, written in very messy C++

Basically, we are hiding data inside a file by modifying only the bits with the least impact on image quality.



LSB is an implementation where you store the data on the lowest bit of each byte of data in a file, leading to minimal observable change. The observable changes increase significantly as you use a higher bit.

BPCS improves upon this by determining the noiseiness of specific segements of the file, and avoiding those which are too uniform. The written data is also obsfuscated to make it noisy in a reversable process if it isn't noisy enough. At higher bits, this reduces the observable changes.

| Original | 100KB LSB | 85KB BPCS 8 x 2 pixel | 94KB BPCS 8 x 16 pixel |
|---|---|---|---|
| ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/original_image.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/100kb_LSB_output.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/85kb_2pxh_0.3A.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/94kb_16pxh_0.3A.bmp) |

Of course, this selective behavior comes with the consequence of being able to store less, alongside more space used for headers. 

Both BPCS implementations had to resort to using higher bits than the LSB, leading to obvious artifacting. This can be improved upon through modifying the algorithm parameters



At lower sizes, the result becomes much more accceptable:

| Original | 25KB LSB | 25KB BPCS 8x16 pixel |
|---|---|---|
| ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/original_image.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/25kb_LSB_output.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/25kb_16pxh_0.4A.bmp) |

## Future improvements
The current program uses a single constant noiseiness factor for all layers, which is used for both identifying viable blocks and evaluating written data.
Seperating these out would greatly improve visual quality and increase storage sizes. 

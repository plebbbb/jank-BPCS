# jank-BPCS
BPCS steganography, written in very messy C++

Basically, we are hiding data inside a file by modifying only the bits with the least impact on image quality.



LSB is an implementation where you store the data on the lowest bit of each byte of data in a file, leading to minimal observable change. The observable changes increase significantly as you use a higher bit.

BPCS improves upon this by determining the noiseiness of specific portions of the file, and then avoiding those which are too uniform. The written data is also obfuscated to make it noisy if it isn't noisy enough. At higher bits, this reduces the observable changes relative to LSB.

Below, 100KB of lorem ipsum text was written to the image:

| Original | 100KB LSB | 85KB BPCS 8 x 2 pixel 0.3A | 94KB BPCS 8 x 16 pixel 0.3A |
|---|---|---|---|
| ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/original_image.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/100kb_LSB_output.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/85kb_2pxh_0.3A.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/94kb_16pxh_0.3A.bmp) |

Of course, this selective behavior comes with the consequence of being able to store less, alongside more space used for headers. 

Both BPCS implementations had to resort to using higher bits than the LSB, leading to obvious artifacting. This can be improved upon through modifying the algorithm parameters



At lower sizes, the result becomes much more accceptable:

| Original | 25KB LSB | 25KB BPCS 8x16 pixel 0.4A |
|---|---|---|
| ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/original_image.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/25kb_LSB_output.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/25kb_16pxh_0.4A.bmp) |

To get a better idea of what steganography actually looks like, this is 100KB of lorem ipsum on a white background, with no threshold:

| 100KB LSB | 100KB BPCS 8 x 6 pixel -0.1A (no obfuscation) |
|---|---|
| ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/LSB_100kb_WB.bmp) | ![alt text](https://github.com/plebbbb/jank-BPCS/blob/main/Images/BPCS_6px_-0.1A_WB.bmp) |


## How to use
Go to the [releases page](https://github.com/plebbbb/jank-BPCS/releases) and download, then run it using a command line.

## Future improvements
The current program uses a single constant noiseiness factor for all layers, which is used for both identifying viable blocks and evaluating written data.
Seperating these out would greatly improve visual quality and increase storage sizes. 

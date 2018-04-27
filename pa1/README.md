# PA1 - Image and Signal Processing

## Overview
Basically, my implementation perform almost the same as the solution executable, except in randomDithering and FloydDithering some minor difference are presented.

Usage: ` ./image -<op> args ... <in.bmp >out.bmp  `

The input image is read from `stdin`, the output image is read from `stdout`. Nothing fancy was done to the CLI part.

## Binary
A binary could be found in the `bin` folder.

## Supported Operations

### 3.2 Basic Operations

#### 3.2.1 Image::Brighten(double factor) 

Usage:`./image -brightness <factor>`

`factor` should be a float value fall between [0,+inf). The larger the value, the brighter the image is.

|Original | 0.0 | 0.3 | 0.5 | 1.0 | 2.0 |
|--------- | --- | --- | --- | --- | --- |
|![Original](images/CAT.bmp) | ![0.0](images/bri00.bmp) | ![0.3](images/bri03.bmp) | ![0.5](images/bri05.bmp) | ![1.0](images/bri10.bmp) | ![2.0](images/bri20.bmp)|

#### 3.2.2 Image::ChangeContrast(double factor)

Usage: `./image -contrast <factor>`

`factor` should be a real number. 

|Original | -1.0 | -0.5 | 0.0 | 0.5 | 1.0 |
|--------- | --- | --- | --- | --- | --- |
|![Original](images/CAT.bmp) | ![-1.0](images/contn10.bmp) | ![-0.5](images/contn05.bmp) | ![0.0](images/cont00.bmp) | ![0.5](images/cont05.bmp) | ![1.0](images/cont10.bmp)|

#### 3.2.3 Image::ChangeSaturation(double factor)

Usage: `./image -saturation <factor>`

`factor` should be a real number. 

|Original | -1.0 | -0.5 | 0.0 | 0.5 | 1.0 |
|--------- | --- | --- | --- | --- | --- |
|![Original](images/CAT.bmp) | ![-1.0](images/satn10.bmp) | ![-0.5](images/satn05.bmp) | ![0.0](images/sat00.bmp) | ![0.5](images/sat05.bmp) | ![1.0](images/sat10.bmp)|

#### 3.2.4 Image::ChangeGamma(double factor)

Usage: `./image -gamma <factor>`

`factor = 1.0/gamma`, should be a real number. `factor` larger than 1.0 brighten the bright area in the image.

|Original | 0.0 | 0.7 | 1.0 | 1.5 | 2.0 |
|--------- | --- | --- | --- | --- | --- |
|![Original](images/CAT.bmp) | ![0.0](images/gam00.bmp) | ![0.7](images/gam07.bmp) | ![1.0](images/gam10.bmp) | ![1.5](images/gam15.bmp) | ![2.0](images/gam20.bmp)|

#### 3.2.5 Image::Crop(int x, int y, int w, int h)

Usage: `./image -crop <x> <y> <w> <h>`
Crop the image by a rectangular. The top-left point of the rectangular is (x,y) and the bottom-right is (x+w,y+h).

|Original | x=100, y=0, w=300, h=300|
|---------|-------------------------|
|![Original](images/CAT.bmp) | ![Cropped](images/crop.bmp)|

### 3.3 Quantization and Dithering

#### 3.3.1 Image::Quantize(int nbits)

Usage: `./image -quantize <nbit>`
Image was quantize to a more compact representation using nbit for each pixel. The quantized error are simply discarded.

| Original | 1 | 2 | 4 | 8 |
| -------- | - | - | - | - |
|![Original](images/CAT.bmp) | ![1](images/qua10.bmp) | ![2](images/qua20.bmp) | ![4](images/qua40.bmp) | ![8](images/qua80.bmp)|

#### 3.3.2 Image::RandomDither (int nbits)
Usage: `./image -randomDither <nbits>`
Add random noise before quantization to prevent aliasing pattern.

| Original | 1 | 2 | 4 | 8 |
| -------- | - | - | - | - |
|![Original](images/CAT.bmp) | ![1](images/ran10.bmp) | ![2](images/ran20.bmp) | ![4](images/ran40.bmp) | ![8](images/ran80.bmp)|

#### 3.3.3 Image::FloydSteinbergDither (int nbits)
Usage: `./image -FloydSteinbergDither <nbits>`
A more sophisticated quantization method which distributes quantized error to neighbouring pixels to reduce the overall error.

| Original | 1 | 2 | 4 | 8 |
| -------- | - | - | - | - |
|![Original](images/CAT.bmp) | ![1](images/flo10.bmp) | ![2](images/flo20.bmp) | ![4](images/flo40.bmp) | ![8](images/flo80.bmp)|

### 3.4 Basic Convolution and Edge Detection 

#### 3.4.1 Image::Blur (int n)
Usage: `./image -blur <n>`
A 2D Gaussian filter was implemented to blur the image. The n is the filter size, the standard derivation was choosed as the maximum value such that the filter size covers a 2 sigma region.

| Original | 3 | 5 | 7 | 11 | 13 | 19 |
| -------- | - | - | - | -  | -- | -- |
|![Original](images/CAT.bmp) | ![3](images/blu30.bmp) | ![5](images/blu50.bmp) | ![7](images/blu70.bmp) | ![11](images/blu110.bmp) | ![13](images/blu130.bmp) | ![19](images/blu190.bmp)|

#### 3.4.2 Image::Sharpen()
Usage: `./image -sharpen`

My sharpen filter was using a 3x3 suggested filter. The only tricky part is to clamp the Pixel to prevent overflow. 

| Original | Sharpened |
| -------- | --------- |
| ![Original](images/CAT.bmp) | ![Sharpened](images/sha1.bmp)

| Original | Sharpened |
| -------- | --------- |
| ![Original](images/DOG.bmp) | ![Sharpened](images/sha2.bmp)

#### 3.4.3 Image::EdgeDetect (int threshold)
Usage: `./image -edgeDetect <threshold>`

Sobel filter was implemented to detect the edge. The image is thresholded to a black/white image for better contrast.

| Original | Threshold=60 |
| -------- | --------- |
| ![Original](images/DOG.bmp) | ![60](images/edg1.bmp)


| Original | Threshold=70 |
| -------- | --------- |
| ![Original](images/CAT.bmp) | ![80](images/edg2.bmp)


### 3.5 Antialiased Scale and Shift

#### 3.5.1 Image::Scale(int sizex, int sizey)
Usage: `./image -size <sizex> <sizey>`

This is probably the most tricky part in the assignment. Basically, for each pixel this method sample both the original pixel and its neighbouring pixels, calculate a weight average of the pixels as the reconstructed pixel value with a proper filter specifying the corresponding weights. 

The nearest neighbouring filter simply choose the nearest pixel in the original image.
The hat filter would linearly interpolated the 1-manifold pixels in original image to get the final pixel.
The mitchell filter non-linearly weighted the 2-manifold pixels in original image to reconstruct the pixel.

| Size | NearestNeighbor | Hat | Mitchell |
| ---- | --------------- | --- | -------- |
| Original | ![Original](images/CKB.bmp)|![Original](images/CKB.bmp)|![Original](images/CKB.bmp)|
| 300x300  | ![](images/nea300300.bmp) | ![](images/hat300300.bmp) | ![](images/mit300300.bmp) |
| 512x300  | ![](images/nea512300.bmp) | ![](images/hat512300.bmp) | ![](images/mit512300.bmp) |
| 300x512  | ![](images/nea300512.bmp) | ![](images/hat300512.bmp) | ![](images/mit300512.bmp) |
| 800x800  | ![](images/nea800800.bmp) | ![](images/hat800800.bmp) | ![](images/mit800800.bmp) |

#### 3.5.2 Image::Shift(double sx, double sy)

Usage: `./image -shift <sx> <sy>`

| Shift | NearestNeighbor | Hat | Mitchell |
| ---- | --------------- | --- | -------- |
| Original | ![Original](images/BUGED.bmp)|![Original](images/BUGED.bmp)|![Original](images/BUGED.bmp)|
| (-30,50)  | ![](images/nea-3050.bmp) | ![](images/hat-3050.bmp) | ![](images/mit-3050.bmp) |
| (30,50)  | ![](images/nea3050.bmp) | ![](images/hat3050.bmp) | ![](images/mit3050.bmp) |
| (100,7)  | ![](images/nea1007.bmp) | ![](images/hat1007.bmp) | ![](images/mit1007.bmp) |
| (-150.5,234.3) | ![](images/shift0.bmp) | ![](images/shift1.bmp) | ![](images/shift2.bmp) |

### 3.6 Fun nonlinear filters


# ImageJ_leaf_segmentation
Segment and measure width and length of individual leafs/algaes in a mat


Created and test with ImageJ 1.53g java 1.8.0_172 64bits
Johnny Gan behappyftw@g.ucla.edu March 2021

For better results:
- do not put leafs/algae close (touching) each other (they will either be segmented as one big object or kicked by the circularity filter)

![image](https://user-images.githubusercontent.com/74852180/112717062-63348100-8ea7-11eb-926d-2aa8104be86c.png)

- dont put anything on the leaf/algae that could block it

![image](https://user-images.githubusercontent.com/74852180/112717069-76475100-8ea7-11eb-9b27-6037277473d8.png)

- avoid things you are not interested being in the picture (empty space, scissors, paper, etc)

![image](https://user-images.githubusercontent.com/74852180/112717020-21a3d600-8ea7-11eb-9c18-a4cb12d8e129.png)
![image](https://user-images.githubusercontent.com/74852180/112717029-2bc5d480-8ea7-11eb-901f-cf0f116be97a.png)
![image](https://user-images.githubusercontent.com/74852180/112717035-2f595b80-8ea7-11eb-9428-2f64767969aa.png)

- Keep imaging conditions (exposure mainly) constant between all images
- Try to flatten leaf as much as possible (wrinkles create highlights and shadows that confuse the program)

![image](https://user-images.githubusercontent.com/74852180/112717048-426c2b80-8ea7-11eb-8b41-62cdf1cfda93.png)

- avoid shadows and uneven lighting (see how left is much brigther than right. This affects the leaf/algae color/intensity)

![image](https://user-images.githubusercontent.com/74852180/112716990-033dda80-8ea7-11eb-9c28-bf604e6d4294.png)


Variables Explained:
The macro works by separating the image into its RGB channels. It assumes that the object is of very different color to that of the background (and everything else) and thus uses the channel with higher separation between background and object to segment. 

>ColorChannel = 3;
To select which channel to use, open your image in ImageJ, then Image>Type>RGB Stack and scroll and see which one has the highest contrast.

The image is then filteres with a median filter and and unsharp mask is applied to increase contrast and separation between object and background using.
Depending on zoom and image resolution, one might have to change this values. Bigger images will have higher radius. 
Change filter with:
>MedianRadius = 20; 
>
>UnsharpRadius = 100; 
>
>UnsharpMask = 0.8; 


From this image, the histogram is analyzed. One can change 
>HistogramBins = 128; 
To reduce the number of bins. This will "smoothen" out the peaks but lower the resolution. So if two peaks are very close together, lowering this migh merge them.

The macro then uses the histogram to find the two peaks, one that represents the object (darkest peak) and the background (lightest peak). It then uses the peaks as reference to set the threshold based on the user variables:
>LowerPeakTHR = 30; 
>
>HigherPeakTHR = 60; 
If a peak is at 100 intensity, then in this case it would select objects with intensity 70 (peak-LowerPeakTHR) to 160 (peak+HigherPeakTHR). This allows to fine tune the selection of object based on how much the intensity deviation in the object. This is why it is important for the object to be as even lighting as possible. The more even, the less deviation and better separation. If the macro is selecting too many bright areas, lower the HigherPeakTHR. if too many dark, then lower LowerPeakTHR. and vice versa.

The objects are then turned into ROIs and filters
>minsize = 1600;
removes all ROIs smaller than minsize and 
>mincircularity
Removes all ROIs will lower ciruclarity than mincircularity. This can be straight lines or really iregular shapes (like touching leafs that make an L shape or star shape)



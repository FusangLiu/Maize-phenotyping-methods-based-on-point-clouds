# Maize-phenotyping-methods-based-on-point-clouds
There are maize phenotyping methods including organ segmentation, leaf traits extraction, etc.

# Reference 
Liu, F., Song, Q., Zhao, J., Mao, L., Bu, H., Hu, Y. and Zhu, X.-G. (2021), Canopy occupation volume as an indicator of canopy photosynthetic capacity. New Phytologist. NPH17611. https://doi.org/10.1111/nph.17611

# Software information
This software is based on MATLAB R2020b.Before you use the software, you should verify that version 9.9 (R2020b) of the MATLAB Runtime is installed.   
If not, you can download and install the Windows version of the MATLAB Runtime for R2020b 
from the following link on the MathWorks website:
https://www.mathworks.com/products/compiler/mcr/index.html


# Maize Organ Segmentation based on Point Clouds (MOSPC)
Input:
input file path, point clouds name, output file path, output plant model name, KNN number of skeletonization (default 20),sample scale of skeletonization (default 0.025), dispaly of skeletonization (default 0), dispaly of classfication result based on the skeleton (default 0),KNN number of region growing for whole plant(default 5),threshold of Curvature of region growing for whole plant (default 0.1),threshold of angle of region growing for whole plant (default 30), display of region growing for whole plant (default 0), KNN number of region growing for unclassfied points(default 5), threshold of Curvature of region growing for unclassfied points(default 0.1),threshold of angle of region growing for unclassfied points (default 15), display of region growing for unclassfied points ((default 0), threshold of angle for classifying stem (default 10), threshold of cluster size for merging unclassified clusters(default 20), display of the segmentation result (default 1) 

Output: organ point clouds


Note: 
1) The input point clouds is must from the PLY or PCD file
2) The output point clouds is the PLY file.


Example:
Using the software in the windows DOS command line:

E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1"


Using the software in the MATLAB:

cmd=['E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1"']; 
system(cmd)



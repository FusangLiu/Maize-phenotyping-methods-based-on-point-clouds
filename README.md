# Maize-phenotyping-methods-based-on-point-clouds
There are maize phenotyping methods including organ segmentation, leaf traits extraction, etc.

# Reference 
Liu, F., Song, Q., Zhao, J., Mao, L., Bu, H., Hu, Y. and Zhu, X.-G. (2021), Canopy occupation volume as an indicator of canopy photosynthetic capacity. New Phytologist. NPH17611. https://doi.org/10.1111/nph.17611

# Software information
These softwares are based on MATLAB R2020b.Before you use the software, you should verify that version 9.9 (R2020b) of the MATLAB Runtime is installed.   
If not, you can download and install the Windows version of the MATLAB Runtime for R2020b 
from the following link on the MathWorks website:
https://www.mathworks.com/products/compiler/mcr/index.html


# Maize Organ Segmentation based on Point Clouds (MOSPC)
This method based on the skeleton and region growing algorithm was developed to segment plant organs, i.e., leaves and a stem. Firstly, the denoised point clouds of each plant were processed with a Laplacian based method to generate plant skeleton points, a connectivity matrix of skeleton points and an incidence matrix between point clouds and skeleton points (Fig. 1a). The Laplacian based method is a contraction operation that is designed to work on point clouds via local Delaunay triangulation and topological thinning. Secondly, a region growing algorithm was used to divide point clouds into clusters (Fig. 1d) according to the curvatures and angles of the normal vectors changing along the organ surface. The plant organ segmentation includes three parts, i.e., leaf skeleton extraction, classification of point clouds clusters and unclassified clusters merging. Reference leaf point clouds (Fig. 1c) were obtained from the plant point clouds according to the leaf skeletons. According to reference leaf point clouds and angles between clusters and stem direction, small clusters (Fig. 1d) were classified to organ point clouds (Fig. 1e) and unclassified point clouds. The unclassified point clouds were classified into clusters by the region growing method (Fig. 1f). The larger clusters from unclassified point clouds were merged into organ point clouds (Fig. 1g) according to the similarity of normal and curvature of points between the larger cluster and neighbouring organ point clouds. Finally, the remaining clusters were merged into organ point clouds by a Euclidean distance-based method. 

![alt text](https://github.com/FusangLiu/Maize-phenotyping-methods-based-on-point-clouds/blob/main/MOSPC/Fig_2.jpg)

Fig.1 A pipeline of Zea mays plant organ segmentation. The plant point clouds of A619 at 37 DAS were transferred to a skeleton by a Laplacian based method (a) and clusters based on the region growing method (d). Leaf skeletons (b) were extracted from the plant skeleton. Reference leaf point clouds (c) were obtained from the plant point clouds according to the leaf skeletons. According to reference leaf point clouds and angles between clusters and stem direction, small clusters (d) were classified to organ point clouds (e) and unclassified point clouds. The unclassified point clouds were classified into clusters by the region growing method (f). The larger clusters from unclassified point clouds were merged into organ point clouds (g) according to the similarity of normal and curvature of points between the larger cluster and neighbouring organ point clouds. Finally, the remaining clusters were merged into organ point clouds by a Euclidean distance-based method (h). 

## Input and Output
Input:
input file path, point clouds name, output file path, output plant model name, KNN number of skeletonization (default 20),sample scale of skeletonization (default 0.025), dispaly of skeletonization (default 0), dispaly of classfication result based on the skeleton (default 0),KNN number of region growing for whole plant(default 5),threshold of curvature of region growing for whole plant (default 0.1),threshold of angle of region growing for whole plant (default 30), display of region growing for whole plant (default 0), KNN number of region growing for unclassfied points(default 5), threshold of curvature of region growing for unclassfied points(default 0.1),threshold of angle of region growing for unclassfied points (default 15), display of region growing for unclassfied points ((default 0), threshold of angle for classifying stem (default 10), threshold of cluster size for merging unclassified clusters(default 20), display of the segmentation result (default 1) 

Output: organ point clouds


## Notes: 
1) The input point clouds is must from the PLY or PCD file
2) The output point clouds is the PLY file.
3) The density of point clouds can affect the skeletonization result hence affecting the final result. However, the sparse point clouds can calculate faster. 


## Examples:
### Using the software in the windows DOS command line:

E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1"

E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1" 20 0.025 1 

E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1" 20 0.025 0 0 5 0.1 30 0 5 0.1 15 0 10 20 1

### Using the software in the MATLAB:

cmd=['E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1"']; 

system(cmd)

cmd=['E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1" 20 0.025 1']; 

system(cmd)

cmd=['E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1" 20 0.025 0 0 5 0.1 30 0 5 0.1 15 0 10 20 1']; 

system(cmd)


### The software supports parallel computing in the MATLAB.

parfor i=1:3

str_in=['test_big_',num2str(i),'.ply'];

str_out=['big_',num2str(i)];

cmd=strcat("E:\MOSPC\MOSPC.exe ","E:\MOSPC ",str_in," E:\MOSPC ",str_out);

system(cmd)

end

# Extraction of Leaf Traits (ELT)
The method extract leaf traits based on the leaf point clouds including Leaf length, maximal leaf width, leaf angle, leaf area, etc.
The leaf base point is defined as the closest point to the stem point clouds. The leaf midrib was rotated to the positive direction of the x-axis around the leaf base point (Fig. 2a). The rotated angle was decided by the main direction of plant point clouds in the horizontal plane (xoy plane in Fig. 2a) estimated by the Principal Component Analysis (PCA) algorithm. The leaf tip point was found mainly by three steps. Frist, leaf point clouds were divided into two segments, with the Part 1 from the leaf base to the highest point of the leaf and Part 2 from the highest point of the leaf to the leaf tip. Then the pcsegdist function was used to find the outliers according to a Euclidean distance in Part 1. If there is no outlier in Part 1, the lowest point in Part 2 was regarded as the leaf tip point (Fig. 2a); otherwise, the lowest point in the outliers in Part 1 was regarded as the leaf tip point (Fig. 2a, sub-figure). Leaf length (Fig. 2a, red line) was the distance of the shortest path between the leaf base and the leaf tip. Leaf width (Fig. 2a, yellow line) was the distance of the shortest path between the points in two leaf edges. The shortest path was calculated by the Dijkstra method which is an algorithm for finding the shortest paths between nodes in a graph. Leaf width at 0, 1/4, 1/2 and 3/4 of leaf length from the leaf base was extracted. Leaf angle was defined as the angle between the vector from the base point to the point of the leaf midrib at 1/3 of leaf length from the leaf base and the vertical direction (Fig. 2a).The leaf area was estimated by a meshed leaf model consisting of triangles(Fig. 2b). The leaf area (LA) was calculated by the sum of triangle areas. Point clouds were transferred to the meshed leaf model based on a Crust-based method(https://github.com/FusangLiu/Surface-reconstruction-from-point-clouds-for-crop-canopies) 

![alt text](https://github.com/FusangLiu/Maize-phenotyping-methods-based-on-point-clouds/blob/main/ELT/Fig_3.PNG)
Fig. 2 An example of Zea mays leaf architectural trait extraction. Leaf length (a, red line) is the distance of the shortest path between the leaf base and the leaf tip. Leaf width (a, yellow line) is the distance of the shortest path between points between leaf edges. The smooth leaf edge (a, blue line), which is defined as the leaf edge with the distortion and undulation removed, was used to evaluate the leaf flatness and leaf smoothness. The sub-figure is a special case during the extraction of the leaf tip, purple and grey lines indicate points belonging to Part 1(a, purple line) and Part 2(a, grey line), respectively. Leaf area was calculated by the meshed point clouds (b)


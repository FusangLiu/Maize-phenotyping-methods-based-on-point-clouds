# Maize-phenotyping-methods-based-on-point-clouds
There are maize phenotyping methods including organ segmentation, leaf traits extraction, crop ideltype method ,etc.

# Reference 
Liu, F., Song, Q., Zhao, J., Mao, L., Bu, H., Hu, Y. and Zhu, X.-G. (2021), Canopy occupation volume as an indicator of canopy photosynthetic capacity. New Phytologist. NPH17611. https://doi.org/10.1111/nph.17611

# Software information
These softwares are based on MATLAB R2020b.Before you use the software, you should verify that version 9.9 (R2020b) of the MATLAB Runtime is installed.   
If not, you can download and install the Windows version of the MATLAB Runtime for R2020b 
from the following link on the MathWorks website:
https://www.mathworks.com/products/compiler/mcr/index.html


# Maize Organ Segmentation based on Point Clouds (MOSPC)
This method based on the skeleton and region growing algorithm was developed to segment plant organs, i.e., leaves and a stem. Firstly, the denoised point clouds of each plant were processed with a Laplacian based method to generate plant skeleton points, a connectivity matrix of skeleton points and an incidence matrix between point clouds and skeleton points (Fig. 1a). The Laplacian based method is a contraction operation that is designed to work on point clouds via local Delaunay triangulation and topological thinning. Secondly, a region growing algorithm was used to divide point clouds into clusters (Fig. 1d) according to the curvatures and angles of the normal vectors changing along the organ surface. The plant organ segmentation includes three parts, i.e., leaf skeleton extraction, classification of point clouds clusters and unclassified clusters merging. Reference leaf point clouds (Fig. 1c) were obtained from the plant point clouds according to the leaf skeletons. According to reference leaf point clouds and angles between clusters and stem direction, small clusters (Fig. 1d) were classified to organ point clouds (Fig. 1e) and unclassified point clouds. The unclassified point clouds were classified into clusters by the region growing method (Fig. 1f). The larger clusters from unclassified point clouds were merged into organ point clouds (Fig. 1g) according to the similarity of normal and curvature of points between the larger cluster and neighbouring organ point clouds. Finally, the remaining clusters were merged into organ point clouds by a Euclidean distance-based method. 

![alt text](https://github.com/FusangLiu/Maize-phenotyping-methods-based-on-point-clouds/blob/main/MOSPC/Fig_2.PNG)

Fig.1 A pipeline of Zea mays plant organ segmentation. The plant point clouds of A619 at 37 DAS were transferred to a skeleton by a Laplacian based method (a) and clusters based on the region growing method (d). Leaf skeletons (b) were extracted from the plant skeleton. Reference leaf point clouds (c) were obtained from the plant point clouds according to the leaf skeletons. According to reference leaf point clouds and angles between clusters and stem direction, small clusters (d) were classified to organ point clouds (e) and unclassified point clouds. The unclassified point clouds were classified into clusters by the region growing method (f). The larger clusters from unclassified point clouds were merged into organ point clouds (g) according to the similarity of normal and curvature of points between the larger cluster and neighbouring organ point clouds. Finally, the remaining clusters were merged into organ point clouds by a Euclidean distance-based method (h). 

## Input and Output
Input:
input file path, point clouds name, output file path, output plant model name, KNN number of skeletonization (default 20),sample scale of skeletonization (default 0.025), dispaly of skeletonization (default 0), dispaly of classfication result based on the skeleton (default 0),KNN number of region growing for whole plant(default 5),threshold of curvature of region growing for whole plant (default 0.1),threshold of angle of region growing for whole plant (default 30), display of region growing for whole plant (default 0), KNN number of region growing for unclassfied points(default 5), threshold of curvature of region growing for unclassfied points(default 0.1),threshold of angle of region growing for unclassfied points (default 15), display of region growing for unclassfied points (default 0), threshold of angle for classifying stem (default 10), threshold of cluster size for merging unclassified clusters(default 20), display of the segmentation result (default 1) 

Output: organ point clouds


## Notes: 
1) The input point clouds must be from the PLY or PCD file
2) The output point clouds is the PLY file.
3) The density of point clouds can affect the skeletonization result hence affecting the final result. However, the sparse point clouds can calculate faster. 


## Examples:
### Using the software in the windows DOS command line:

E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1"

E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1" 20 0.025 1 

E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1" 20 0.025 0 0 5 0.1 30 0 5 0.1 15 0 10 20 1

### Using the software in MATLAB:

cmd=['E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1"']; 

system(cmd)

cmd=['E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1" 20 0.025 1']; 

system(cmd)

cmd=['E:\MOSPC\MOSPC.exe "E:\MOSPC" "test_small_1.ply" "E:\MOSPC" "small_1" 20 0.025 0 0 5 0.1 30 0 5 0.1 15 0 10 20 1']; 

system(cmd)


### The software supports parallel computing in MATLAB.

parfor i=1:3

str_in=['test_big_',num2str(i),'.ply'];

str_out=['big_',num2str(i)];

cmd=strcat("E:\MOSPC\MOSPC.exe ","E:\MOSPC ",str_in," E:\MOSPC ",str_out);

system(cmd)

end

# Extraction of Leaf Traits (ELT)
The method extract leaf traits based on the leaf point clouds including Leaf length, maximal leaf width, leaf angle, leaf area, etc.

### Finding the leaf tip point
The leaf base point is defined as the closest point to the stem point clouds. The leaf midrib was rotated to the positive direction of the x-axis around the leaf base point (Fig. 2a). The rotated angle was decided by the main direction of plant point clouds in the horizontal plane (xoy plane in Fig. 2a) estimated by the Principal Component Analysis (PCA) algorithm. The leaf tip point was found mainly by three steps. Frist, leaf point clouds were divided into two segments, with the Part 1 from the leaf base to the highest point of the leaf and Part 2 from the highest point of the leaf to the leaf tip. Then the pcsegdist function was used to find the outliers according to a Euclidean distance in Part 1. If there is no outlier in Part 1, the lowest point in Part 2 was regarded as the leaf tip point (Fig. 2a); otherwise, the lowest point in the outliers in Part 1 was regarded as the leaf tip point (Fig. 2a, sub-figure). 

### Leaf traits
Leaf length (Fig. 2a, red line) was the distance of the shortest path between the leaf base and the leaf tip. Leaf width (Fig. 2a, yellow line) was the distance of the shortest path between the points in two leaf edges. The shortest path was calculated by the Dijkstra method which is an algorithm for finding the shortest paths between nodes in a graph.

Leaf width at 0, 1/4, 1/2 and 3/4 of leaf length from the leaf base was extracted. 

Leaf angle was defined as the angle between the vector from the base point to the point of the leaf midrib at 1/3 of leaf length from the leaf base and the vertical direction (Fig. 2a).

Smooth leaf edges, which is defined as the leaf edge with the distortion and undulation removed, were estimated by an empirical leaf shape model based on the calculated leaf length and four leaf width values. The smooth leaf edge (Fig. 2a, blue line; Eqn. 1) was generated by the path of leaf length (Fig. 2a, red line): 

(Xs,Ys,Zs )=(Xp,Yp,Zp )±(0,0.5*LWp,0)             (1)

where the xs, ys, zs are coordinates of points of the smooth leaf edge, xp. yp. zp are coordinates of points in the path of leaf length, LWp is the leaf width at the corresponding point in the path of leaf length.

Leaf flatness was defined as the ratio between the area surrounded by the projected smooth leaf edge (PAe) and leaf area estimated by leaf shape model (LAe). Leaf smoothness (Ls) was defined as the ratio between the projected area of point clouds (PAp) and the area surrounded by the projected smooth leaf edge (PAe). 

The leaf area was estimated by a meshed leaf model consisting of triangles(Fig. 2b). The leaf area (LA) was calculated by the sum of triangle areas. Point clouds were transferred to the meshed leaf model based on a Crust-based method(https://github.com/FusangLiu/Surface-reconstruction-from-point-clouds-for-crop-canopies) 

![alt text](https://github.com/FusangLiu/Maize-phenotyping-methods-based-on-point-clouds/blob/main/ELT/Fig_3.PNG)
Fig. 2 An example of Zea mays leaf architectural trait extraction. Leaf length (a, red line) is the distance of the shortest path between the leaf base and the leaf tip. Leaf width (a, yellow line) is the distance of the shortest path between points between leaf edges. The smooth leaf edge (a, blue line), which is defined as the leaf edge with the distortion and undulation removed, was used to evaluate the leaf flatness and leaf smoothness. The sub-figure is a special case during the extraction of the leaf tip, purple and grey lines indicate points belonging to Part 1(a, purple line) and Part 2(a, grey line), respectively. Leaf area was calculated by the meshed point clouds (b)

## Input and Output
Input: input stem file path,input leaf file path, output name, scale of converting to a centimeter, display


Output：txt file including LeafBase, leafLength.Leaf width at 0.1, 1/4, 1/2 and 3/4 of leaf length from the leaf base (i.e, leafWidth(0.1),leafWidth(0.25), leafWidth(0.5),  leafWidth(0.75)), leafAngle, The leaf area (leafArea_tri), The leaf area estimated by leaf shape model(LeafArea_shape), PAp(projectedArea_pc), PAe(projectedArea_se)

## Notes: 
1) The input point clouds must be from the PLY or PCD file

## Examples:
### Using the software in the windows DOS command line:

E:\ELT\ELT.exe "E:\ELT\stem_t.ply" "E:\ELT\leaf_t1.ply" "E:\ELT\leaf_t1" 100 1

### Using the software in MATLAB:

cmd=['E:\ELT\ELT.exe "E:\ELT\stem_t.ply" "E:\ELT\leaf_t1.ply" "E:\ELT\leaf_t1" 100 1']; 

system(cmd)

### The software supports parallel computing in MATLAB.
parfor i=1:3

cmd=strcat("E:\ELT\ELT.exe ","E:\ELT\stem_t.ply ","E:\ELT\leaf_t",num2str(i),".ply ","E:\ELT\leaf_t",num2str(i)," 100 1"');

system(cmd)

end

# Crop Idealtype Design method based on Point Clouds (CIDPC)
The method can change the leaf architecture(leaf length, width and angle) via changing the coordinates of leaf point clouds. At first, the point clouds of selected leaves were standardised with three steps. 1) The main direction of the selected leaf in the horizontal plane was calculated by the x and y coordinates of point clouds of the selected leaf based on the PCA algorithm. 2) The angle between the main direction and the positive direction of the x-axis was calculated. 3) The point clouds of the selected leaf were rotated to the positive direction of the x-axis around the leaf base point according to the calculated angle (Fig. 3a). Secondly, the corresponding leaf architecture was changed. In terms of changing leaf length (Fig. 3a), the x and z coordinates of the point clouds of the selected leaf were multiplied by the set percentages change of leaf length to increase or decrease the leaf length. Regrading changing leaf width (Fig. 3b), the y coordinates of the point clouds of the selected leaf were multiplied by the set percentages change of leaf width to increase or decrease the leaf width. To ensure the accuracy of percentages change in leaf length and leaf width, the actual percentages change was calculated by the changed leaf length or the maximal leaf width dividing original leaf length or the maximal leaf width, and the actual percentages change was compared with the set percentages change. If the relative error between the actual percentages change and the set percentages change is more than 5%, the changed point clouds will change again with a new percentage change calculated by the set percentages change dividing the actual percentage change. In terms of changing leaf angle (Fig. 3c), coordinates of the point clouds of the selected leaf were multiplied by corresponded rotated matrix to rotate around the y-axis with the set percentages change. If the leaf angle of an individual leaf decreases to 0 degrees, the leaf angle of the individual leaf will stay unchanged rather than becoming a negative value. Similarly, if the leaf angle of an individual leaf increases to 90 degrees, the leaf angle of the individual leaf will stay unchanged. Finally, after changing the architectural traits of leaves, the changed leaves were rotated to their original position.
![alt text](https://github.com/FusangLiu/Maize-phenotyping-methods-based-on-point-clouds/blob/main/CIDPC/Fig_4.PNG)
Fig. 3 An example of the new ideotype design method. The example is based on point clouds of Zea mays plants. The leaf length (a) and the leaf width (b) of original point clouds (red point clouds) were increased by 20% (blue point clouds). The leaf angle (c) of original point clouds was reduced by 20 degrees. 

## Input and Output
Input: input stem file path,input leaf file path, output path,output name,the percentage change of leaf length,the percentage change of leaf width,the percentage change of leaf angle, display

Output：the changed leaf point clouds

## Notes: 
1) The input point clouds must be from the PLY or PCD file
2) The output point clouds is the PLY file.

## Examples:
### Using the software in the windows DOS command line:
E:\CIDPC\CIDPC.exe "E:\CIDPC\stem_t.ply" "E:\CIDPC\leaf_t1.ply" "E:\CIDPC" "leaf_t1" 1.2 1.2 0.5 1

### Using the software in MATLAB:
cmd=['E:\CIDPC\CIDPC.exe "E:\CIDPC\stem_t.ply" "E:\CIDPC\leaf_t1.ply" "E:\CIDPC" "leaf_t1" 1.2 1.2 0.5 1'];
system(cmd)

### The software supports parallel computing in MATLAB.
parfor i=1:3

cmd=strcat("E:\CIDPC\CIDPC.exe ","E:\CIDPC\stem_t.ply ","E:\CIDPC\leaf_t",num2str(i),".ply ","E:\CIDPC ","leaf_t",num2str(i)," 1.2 1.2 0.5 1"');

system(cmd)

end

# Canopy Occupation Volume (COV)
The canopy occupation volume (COV) can better explain the variations of canopy photosynthetic capacity. Specifically, COV can explain more than 79% variations of canopy photosynthesis generated by changing leaf angle and more than 84% variations of canopy photosynthesis generated by changing leaf area.To calculate the canopy occupation volume, the canopy model was voxelized (one voxel/1 cm thickness; Fig. 4a), and the voxels containing triangles of the canopy model are regarded as the occupied voxels (Fig. 4, yellow voxels). The total volume of the occupied voxels is the canopy occupation volume. 
![alt text](https://github.com/FusangLiu/Maize-phenotyping-methods-based-on-point-clouds/blob/main/COV/Fig_5.PNG)
Fig. 4 The canopy occupation volume in normal view (a), front view (b) and top view (c) of a Zea mays plant. The space of the meshed plant was divided into a set of small 3D voxels (blue voxels). If a voxel contains triangles of meshed plant, the voxel is regarded as the occupied voxel (yellow voxels). The sum of occupied voxel volume is the canopy occupation volume. The size of voxels is 20 cm×20 cm×20cm in this main figure and is 1 cm×1 cm×1cm in calculation (a, sub-fig). 

## Input and Output
Input input model file path,input file path,scale of converting to a meter(default 1), voxel size(default 0.01), display(default 0)

Output：txt file including COV

## Notes: 
1) The input canopy model must be from the PLY file. Canopy point clouds can be transferred to the meshed canopy model based on a Crust-based method(https://github.com/FusangLiu/Surface-reconstruction-from-point-clouds-for-crop-canopies) 

## Examples:
### Using the software in the windows DOS command line:
E:\COV\COV.exe "E:\COV\example_maize_pm.ply" "E:\COV\example_maize" 
E:\COV\COV.exe "E:\COV\example_maize_pm.ply" "E:\COV\example_maize" 1 0.3 1

### Using the software in MATLAB:
cmd=['E:\COV\COV.exe "E:\COV\example_maize_pm.ply" "E:\COV\example_maize"'];
system(cmd)

cmd=['E:\COV\COV.exe "E:\COV\example_maize_pm.ply" "E:\COV\example_maize" 1 0.3 1'];
system(cmd)

### The software supports parallel computing in MATLAB.
parfor i=1:2

cmd=strcat("E:\COV\COV.exe ","E:\COV\example_maize_pm_",num2str(i),".ply ","E:\COV\example_maize_pm_",num2str(i)," 1 0.3 1");

system(cmd)

end

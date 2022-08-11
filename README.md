# EdgeVision/Innopolis
# Camera Calibration in real world for precise object positioning
camera calibration in real world for precise object positioning for surveillance camera
The goal of camera calibration is to find the intrinsic and extrinsic parameters of a AXIS P1364 Network Camera .
# Used Method:
Inorder to calibrate this surviellence camera, we take one camera image and take the 3D coordinates of objects that are known with more than six points and use the 3D-2D point correspondences between the 3D points and its 2d camera image to find the camera parameters.
Satellite images might be used to map the coordinates and I'm still working on it.
# Other Proposed Methods:
2. using a person with known Hight and make him stand in N different places across the monitored area locations and use this information (his length) to calculate intrinsic matrix .
3. calibrating using Vanishing points:
     a. Vehicle Vanishing point ( no specific car model required)
     b. vanishing points in a vision system (roads  and intersections are full   of lines that can be used to calculate vanishing points)
4. most complicated : using 3D Model Bounding Box Alignment
5. in the light of option [2] we can also include the pose (sort of pose estimation) of the person for more accurate calibration (Hight of the person , and width of the shoulders)
# Intrinsic parameter
The calibrating image that we use is from the surroundings of the monitored are as you can see in this picture

![Intern 6 8-8 8](https://user-images.githubusercontent.com/90598253/183605110-e6531bb2-cb19-410e-b07e-9c957abd5280.png)

                                                             fig 1

and the object whith known (measured) dimentions in the camera picture can be used to calibrate as points

The matrix K which relates the 3D to the 2D points is an upper triangular matrix with the following shape, where αx, αy represents the focal length in terms of the x and y pixel dimensions,px and py are the principal points and s is the skew parameter.
![image](https://user-images.githubusercontent.com/90598253/183483698-bcc3f90f-21be-42ea-a172-1dcc1d072b16.png)


The relation between the 2D point (x, y) and the corresponding 3D point (X, Y, Z) are given by

x = αx(X/Z) + s(Y/Z) + px

y = αy(Y/Z) + py

# Intrinsic and extrinsic parameter computation

In the previous part, we have computed the intrinsic parameter assuming that the extrinsic parameters are known, i.e., we assumed that we know the 3D point correspondences in the camera coordinate system. But this is rarely the case; almost always we know the 3D point correspondences only in the world coordinate system and hence we need to estimate both the intrinsic and extrinsic parameters. But before that we need to obtain the 3D-2D point correspondences. The 3D points are described with respect to a world coordinate system as shown in the figure 1. The figure shows the x,y and z axes of the world coordinate system along with some sample 3D points, which are featured points, There should be more than 6 points, but we have 7 points.

  -The 3D points in the world coordinate system are provided in 3D_pts.mat and the corresponding 2D points on the image are provided in 2D_pts.mat

  -Next we want to compute the camera projection matrix P = K[R t], where K is the internal/intrinsic calibration matrix, R is the rotation matrix which specifies the    orientation of the camera coordinate system w.r.t the world coordinate system and t is the translation vector which species the location of the camera center in the    world coordinate system.

  -For computing P, the code is in the function “calibrate(x,X)” and is based on the “Direct Linear Transformation (DLT)" The DLT is an important algorithm to            understand and is detailed below.
  
 # Discrete Linear Transform (DLT)

The Discrete Linear Transorm(DLT) is simple linear algorithm for estimating the camera projection matrix P from corresponding 3-space and image entities. This computation of the camera matrix is known as resectioning. The simplest such correspondence is that between a 3D point X and its image x under the unknown camera mapping. Given sufficiently many such correspondences the camera matrix may be determined.


Let’s assume a number of point correspondences between 3D points and 2D image points are given. The camera matrix is a 3x4 matrix which relates the points by, xi = P.Xi For each correspondence Xi ↔ xi , we get three equations of which two are linearly independent and is described below

1.From a set of n point correspondences, we obtain a 2nx12 matrix A by stacking up the equations of the above form for each correspondence

2.Obtain the SVD of A. The unit singular vector corresponding to the smallest singular value is the solution p. Specifically, if A = UDVT with D diagonal with positive diagonal entries, arranged in descending order down the diagonal, then p is the last column of V

3.Obtain p and write it in matrix form to get the matrix P

![imageedit_5_5151063270](https://user-images.githubusercontent.com/90598253/183870222-2ee4ec79-df8e-4656-b065-d4f172f37a6c.png)

The projection matrix P is computed by solving the set of equations Ap = 0, where p is the vector containing the entries of the matrix P.

and as we mentioned befor we can use n>=6 points to do the calibration and the calculations and we can prove that.

now we have The decomposition of P into K,R,t is done as by RQ decomposition. It involves calculating the decomposition A = R Q where Q is unitary/orthogonal and R upper triangular.

# Verifying the accuracy of the calibration process

For this we will compute the re-projection error, which is a measure of the distance between the 2D points and the 2D points obtained by projecting the 3D points using the computed camera parameters.

     References

[1] Huang, Shiyao, et al. "Camera calibration from periodic motion of a pedestrian." Proceedings of the IEEE conference on computer vision and pattern recognition. 2016. 

[2] Kocur, Viktor, and Milan Ftáčnik. "Traffic Camera Calibration via Vehicle Vanishing Point Detection." International Conference on Artificial Neural Networks. Springer, Cham, 2021.

[3] Sochor, Jakub, Roman Juránek, and Adam Herout. "Traffic surveillance camera calibration by 3d model bounding box alignment for accurate vehicle speed measurement." Computer Vision and Image Understanding 161 (2017): 87-98.

[4] R. Hartley and A. Zissermann, Multiview geometry, 2nd edition, Cambridge University Press.

[5] Z. Zhang. A flexible new technique for camera calibration. IEEE Transactions on Pattern Analysis and Machine Intelligence, 22(11):1330-1334, 2000.     


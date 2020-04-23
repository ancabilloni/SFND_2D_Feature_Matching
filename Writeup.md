# Camera Based 2D Feature Tracking

###I. Project Description and Build Instruction
Refer to [Install.md](./Install.md)

###II. Tasks
####1. Data Buffer
(MP.1)
- Allows only a fixed number of `DataFrame` in the `dataBuffer` vector. The approach is to check the `dataBuffer` size and erase the first element in the vector if the size already exceeded the allowed buffer size. So the new data can be appended into the `dataBuffer` and not violate the allow buffer size.
- Cons: this approach is not an efficient approach to handle large queue size. Because everytime an element is deleted, the rest of the elements will be shifted. Therefore, more run time will go into the shifting operations. However, in this project, the buffer size is small which may not be a big concern for run time cost.

####2. Keypoints
2.1 Keypoint Detection (MP.2)
- HARRIS detector is implemented in `detKeypointHarris` in `matching2D_Student.cpp` using OpenCV library to output detected keypoints. The parameters are set based on lecture. 
- FAST, BRISK, ORB, AKAZE and SIFT are implemented in `detKeypointsModern` in`matching2D_Student.cpp`, also using OpenCV library.  
- The detector type can be selectable by changing the string variable `detectorType` in `MidTermProject_Camera_Student.cpp` according. 

2.2 Remove Keypoints Outside of pre-defined rectangle (MP.3)
- Every detected keypoint as the result of 2.1 goes through position check. If x or y coordinate of any point locate outside of pre-defined rectangle, the point is erased from the dynamic list.  

####3 Descriptors
3.1 Keypoint Descriptor (MP.4)
- BRIEF, ORB, FREAK, AKAZE and SIFT descriptors are implemented in `desKeypoints` in `matching2D_Student.cpp`. The type of descriptor is selectable by setting the string variable `descriptorType` in `MidTermProject_Camera_Student.cpp` accordingly. 

3.2 Descriptor Matching & Distance Ratio (MP.5 & MP.6)
- Both BF and FLANN matching are implemented in `matchDescriptors` in `matching2D_Student.cpp`. The matchers are from OpenCV library. Either method is selectable by setting string variable `matcherType` in `MidTermProject_Camera_Student.cpp`

- Both Nearest Neighbor and K-Nearest-Neighbor selector types are implemented following the matcher in `matchDescriptors` function. While NN finds the best match, the KNN matching find the best and second best match. The distance ratio test between best vs. second best filter out the bad keypoint match. In this case, the chosen distance ratio is 0.8.

####4 Performance Evaluation
4.1 Count number of keypoints of the preceding vehicle in all 10 images (MP.7)

4.2 Count the matched keypoints with all possible Detectors & Descriptors (MP.8)

4.3 Log the time it takes for keypoint detection and descriptor extraction (MP.9)

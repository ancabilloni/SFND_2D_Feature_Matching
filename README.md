# Camera Based 2D Feature Tracking

### I. Project Description and Build Instruction
Refer to [Install.md](./Install.md)

### II. Tasks
#### 1. Data Buffer
(MP.1)
- Allows only a fixed number of `DataFrame` in the `dataBuffer` vector. The approach is to check the `dataBuffer` size and erase the first element in the vector if the size already exceeded the allowed buffer size. So the new data can be appended into the `dataBuffer` and not violate the allow buffer size.
- Cons: this approach is not an efficient approach to handle large queue size. Because everytime an element is deleted, the rest of the elements will be shifted. Therefore, more run time will go into the shifting operations. However, in this project, the buffer size is small which may not be a big concern for run time cost.

#### 2. Keypoints
##### 2.1 Keypoint Detection (MP.2)
- HARRIS detector is implemented in `detKeypointHarris` in `matching2D_Student.cpp` using OpenCV library to output detected keypoints. The parameters are set based on lecture. 
- FAST, BRISK, ORB, AKAZE and SIFT are implemented in `detKeypointsModern` in`matching2D_Student.cpp`, also using OpenCV library.  
- The detector type can be selectable by changing the string variable `detectorType` in `MidTermProject_Camera_Student.cpp` according. 

##### 2.2 Remove Keypoints Outside of pre-defined rectangle (MP.3)
- Every detected keypoint as the result of 2.1 goes through position check. If x or y coordinate of any point locate outside of pre-defined rectangle, the point is erased from the dynamic list.  

#### 3 Descriptors
##### 3.1 Keypoint Descriptor (MP.4)
- BRIEF, ORB, FREAK, AKAZE and SIFT descriptors are implemented in `desKeypoints` in `matching2D_Student.cpp`. The type of descriptor is selectable by setting the string variable `descriptorType` in `MidTermProject_Camera_Student.cpp` accordingly. 

##### 3.2 Descriptor Matching & Distance Ratio (MP.5 & MP.6)
- Both BF and FLANN matching are implemented in `matchDescriptors` in `matching2D_Student.cpp`. The matchers are from OpenCV library. Either method is selectable by setting string variable `matcherType` in `MidTermProject_Camera_Student.cpp`

- Both Nearest Neighbor and K-Nearest-Neighbor selector types are implemented following the matcher in `matchDescriptors` function. While NN finds the best match, the KNN matching find the best and second best match. The distance ratio test between best vs. second best filter out the bad keypoint match. In this case, the chosen distance ratio is 0.8.

#### 4 Performance Evaluation
##### 4.1 Count number of keypoints of the preceding vehicle in all 10 images (MP.7)

| |Shitomashi|Harris|Fast|Brisk|Orb|Akaze|SIFT|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Images|
| 1 |1370|115|1824|2757|500|1676|1438|
| 2 |1301|98 |1832|2777|500|1638|1371| 
| 3 |1361|113|1810|2741|500|1622|1380|
| 4 |1358|121|1817|2735|500|1704|1335|
| 5 |1333|160|1793|2757|500|1662|1305|
| 6 |1284|383|1796|2695|500|1644|1370|
| 7 |1322|85 |1788|2715|500|1651|1396|
| 8 |1366|210|1695|2638|500|1625|1382|
| 9 |1389|171|1749|2639|500|1663|1463|
|10 |1339|281|1770|2672|500|1638|1422|
|Distribution|License plate,</br>edge of car shadown,</br>tail lights, around car's outer edge, back window.|Tail lights, back window, car outer edge, hood.| Similar to Shitomashi|Outside edge of vehicle, vehicle shadow, tail lights| Tail lights, top right outside edge of vehicle| Shadow line and outside edge of the car.| Edge of shadow, vehicle's back window edge, vehicle hood

##### 4.2 Count the matched keypoints with all possible Detectors & Descriptors (MP.8)
BRISK Descriptor
| Image/Detector | SHITOMASI | HARRIS | FAST | BRISK | ORB  | AKAZE  | SIFT   |
|:----------------:|-----------|--------|------|-------|------|--------|--------|
| 1              | 97        | 12     | 97   | 168   | 73   | 172    | 63     |
| 2              | 88        | 10     | 103  | 169   | 78   | 164    | 64     |
| 3              | 80        | 14     | 104  | 157   | 81   | 169    | 60     |
| 4              | 90        | 15     | 98   | 170   | 85   | 174    | 65     |
| 5              | 82        | 16     | 86   | 171   | 80   | 163    | 59     |
| 6              | 79        | 16     | 108  | 186   | 94   | 173    | 65     |
| 7              | 86        | 15     | 108  | 174   | 87   | 179    | 64     |
| 8              | 86        | 23     | 100  | 168   | 85   | 185    | 67     |
| 9              | 83        | 21     | 100  | 182   | 91   | 186    | 79     |

BRIEF Descriptor
| Image/Detector | SHITOMASI | HARRIS | FAST | BRISK | ORB | AKAZE | SIFT |
|----------------|-----------|--------|------|-------|-----|-------|------|
| 1              | 118       | 14     | 119  | 174   | 49  | 137   | 86   |
| 2              | 111       | 11     | 129  | 195   | 43  | 133   | 76   |
| 3              | 104       | 15     | 122  | 182   | 45  | 130   | 72   |
| 4              | 101       | 20     | 126  | 177   | 59  | 130   | 83   |
| 5              | 104       | 24     | 109  | 182   | 53  | 134   | 69   |
| 6              | 102       | 26     | 124  | 193   | 76  | 146   | 75   |
| 7              | 101       | 16     | 132  | 208   | 67  | 150   | 76   |
| 8              | 110       | 24     | 125  | 186   | 83  | 147   | 69   |
| 9              | 102       | 23     | 120  | 179   | 65  | 150   | 87   |

ORB Descriptor
| Image/Detector | SHITOMASI | HARRIS | FAST | BRISK | ORB  | AKAZE |
|----------------|-----------|--------|------|-------|------|-------|
| 1              | 108       | 12     | 118  | 152   | 70   | 162   |
| 2              | 102       | 12     | 124  | 162   | 71   | 153   |
| 3              | 99        | 15     | 116  | 158   | 74   | 161   |
| 4              | 102       | 18     | 126  | 160   | 84   | 158   |
| 5              | 105       | 24     | 107  | 156   | 89   | 147   |
| 6              | 98        | 20     | 123  | 181   | 100  | 155   |
| 7              | 99        | 15     | 123  | 164   | 89   | 162   |
| 8              | 105       | 24     | 123  | 172   | 90   | 171   |
| 9              | 99        | 22     | 120  | 170   | 94   | 174   |

FREAK Descriptor
| Image/Detector | SHITOMASI | HARRIS | FAST | BRISK | ORB | AKAZE | SIFT |
|----------------|-----------|--------|------|-------|-----|-------|------|
| 1              | 87        | 13     | 98   | 154   | 41  | 123   | 64   |
| 2              | 90        | 13     | 100  | 173   | 36  | 128   | 70   |
| 3              | 86        | 15     | 92   | 153   | 44  | 128   | 62   |
| 4              | 88        | 15     | 98   | 168   | 47  | 121   | 65   |
| 5              | 87        | 17     | 86   | 158   | 44  | 122   | 59   |
| 6              | 80        | 20     | 100  | 181   | 51  | 133   | 60   |
| 7              | 81        | 12     | 104  | 169   | 52  | 144   | 64   |
| 8              | 86        | 21     | 101  | 175   | 47  | 145   | 65   |
| 9              | 86        | 18     | 105  | 165   | 54  | 135   | 79   |

SIFT Descriptor
| Image/Detector | SHITOMASI | HARRIS | FAST | BRISK | ORB | AKAZE | SIFT |
|----------------|-----------|--------|------|-------|-----|-------|------|
| 1              | 115       | 14     | 118  | 177   | 66  | 132   | 81   |
| 2              | 109       | 11     | 122  | 187   | 79  | 134   | 79   |
| 3              | 104       | 16     | 114  | 171   | 78  | 129   | 83   |
| 4              | 103       | 19     | 120  | 177   | 79  | 136   | 92   |
| 5              | 101       | 22     | 115  | 168   | 82  | 136   | 90   |
| 6              | 101       | 22     | 119  | 190   | 93  | 147   | 82   |
| 7              | 97        | 13     | 124  | 193   | 94  | 147   | 82   |
| 8              | 107       | 24     | 118  | 173   | 93  | 153   | 100  |
| 9              | 99        | 22     | 104  | 181   | 92  | 149   | 101  |

##### 4.3 Log the time it takes for keypoint detection and descriptor extraction (MP.9)
Average time of 10 images.
Detector Time (ms)
| Detector/Descriptor | BRISK  | BRIEF  | ORB   | FREAK  | SIFT   |
|---------------------|--------|--------|-------|--------|--------|
| SHITOMASI           | 19.92  | 16.38  | 20.16 | 14.54  | 16.27  |
| HARRIS              | 9.95   | 18.619 | 21.76 | 17.32  | 19.42  |
| FAST                | 2.51   | 0.93   | 2.55  | 0.99   | 1.01   |
| BRISK               | 28.53  | 38.68  | 28.04 | 41.65  | 51.28  |
| ORB                 | 6.69   | 8.02   | 5.72  | 7.94   | 9.44   |
| AKAZE               | 111.46 | 96.92  | 112   | 85.38  | 82.04  |
| SIFT                | 115.13 | 140.06 |       | 134.13 | 107.95 |

Discriptor Time (ms)
| Detector/Descriptor | BRISK | BRIEF | ORB  | FREAK | SIFT  |
|---------------------|-------|-------|------|-------|-------|
| SHITOMASI           | 1.04  | 1.98  | 1.17 | 41.64 | 18.68 |
| HARRIS              | 0.41  | 1.04  | 1.34 | 42.6  | 22.3  |
| FAST                | 1.32  | 1.25  | 1.23 | 43.84 | 28.9  |
| BRISK               | 2.14  | 1.14  | 4.37 | 47.32 | 50.38 |
| ORB                 | 0.96  | 0.74  | 4.28 | 44.77 | 51.26 |
| AKAZE               | 1.69  | 2.09  | 2.75 | 45.35 | 26.59 |
| SIFT                | 1.74  | 1.05  |      | 42.47 | 85.33 |

ORB/SIFT combination does not make this list due to error after compiling.
Other detector/AKAZE does not make this list because AKAZE discriptor only works with AKAZE detector. 

In order to recommend TOP 3 detector/descriptor, some points are taken into consideration:
- Speed for real time application
- Compability and truly opensource
- Decent detection output and matching. 

FAST-ORB detectors and BRISK-BRIEF descriptors are the candidates that meet with criterias. FAST detector consistently performs fastest in all detectors. BRISK and BRIEF does not have much difference in time performance. However, BRIEF does seem to work better with FAST because BRIEF yields more matching pair than BRISK. In case of ORB, ORB yields more matching pair in BRISK than with BRIEF. Therefore, the top 3 recommendation of detector/descriptor are:

1. FAST/BRIEF
2. FAST/BRISK
3. ORB/BRISK

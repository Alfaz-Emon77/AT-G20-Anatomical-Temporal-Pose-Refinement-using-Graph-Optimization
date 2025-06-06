
**AT-G20-Anatomical-Temporal-Pose-Refinement-using-Graph-Optimization**

we introduce AT-G2O (Anatomical-Temporal Pose Refinement using Graph 
Optimization), a novel algorithm designed to refine 2D human pose estimations by integrating 
anatomical and temporal constraints within a graph optimization framework. While existing 2D 
pose estimation methods often suffer from inconsistencies due to occlusions, noise, or missing 
data, AT-G2O addresses these challenges by leveraging the structural relationships inherent in the 
human skeleton and the temporal continuity of motion sequences. 
The core of AT-G2O lies in constructing a graph where each node represents a joint position, and 
edges encode both anatomical connections (e.g., bones) and temporal links across consecutive 
frames. By formulating pose refinement as a graph optimization problem, we employ the g²o 
framework to minimize discrepancies while adhering to anatomical plausibility and motion 
smoothness. 
Our approach operates in two stages: initially fixing high-confidence joints to stabilize the 
optimization, followed by a refinement phase that relaxes these constraints to fine-tune all joint 
positions. This two-tiered strategy ensures robustness against initial estimation errors and 
enhances the overall coherence of the pose sequences. 
Through extensive experiments, we demonstrate that AT-G2O significantly improves the 
accuracy and consistency of 2D pose estimations, making it a valuable tool for applications in 
human-computer interaction, animation, and biomechanics. 
In the AT-G2O framework, each human pose is represented by a fixed set of joints The joints 
indices and their corresponding anatomical names used in the skeleton model are as follows: 
Index 0 corresponds to the Neck, 1 to the Nose, 2 to the Right Shoulder, 3 to the Right Elbow, 4 
to the Right Wrist, 5 to the Left Shoulder, 6 to the Left Elbow, 7 to the Left Wrist, 8 to the Right 
Hip, 9 to the Right Knee, 10 to the Right Ankle, 11 to the Left Hip, 12 to the Left Knee, and 13 
to the Left Ankle. The connections between these joints are based on standard anatomical 
relationships, facilitating accurate pose estimation and analysis. This structure serves as the 
foundation for applying anatomical constraints in our optimization algorithm.


Our algorithm is as shown in Algorithm 1. 

**Algorithm 1: Anatomical-Temporal Pose Refinement using Graph Optimization**
*Input:OpenPose keypoint JSON files for multiple persons and sequences (extracted from 
video)*
*Output:Optimized keypoint JSON files for each sequence (excluding the reference)*
Step 1: For Each Person Folder (p1, p2, p3) 
1.1 Find all sequence folders (e.g., p11, p12, p13) 
1.2 For each sequence folder: 
Load all keypoint JSONs into a sequence array 
Filter out low-confidence keypoints (set to NaN if confidence < 0.3) 
Interpolate missing/low-confidence keypoints using linear interpolation 
1.3 Set the first sequence (pX1) as reference: 
Compute reference bone lengths (median length for each skeleton connection) 


Step 2 : For Each Non-Reference Sequence 
2.1 Initialize a g2o optimizer 
2.2 For each frame: 
Create a vertex for each keypoint (fixed if confidence > 0.5) 
2.3 Add temporal edges: 
For each keypoint, connect consecutive frames 
Use reference sequence movement and confidence to set edge weights 
2.4 Add skeleton (anatomical) constraints: 
For each skeleton connection, add an edge to maintain reference bone length (soft constraint) 
2.5 Run two-stage optimization: 
Stage 1, Initial optimization (10 iterations) 
Stage 2, Refinement (20 iterations) 
2.6 Extract optimized keypoints from the optimizer 
2.7 Apply temporal smoothing (Savitzky-Golay filter) to reduce jitter 
2.8 Save optimized keypoints as new JSON files in an output folder

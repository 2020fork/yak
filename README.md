# yak

yak (yet another kinfu) is a library and ros wrapper for truncated sign distance fields (TSDF).

# yak_meshing

ROS package to mesh TSDF volumes generated by Kinect Fusion-like packages.

This package handles two very different use cases. It can reconstruct from a RGBD camera moved around by a human without any knowledge of pose relative to the global frame. It can also reconstruct from a sensor mounted on a robot arm using pose hints provided by TF and robot kinematics. The idea is that this second case doesn't need to deduce sensor motion by comparing the most recent reading to previous readings via ICP, so it should work better in situations with incomplete sensor readings.

The manual situation should work out of the box without any other packages. You might need to force the sensor to reset to get the volume positioned in a desirable orientation around your target. The easiest way to do this is to cover and uncover the camera lens.

The robot-assisted situation is basically hardcoded to use the sensors and workcell models from the Godel blending project. This will change soon to make it more generalized!

Meshing happens through the /get_mesh service, which in turn calls the kinfu_ros /get_tsdf service. yak_meshing_node expects a serialized TSDF voxel volume, which is a list of TSDF values and weights for every occupied voxel along with a list of the coordinates of each occupied voxel. OpenVDB's voxel meshing algorithm generates a triangular mesh along the zero-value isosurface of the TSDF volume. The mesh is saved as a .obj, which can be viewed and manipulated in a program like Meshlab or Blender.

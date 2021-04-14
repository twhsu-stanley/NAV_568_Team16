# Examination of Long Term Environment Effects on Robot Localization Using RI-EKF and the NCLT Dataset
ROB 530: Mobile Robotics Winter 2021
Final Project Team 16: Adarsh Karnati, Lars Grantz, Ting-Wei Hsu

# Introduction
This project aims at evaluating robustness of the right-invariant extended Kalman filter(RI-EKF) against long term environmental changes. RI-EKF localization is implemented on the NCLT dataset and its performance is analyzed across different sessions. This repository includes Python code that allows to:
1. extract landmarks from 3-D lidar scans
2. create a global reference map of landmarks
3. implement RI-EKF localization on this map
4. evaluate performance of RI-EKF localization

# NCLT Dataset
The NCLT Dataset is a large-scale, long-term dataset collected on the North Campus of the University of Michigan. The dataset consists of vision data, lidar data, GPS, and IMU data collected using a Segway robot. The reason for choosing this dataset is that it includes a wide range of long-term environmental changes, such as seasonal changes, lighting changes, and structural changes. The data can be downloaded [here](http://robots.engin.umich.edu/nclt/index.html). Use the provided downloader.py to download sensor data, Velodyne(3D lidar)data, and the ground truth.

# Landmarks Extraction and Mapping
Landmarks extraction and mapping are implemented using the algoritm proposed in [this paper](http://ais.informatik.uni-freiburg.de/publications/papers/schaefer19ecmr.pdf) (Schaefer et al. 2019). Use Python code and modules in [this repositpry](https://github.com/acschaefer/polex) to generate the global reference map. The resulting global map is shown in Fig. 1.

Figure 1: global reference map
<img src="https://user-images.githubusercontent.com/78635240/114787787-555b7880-9d4e-11eb-98a7-662765b00502.png" width="500" height="400">

# RI-EKF Localization
The RI-EKF localization is implemented using inEKF.py. In inEKF.py:
1. robot pose in SE(2) is considered
2. for the process model, the odometry data are used to propagate the states
3. for the measurement model, a ball tree search algorithm is employed for data association between online lidar scans and the landmarks in the global reference map. Mahalanobis distance is used as the distance metric and the landmarks are accepted/rejected based on Chi-squre tests.

# Running the Code
Once complete downloading the data from NCLT dataset and building the dependencies, run ncltpoles.py for the overall maping and localization process. In ncltpoles.py:
1. save_global_map() builds the global reference map of landmarks
2. save_local_maps(session_name) builds the local map (i.e. online lidar scans) of detected landmarks
3. localize(session_name, visualize = False) implements RI-EKF localization on the global reference map; it also plots the NEES (normalized estimation error squared) graph to evaluate the consistency of the RI-EKF, as shown in Fig. 2.
4. plot_trajectories() plots the trajectories of both the estimated and ground truth poses, as shown in Fig. 3.
5. evaluate() computes the mean error and root-mean-square error of between the estimated and ground truth poses


Figure 2: the NEES plot
<img src="https://user-images.githubusercontent.com/78635240/114788979-673e1b00-9d50-11eb-8f55-856c35e4fe3f.png" width="500" height="400">

Figure 3: estimated and ground truth trajectories

<img src="https://user-images.githubusercontent.com/78635240/114788207-0a8e3080-9d4f-11eb-96dc-bb7587c1f5e4.png" width="500" height="400">

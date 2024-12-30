# Grace Eye and Neck Motor Calibration

## Dataset

This file contains the chest camera to left & right eye camera coordinate transform given a set of neck and eyes motor commands from the Grace Robot. Additional data such as the motor angle position from motor encoders and eye camera ray intersections with the planar calibration board in chest camera coordinates are also included.

The setup involves the Grace Robot standing 0.75 meters away from a 8x15 charuco calibration board displayed on a 98-inch TV. The image has a resolution of 3840 x 2160 with a square length of 13.6 cm, and marker length of 9.1 cm. For the gaze pixel of the left and right camera, it is the location of the nose bridge of the user on the eye camera image wherein he/she perceives that the robot is gazing directly at him/her.

![Alt text](charuco_marker.png)

The motor commands are uniformly sampled from a combination of:  
* lower neck tilt (deg): {-10,0,10,20,30}
* lower neck pan (deg): {-35,-30,-25,-20,-15,-10,-5,0,5,10,15,20,25,30,35}
* upper neck tilt (deg): {-10,0,10,20,30,40}
* eyes tilt (deg): {-30,-25,-20,-15,-10,-5,0,5,10,15,20}
* left eye pan (deg): {-14,-12,-10,-8,-6,-4,-2,0,2,4,6,8,10,12,14}
* right eye pan (deg): {-14,-12,-10,-8,-6,-4,-2,0,2,4,6,8,10,12,14}

The total dataset contains 1,057,910 samples wherein motor states that yield error status have already been removed.

The dataset is located at `data/intern/0pt75m_grace_dataset.zip`. Extract the zip file to obtain the csv dataset. The columns are:
* x_c_l (meter): the x-axis value of the left eye camera gaze point intersection with charuco board in chest camera coordinates.
* y_c_l (meter): the y-axis value of the left eye camera gaze point intersection with charuco board in chest camera coordinates.
* z_c_l (meter): the z-axis value of the left eye camera gaze point intersection with charuco board in chest camera coordinates.
* x_c_r (meter): the x-axis value of the right eye camera gaze point intersection with charuco board in chest camera coordinates.
* y_c_r (meter): the y-axis value of the right eye camera gaze point intersection with charuco board in chest camera coordinates.
* z_c_r (meter): the z-axis value of the right eye camera gaze point intersection with charuco board in chest camera coordinates.
* cmd_theta_lower_neck_pan (deg): motor command for the neck rotation motor
* cmd_theta_lower_neck_tilt (deg): motor command for the lower gimbal left and lower gimbal right motor
* cmd_theta_upper_neck_tilt (deg): motor command for the upper gimbal left and upper gimbal right motor
* cmd_theta_left_eye_pan (deg): motor command for the eye turn left motor
* cmd_theta_right_eye_pan (deg): motor command for the eye turn right motor
* cmd_theta_eyes_tilt (deg): motor command for the eyes up down motor


* state_theta_lower_neck_pan (deg): motor encoder position for the neck rotation motor
* state_theta_left_lower_neck_tilt (deg): motor encoder position for the lower gimbal left motor
* state_theta_right_lower_neck_tilt (deg): motor encoder position for the lower gimbal right motor
* state_left_theta_upper_neck_tilt (deg): motor encoder position for the upper gimbal left motor
* state_right_theta_upper_neck_tilt (deg): motor encoder position for the upper gimbal right motor
* state_theta_left_eye_pan (deg): motor encoder position for the eye turn left motor
* state_theta_right_eye_pan (deg): motor encoder position for the eye turn right motor
* state_theta_eyes_tilt (deg): motor encoder position for the eyes up down motor

* l_rvec_0 (rad): first component of the rotation vector or x-axis rotation in the T_lc0 homogeneous transformation matrix. T_lc0 is the coordinate transform from left eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.
* l_rvec_1 (rad): second component of the rotation vector or y-axis rotation in the T_lc0 homogeneous transformation matrix. T_lc0 is the coordinate transform from left eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.
* l_rvec_2 (rad): third component of the rotation vector or z-axis rotation in the T_lc0 homogeneous transformation matrix. T_lc0 is the coordinate transform from left eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.
* l_tvec_0 (rad): first component of the transation vector or the x value in the T_lc0 homogeneous transformation matrix. T_lc0 is the coordinate transform from left eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.
* l_tvec_1 (rad): second component of the translation vector or y value in the T_lc0 homogeneous transformation matrix. T_lc0 is the coordinate transform from left eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.
* l_tvec_2 (rad): third component of the translation vector or z value in the T_lc0 homogeneous transformation matrix. T_lc0 is the coordinate transform from left eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.

* r_rvec_0 (rad): first component of the rotation vector or x-axis rotation in the T_rc0 homogeneous transformation matrix. T_rc0 is the coordinate transform from right eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.
* r_rvec_1 (rad): second component of the rotation vector or y-axis rotation in the T_rc0 homogeneous transformation matrix. T_rc0 is the coordinate transform from right eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.
* r_rvec_2 (rad): third component of the rotation vector or z-axis rotation in the T_rc0 homogeneous transformation matrix. T_rc0 is the coordinate transform from right eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.
* r_tvec_0 (rad): first component of the transation vector or the x value in the T_rc0 homogeneous transformation matrix. T_rc0 is the coordinate transform from right eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.
* r_tvec_1 (rad): second component of the translation vector or y value in the T_rc0 homogeneous transformation matrix. T_rc0 is the coordinate transform from right eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.
* r_tvec_2 (rad): third component of the translation vector or z value in the T_rc0 homogeneous transformation matrix. T_rc0 is the coordinate transform from right eye camera to chest camera coordinate system assuming 0-degree chest camera motor command.

Pose orientation follows the OpenCV convention; x-axis: positive rightwards, y_axis: positive downwards, z-axis: positive towards the board. Conversion of rotation vector to rotation matrix and vice versa can be done using the Rodrigues function in OpenCV.


## ANN Model

The model predicts the coordinate transformation from the left and right camera to the chest camera of the Grace Robot given a set of motor commands as input.

The dataset is normalized using min-max normalization with the ranges [-44,44] deg for the motor commands inpiut, [-1.5,1.5] meters for the x,y,z translation vector output, and [-1.5708,1.5708] rad for the rotation vector output.

The ANN model checkpoint is located at `model/grace_ann/checkpoints/epoch_498.ckpt` with its training configuration and parameters found at `model/grace_ann/config_tree.log`.

To retrain the model, run the following command `source scripts/train_template.sh` and view the training curves on tensorboard.





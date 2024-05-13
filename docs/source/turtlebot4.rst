Turtlebot4
=====

.. _Environment:

Environment
------------

This project was developed with the following environment and platform:
 * Ubuntu              22.04
 * ROS2                humble
 * ignition gazebo     6.16.0
 * ignition service    11.4.1
 * python              3.10.12
 * c++                 11.4.0
 * Eigen3              3.4.0-2ubuntu2


.. _Hardware:

Hardware
------------

Turtlebot4 from Wyman


.. _installation:

Installation
------------

To successfully detect Aruco AR markers through Turtlebot4 camera, first install relevant libraries by:

.. code-block:: bash

   $ pip3 install opencv-contrib-python transforms3d
   # Then clone ros2-aruco library from https://github.com/JMU-ROBOTICS-VIVA/ros2_aruco.git
   # Go to the corresponding directory, then execute
   $ colcon build --packages-select ros2_aruco
   # Don't forget to run source install/setup.bash when you want to use ros2_aruco package. 


| More information on https://github.com/JMU-ROBOTICS-VIVA/ros2_aruco.git.
As for the turtlebot4, we follow the turtlebot4 User Manual on https://turtlebot.github.io/turtlebot4-user-manual. We follow every steps to install the turtlebot4 gazebo simulation and turtlebot4 packages.

| Once installing ros2 aruco packages and turtlebot4 packages, our project's packages about turtlebot4 could be installed through colcon build. And it would work.


.. _Package Specification:

Package Specification
------------

The packages are listed as followed.
* t4_wyman

* move_turtlebot

* ros2_aruco

* spin_around


| Here is a description of each package.


.. _t4_wyman:

t4_wyman
------------

This is a package similar to RSP homework 10. The main idea is that we load wyman building in gazebo first and we put a cube with a Aruco AR marker in this world. Also, we have a slam launch file to generate a map as well as a patrol launch file to do localization and navigation. Here is the description of each file/folder in this package.
* config: it includes behavior tree file and navigation file.
* launch: it includes slam and patrol launch files.
* maps: it includes generated wyman map. 
* worlds: it includes wyman building model and cube with a Aruco AR marker model.
* CmakeLists.txt
* package.xml


.. _ros2_aruco:

ros2_aruco
------------

This is the package from https://github.com/JMU-ROBOTICS-VIVA/ros2_aruco.git for Aruco AR marker detecting.


.. _move_turtlebot:

move_turtlebot
------------

The main idea for the Aruco AR marker detecting part of our project is as we run the turtlebot4, no matter in the gazebo simulation environment or in the real world, if the camera successfully detect the marker, our turtlebot4 will stop previous moving command and then automatically move toward the marker. We define a safe distance so the turtlebot4 will keep moving toward the marker until it's within the safe distance. 
In this package, it mainly has "include" and "src" folder like the standard ros2 c++ package.

Here is the description for functions achieved in this package.
*Initialization: We initialize several 
*quat2matrix:
*ar_cb:
*move_cb:

.. _spin_around:

spin_around
------------

asdasd.






| Then change the specific directory of ``mx_controllers.yaml`` on your computer for the two files in package ``manipulatorx_ign``

* manipulator_camera.urdf.xacro

.. code-block:: bash
   
   Line22  <parameters>[your full directory]/mx_controllers.yaml</parameters>


* manipulator_camera.urdf

.. code-block:: bash
   
   Line350  <parameters>[your full directory]/mx_controllers.yaml</parameters>


| And one file in package ``manipulatorx_moveit``


* open_manipulator_x.urdf.xacro

.. code-block:: bash
   
   Line18  <parameters>[your full directory]/mx_controllers.yaml</parameters>


.. _Open_Manipulator_X status:

Open_Manipulator_X status
----------------

.. code-block:: bash
   
   $ source install/setup.bash
   # start the controller for the arm
   $ ros2 launch open_manipulator_x_controller open_manipulator_x_controller.launch.py 
   $ ros2 run arm_subscriber test_armsubcriber 
   # view the kinematic pose, joint positions and arm status


.. _Open_Manipulator_X service:

Open_Manipulator_X service
----------------

.. code-block:: bash
   
   $ source install/setup.bash
   # start the controller for the arm
   $ ros2 launch open_manipulator_x_controller open_manipulator_x_controller.launch.py 
   # use the tele-op to control the joint positions
   $ ros2 run arm_service test_movejoint
   # <------------or---------------> 
   # use the tele-op to control the gripper open or close
   $ ros2 run arm_service test_movetool 
   # <------------or---------------> 
   # use the tele-op to control the move in x y or z in cartesian space
   $ ros2 run arm_service test_movecart
   # <------------or---------------> 
   # run a pick-n-place program in fixed positions
   $ ros2 run arm_service test_pnp


.. _Open_Manipulator_X moveit:

Open_Manipulator_X moveit
----------------

.. code-block:: bash

   $ source install/setup.bash
   # launch the moveit package
   $ ros2 launch manipulatorx_moveit manipulator_moveit.launch.py

| The moveit package demostrate the trajectory planning with several famous algorithm like PRM, RRT etc
|
| you can test your own planning algorithm by switch to custom planning pipeline ``manipulator_moveit.launch.py``: 
|


.. code-block:: bash

   $ >>> Planning Configuration
   # planning_pipeline_config = {
   #     "move_group": {
   #         "planning_plugin": "ompl_interface/OMPLPlanner",
   #         "request_adapters": """default_planner_request_adapters/AddTimeOptimalParameterization default_planner_request_adapters/FixWorkspaceBounds default_planner_request_adapters/FixStartStateBounds default_planner_request_adapters/FixStartStateCollision default_planner_request_adapters/FixStartStatePathConstraints""",
   #         "start_state_max_bounds_error": 0.1,
   #     }
   # }
   # ompl_planning_yaml = load_yaml("manipulatorx_moveit", "config/ompl_planning.yaml")
   # planning_pipeline_config["move_group"].update(ompl_planning_yaml)
   
   $ >>> custom planning configuration
   planning_pipeline_config = {
     "move_group": {
         "planning_plugin": "manipulatorx_moveit/ASBRPlanner",
         "start_state_max_bounds_error": 0.1,
      }
   }
   asbr_planning_yaml = load_yaml("manipulatorx_moveit", "config/custom_planning.yaml")
   planning_pipeline_config["move_group"].update(asbr_planning_yaml)


|
.. image:: images/custom_moveit_planner.png
   :height: 200px
   :width: 400px
   :alt: custom planning in moveit


.. _Open_Manipulator_X gazebo:

Open_Manipulator_X gazebo
----------------

.. code-block:: bash

   $ source install/setup.bash
   # launch the moveit package
   $ ros2 launch manipulatorx_ign manipulatorx_ign.launch.py
   # you can run the node to see the arm move and view changes
   $ ros2 run manipulatorx_ign test_manipulatorx_ign_node 

.. image:: images/gazebo_gif.gif
   :height: 450px
   :width: 800px
   :alt: gazebo_gif



.. _Open_Manipulator_X ArUco:

Open_Manipulator_X ArUco
----------------

.. code-block:: bash

   $ source install/setup.bash
   # launch the pick aruco package
   $ ros2 launch manipulatorx_handeye manipulatorx_handeye.launch.py
   # you can run the node to see the arm move and pick up the aruco object in workspace automatically
   $ ros2 run manipulatorx_handeye search_aruco


.. image:: images/grasp_aruco_gif.gif
   :height: 450px
   :width: 800px
   :alt: grasp_aruco_gif


| The handeye calibration matrix is written into urdf, can be retrieved from:

.. code-block:: bash

   $ ros2 run tf2_ros tf2_echo camera_color_optiocal_frame end_effector_link


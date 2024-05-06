Open_Manipulator_X
=====

.. _Environment:

Environment
------------

This project was developed with the following environment and platform:
- Ubuntu              22.04
- ROS2                humble
- ignition
- openmanipulatorx    `emanual <https://emanual.robotis.com/docs/en/platform/openmanipulator_x/overview/>`_
- Intel Realsense     D435


.. _installation:

Installation
------------

To use open manipulator x, first install relevant libraries by:

.. code-block:: console

   $ sudo apt install ros-humble-moveit*
   $ sudo apt-get install ros-humble-control*
   $ sudo apt-get install ros-humble-ros2-control*
   $ sudo apt-get install ros-humble-ros-ign
   $ sudo apt-get install ros-humble-ign-ros2-control
   $ sudo apt-get install ros-humble-kinematics-interface-kdl
   $ sudo apt-get install ros-humble-rqt-joint-trajectory-controller
   $ sudo apt-get install python3-colcon*
   $ sudo apt-get install ros-humble-ur-moveit-config

Then goto the directory containing src folder and run rosdep 

.. code-block:: console
   
   $ rosdep install --from-paths src --ignore-src -r -y


.. _Open_Manipulator_X status:

Open_Manipulator_X status
----------------

To retrieve a list of random ingredients,
you can use the ``lumache.get_random_ingredients()`` function:

.. autofunction:: lumache.get_random_ingredients

The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
will raise an exception.

.. autoexception:: lumache.InvalidKindError

For example:

>>> import lumache
>>> lumache.get_random_ingredients()
['shells', 'gorgonzola', 'parsley']


.. _Open_Manipulator_X service:

Open_Manipulator_X service
----------------

.. _Open_Manipulator_X moveit:

Open_Manipulator_X moveit
----------------

.. _Open_Manipulator_X gazebo:

Open_Manipulator_X gazebo
----------------

.. _Open_Manipulator_X ArUco:

Open_Manipulator_X ArUco
----------------
```bash
# 🛠️ Project Goal:
Set up a 5-joint robot arm in ROS 2 (Rolling) using URDF/XACRO, simulate the robot in RViz, and manually control joints via `joint_state_publisher_gui`. Due to VirtualBox limitations, the visual mesh model may not render, but TF frames are correctly published.

# ✅ Environment Setup:
sudo apt update && sudo apt install ros-rolling-desktop python3-colcon-common-extensions -y
source /opt/ros/rolling/setup.bash

# 📁 Workspace Setup:
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
git clone https://github.com/smart-methods/Robot_Arm_ROS2.git
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build
source install/local_setup.bash

# 🚀 Manual Launch (Split Terminals):

# Terminal 1 – Start robot_state_publisher:
source /opt/ros/rolling/setup.bash
source ~/ros2_ws/install/local_setup.bash
ros2 run robot_state_publisher robot_state_publisher \
  --ros-args -p robot_description:="$(xacro ~/ros2_ws/src/Robot_Arm_ROS2/arduinobot_description/urdf/arduinobot.urdf.xacro)"

# Terminal 2 – Start joint_state_publisher_gui:
ros2 run joint_state_publisher_gui joint_state_publisher_gui

# Terminal 3 – Start RViz (with software rendering for VirtualBox):
export LIBGL_ALWAYS_SOFTWARE=1
rviz2

# 🎯 Inside RViz:
- Set **Fixed Frame** to `base_link`
- Add:
  - **TF** → to view joint transforms
  - **RobotModel** → (may not show meshes due to VirtualBox, that's OK)
  - **Grid**

# ℹ️ Notes:
- STL meshes exist and are referenced correctly from URDF.
- TF frames confirm the URDF is parsed and published.
- If RViz doesn't display the robot model, it's due to graphics driver issues in VirtualBox. In a native Ubuntu setup, it should render properly.
```

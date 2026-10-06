## Robotiq 2F 85 gripper
Cho tay gắp của mô hình này, repo git dưới đây có thể được dùng như nguồn tham khảo: https://github.com/Shreeyak/robotiq.git

### mimic tag trong URDF
Tay gắp này được xây dựng trong ROS và dùng `mimic` tag trong file URDF để khiến tay gắp di chuyển, tuy nhiên chương trình trên không thể chạy được trên pybullet. Để giải quyết vấn đề trên, có thể dùng hàm `createConstraint` tham khảo trong [đây](https://github.com/bulletphysics/bullet3/blob/master/examples/pybullet/examples/mimicJointConstraint.py) một ví dụ để xây dựng mô hình cho tay gắp chuyển động trong bullet3 với `mimic` joint:

```python
#a mimic joint can act as a gear between two joints
#you can control the gear ratio in magnitude and sign (>0 reverses direction)

import pybullet as p
import time
p.connect(p.GUI)
p.loadURDF("plane.urdf",0,0,-2)
wheelA = p.loadURDF("differential/diff_ring.urdf",[0,0,0])
for i in range(p.getNumJoints(wheelA)):
	print(p.getJointInfo(wheelA,i))
	p.setJointMotorControl2(wheelA,i,p.VELOCITY_CONTROL,targetVelocity=0,force=0)


c = p.createConstraint(wheelA,1,wheelA,3,jointType=p.JOINT_GEAR,jointAxis =[0,1,0],parentFramePosition=[0,0,0],childFramePosition=[0,0,0])
p.changeConstraint(c,gearRatio=1, maxForce=10000)

c = p.createConstraint(wheelA,2,wheelA,4,jointType=p.JOINT_GEAR,jointAxis =[0,1,0],parentFramePosition=[0,0,0],childFramePosition=[0,0,0])
p.changeConstraint(c,gearRatio=-1, maxForce=10000)

c = p.createConstraint(wheelA,1,wheelA,4,jointType=p.JOINT_GEAR,jointAxis =[0,1,0],parentFramePosition=[0,0,0],childFramePosition=[0,0,0])
p.changeConstraint(c,gearRatio=-1, maxForce=10000)


p.setRealTimeSimulation(1)
while(1):
	p.setGravity(0,0,-10)
	time.sleep(0.01)
#p.removeConstraint(c)

```


Thông tin chi tiết về hàm `createConstraint` có thể được tìm thấy tại hướng dẫn của pybullet [getting started](https://docs.google.com/document/d/10sXEhzFRSnvFcl3XxNGhnD4N2Sed qwdAvK3dsihxVUA/edit#heading=h.fq749wu22x4c) guide.

### Trong thư mục này có chứa
Tham khảo các thông số về tỉ lệ khớp và hướng, vị trí đã được dựng sẵn trong `robotiq_2f_85_mimic_joints.urdf` chứa các minic tag như file URDF ban đầu. Các thông số trên được tạo từ `robotiq/robotiq_2f_robot/robot/simple_rq2f85_pybullet.urdf.xacro` bằng câu lệnh:
```
rosrun xacro xacro --inorder simple_rq2f85_pybullet.urdf.xacro
adaptive_transmission:="true" > robotiq_2f_85_mimic_joints.urdf
```

File URDF được xây dựng để dùng trong pybullet là `robotiq_2f_85.urdf` và được dựng bởi cùng một phương pháp như trên khi chạy:
```
rosrun xacro xacro --inorder simple_rq2f85_pybullet.urdf.xacro > robotiq_2f_85.urdf
```
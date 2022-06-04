# GAMES104 - Anim System
### 主要挑战
- 性能
- 真实
## Basics of Animation Technology
### 2D Animation
- Sprite Anim 精灵动画：播放一串连续的图片
- Live2D：变换图片上的一系列控制点来做表情和动作的变化；Key Frame
### 3D Animation
- Vertex Anim:逐顶点动画(存的是每帧顶点数据)
    - 很灵活
    - 大部分使用vertex animation texture(VAT)实现，通常需要两张，Translation Texture & Rotation Texture
    - **适用于复杂变形动画**
    - 需要大量的数据，数据爆炸
- Morph Target animation:目标变形动画(一种顶点动画的变体)
    - 在关键帧动画序列之间插值得到平滑的动画(Key Pose -> Play with Lerp)
    - **适用于面部动画**
- 3D Skinned Animation(骨骼蒙皮动画， 存多帧关节数据，和顶点相对于关节的位置数据)
    - Mesh/Skin被绑定到骨骼的关节(多个)
    - 每个顶点受不同关节影响有不同权重(多个关节的Trans加权后得到顶点的Trans)
    - **比逐顶点动画数据量小，Mesh的动画可以更自然**(就像人的皮肤一样)
    - 现代游戏引擎中最常见的技术
- 2D Skinned animation
    - 衍生自3D蒙皮动画类似的2D版本
- Physics-based Animation
    - Ragdoll
    - 物理模拟：布料和流体模拟
    - IK反向动力学
- ACC(Animation Content Creation)
    - Key Frame:手K动画
    - Motion Capture:动捕
### Skinned Animation Impl
- How to Animate a Mesh?
    - Mesh: 给绑定Pose创建一个Mesh网格
    - Skeleton: 给网格创建一个绑定的骨架
    - Skinning/Rig: 将Mesh上每个顶点关联/绑定到相关的骨骼上
    - Pose: 调整骨骼体到想要的Pose
    - Animation: 根据骨骼的Trans变化以及权重更新网格体顶点的位置
- Different Spaces
    - WS: World Space
    - MS: Model Space(模型坐标系，类似Root)
    - LS: Local Space(骨骼的局部坐标系)
- Joint vs. Bone
    - Joint: 关节(真正用来控制Pose的对象)
    - Bone: 骨骼(两个关节之间的才是骨头，没什么实际作用)
- Humaniod Skeleton in Real Game
    - Normal : 50-100 joins(标准骨骼)
    - Others : facial joins, weapon join, mount joint or gameplay joints(披风 面部 以及 Gameplay需要的Joints, 比如武器和骑乘的关节点)
- Root Joint(Skeleton's Start)
    - Root joint: 脚之间，贴地
    - Pelvis joint: 骨盆的位置
- Bind Pose - T-pose vs. A-pose
    - 行业实践，A-pose肩膀处更自然，更适用
    - Skeleton Pose: 从绑定姿势变换关节/骨骼点位置来的Pose
- Math of 3D Rotation 
    - Euler Angle的问题
        - 难以插值
        - 旋转叠加需要旋转矩阵，难以叠加
        - 绕XYZ轴旋转简单，绕其他的轴旋转很难
    - Quaternion
        - 游戏数学的基础
- Joint Pose
    - Rotation Translate Scale
    - 动画数据存在local space:如果存在model/world插值起来不方便
    - Skinning Matrix(**不太懂**):思路是mesh顶点相对于绑定骨骼的Transform不变
- Clip: 一个Pose序列
- Interpolation
    - LERP
    - NLERP:使用Quaternion插值
    - SLERP(**不太懂**)
- Simple Anim Runtime Pipeline
    - 输入Clip和time, 从上一帧Pose到目标帧Pose之间插值(LS)得到当前帧的骨架Pose,换算到MS，算出蒙皮矩阵(WS), 最终计算出顶点的位置

### Animation Compression(动画压缩)
- Scale：忽略掉Scale Track
- Keyframe(误差比较大，如果要好的效果，需要加很多关键帧):只存关键帧，然后在之间插值。去关键帧的思路：遍历每帧动画，然后Lerp(Frame[i-1], Frame[i+1])之间插值，如果误差太大，那么就把中间帧当作关键帧存起来
- Catmull-Rom Spline: 效果很好
- float Quantization: 数据压缩(**不太懂**)
### Animation DCC(Pileline)
- Mesh building: LowpolyMesh
- Skeleton Binding
- Skinnning - Auto Calculation, hand adjust
- Animation Creation : artist make keyframe and program handle

## Q&A
- Mesh顶点绑定的关节有数量限制嘛？
    - 理论上可以绑定无数个，但是绑太多会大大增加计算量，一般不超过4个

## Advanced Animation Technology
### Animation Blend
### Inverse Kinematics
### Animation Pipeline
### Animation Graph
### Facial Animation 
### Retargeting 


# 1 环境安装
本文档使用的环境如下：

- ML-Agents Release 23 [4.0.0] - 2025-08-28

| 软件          | 版本号         |
| ----------- | ----------- |
| python      | 3.10.12     |
| python软件包   | 1.1.0       |
| UnityHub    | 3.15.2      |
| UnityEditor | 6000.0.62f1 |
| Unity软件包    | 4.0.0       |

## 1.1 参考资料
[Installation | ML Agents | 4.0.0](https://docs.unity3d.com/Packages/com.unity.ml-agents@4.0/manual/Installation.html)

[GitHub - Unity-Technologies/ml-agents: The Unity Machine Learning Agents Toolkit (ML-Agents) is an open-source project that enables games and simulations to serve as environments for training intelligent agents using deep reinforcement learning and imitation learning.](https://github.com/Unity-Technologies/ml-agents)

## 1.2 操作步骤
### 1.2.1 安装unity
#### 1.2.1.1 下载最新unity hub
自行选择科学上网方式，节点建议选择美国，**注意香港节点也会跳转回cn网站**。
![[Pasted image 20251118125424.png]]

进入国际版UnityHub官网，下载UnityHub
[Start Your Creative Projects and Download the Unity Hub | Unity](https://unity.com/download)
![[Pasted image 20251118125326.png]]

下载安装包，安装后打开，自行创建账号并登录
![[Pasted image 20251118125850.png]]

切换至中文
![[Pasted image 20251118125632.png]]

#### 1.2.1.2 添加个人版许可证
![[Pasted image 20251118125716.png]]

如果获取失败，修改当前用户对unity文件夹的写入权限，重新获取
![[Pasted image 20251118104551.png]]

#### 1.2.1.3 安装unity编辑器
科学上网开启TUN模式，**否则编辑器的下载请求会重定向回cn导致下载失败。**
![[Pasted image 20251118130825.png]]

安装unity编辑器
![[Pasted image 20251118130029.png]]

勾选中文语言包
![[Pasted image 20251118130046.png]]

### 1.2.2 安装Python 3.10.12及所需库
Conda安装好后，打开Prompt，搭建Python 3.10.12虚拟环境。
```
conda create -n IFR_mlagents python=3.10.12 --platform win-64
```

激活环境：
```
conda activate IFR_mlagents
```

优先安装torch，指定版本2.2.1，**否则安装mlagents时会下载最新版，导致模型导出时出现报错**
```
CPU训练：
    pip install torch==2.2.1
    
    
N卡训练：
	pip install torch==2.2.1 torchvision==0.17.1 torchaudio==2.2.1 --index-url https://download.pytorch.org/whl/cu121
	
	**安装后验证**
		python -c "import torch; print('版本:', torch.__version__); print('CUDA:', torch.cuda.is_available()); print('GPU:', torch.cuda.get_device_name(0))"
	
	**预期输出**：
		版本: 2.2.1+cu121
		CUDA: True
		GPU: NVIDIA GeForce GTX 1080 Ti
		如果显示 `CUDA: True`，就可以开始GPU训练了！
	
```

安装mlagents：
```
python -m pip install mlagents==1.1.0
```

**pytorch安装**
pip uninstall torch torchvision torchaudio -y
pip install torch==2.2.1 torchvision==0.17.1 torchaudio==2.2.1 --index-url https://download.pytorch.org/whl/cu121


## 1.3 运行demo验证环境
### 1.3.1 下载demo工程文件
[Release ML-Agents Release 23 · Unity-Technologies/ml-agents · GitHub](https://github.com/Unity-Technologies/ml-agents/releases/tag/release_23_tag)

### 1.3.2 将例程放入工程目录下
![[Pasted image 20251118132821.png]]

### 1.3.3 打开例程工程
![[Pasted image 20251118132920.png]]
![[Pasted image 20251118133012.png]]
![[Pasted image 20251118133125.png]]
![[Pasted image 20251118131732.png]]
![[Pasted image 20251118133928.png]]

### 1.3.4 开始训练
打开Prompt，激活环境，切换到工程目录下
![[Pasted image 20251118133652.png]]

输入指令 启动训练
```
mlagents-learn config/ppo/3DBall.yaml --run-id=first3DBallRun
```

观察到python侧准备完成，返回unity编辑器，点击开始
![[Pasted image 20251118133812.png]]
![[Pasted image 20251118133828.png]]

可以观察到已开始训练。
![[Pasted image 20251118133840.png]]

等待训练完成，可以看到类似的onnx模型成功导出，环境验证成功。
![[Pasted image 20251119141722.png]]

## 1.4 Unity编辑器ML库安装
在1.3中，因为是导入的工程，本身已安装ML库，这里演示空白工程添加ML库的方式。
![[Pasted image 20251118134505.png]]

新建项目
![[Pasted image 20251118144819.png]]
打开包管理器
![[Pasted image 20251118140014.png]]

在Unity库列表中搜索ML，选择ML Agents安装
![[Pasted image 20251118140116.png]]

等待完成，安装成功
![[Pasted image 20251118140235.png]]


## 1.5 Unity后台运行：在失去焦点时继续执行##
![[Pasted image 20251120100556.png]]
![[Pasted image 20251120100646.png]]

# 2 自定义训练
## 2.1 Unity工程文件夹架构
![[Pasted image 20251120103713.png]]

### 2.1.1 **Assets** 文件夹：
这是 Unity 项目的主要文件夹，存放项目中所有的资源，如脚本、场景、材质、模型、纹理等。            
#### 2.1.1.1 **ML-Agents**:  
存放与 **ML-Agents** 相关的资源和配置。
##### **Config**:^ 
- 存放训练配置文件（如 `.yaml`）和其他训练相关的配置文件。
##### **Results**:^ 
- 存放训练过程中的结果文件，如保存的模型（`.onnx` 文件）、日志文件等。

	- **Timers**: 可能用于存储训练过程的时间数据，帮助分析训练效率和瓶颈。
#### 2.1.1.2 **Prefabs**:  
存放 **Prefab** 资源。Prefab 是 Unity 中的一个非常重要的概念，它允许你保存和复用具有相同组件配置的游戏对象。这里通常存放你希望在多个场景中重复使用的对象，比如角色、道具等。
#### 2.1.1.3 **Scenes**:  
存放 Unity 的场景文件（以 `.unity` 为扩展名）。每个场景代表一个独立的游戏环境，包含了场景中的所有对象、灯光、摄像机等。
#### 2.1.1.4 **Scripts**:  
存放所有的 C# 脚本。通常会有多个子文件夹来组织不同的脚本，例如 **Agents**（用于存放智能体相关的脚本），**GameLogic**（游戏逻辑相关的脚本）等。
#### 2.1.1.5 **Materials**:  
存放 Unity 中使用的材质文件。材质是给游戏对象赋予外观和光照特性的资源，例如纹理、颜色、反射等。
#### 2.1.1.6 **Settings**:  
存放 Unity 项目的设置文件，如输入设置、图形设置、标签和图层设置等。这些文件控制着项目的全局设置。
#### 2.1.1.7 **TutorialInfo**:  
可能是存放与教程相关的文件夹，或者包含学习和实验的一些示例或说明文件。如果你的项目包含一些学习模块或演示文件，可以存放在这里。

### 2.1.2 **Library** 文件夹：
- **用途**：存放 Unity 编辑器生成的临时数据和缓存文件。例如，编译后的脚本、导入的资产、着色器缓存等。这个文件夹一般不需要手动管理，Unity 会自动处理。
### 2.1.3 **Logs** 文件夹：
- **用途**：存放 Unity 编辑器或运行时生成的日志文件。对于调试和错误跟踪非常有用，特别是在训练过程中记录了很多调试信息和训练日志。
### 2.1.4 **Packages** 文件夹：
- **用途**：存放项目使用的所有 Unity 包（如 **ML-Agents**、**TextMeshPro**、**Cinemachine** 等）。Unity 使用包来管理外部依赖，类似于其他开发环境中的包管理器。这个文件夹包含了所有从 Unity Package Manager 安装的包。

### 2.1.5 **ProjectSettings** 文件夹：
- **用途**：存放项目的设置文件，包括图形设置、输入设置、标签设置、层级设置、序列化设置等。它控制项目的整体配置。

### 2.1.6 **Temp** 文件夹：
- **用途**：临时文件夹，存放 Unity 编辑器运行时生成的中间文件。这些文件通常不需要手动管理，Unity 会自动生成并清理它们。

### 2.1.7 **UserSettings** 文件夹：
- **用途**：存放与用户相关的设置文件，如 Unity 编辑器的用户偏好设置、布局等。这个文件夹通常是与项目本身无关的，只与编辑器的配置相关。




## 2.2 磁条
### 2.2.1 物理特性
- 宽度30mm
### 2.2.2 仿真处理
#### 2.2.2.1 **磁条的表示**：
- `MagneticTape` 通过 `LineRenderer` 组件来表示磁条。在 Unity 中，`LineRenderer` 可以绘制由多个连接点组成的线条。该脚本利用 `LineRenderer` 中的 `positionCount` 和 `GetPositions` 方法获取磁条的所有采样点，存储在 `points` 数组中。

- 每个采样点是磁条上的一个离散点，`points` 数组包含了所有这些点的位置。

#### 2.2.2.2 **磁场的计算**：
- **积分方法**：为了计算某个传感器位置感受到的磁场，脚本对磁条上的每个线段进行离散积分。每个线段是由两个点 (`start`, `end`) 组成。脚本在每个线段上执行积分，逐渐计算出传感器位置的磁场。
    - 每段线段被划分为若干小段（通过 `integrationStepsPerSegment` 来控制分割数量）。
    - 对于每一小段，脚本通过 `Lerp` 方法计算出该段的中点位置作为采样点。

#### 2.2.2.3 **磁场的衰减**：
- **垂直衰减**：
	- 垂直方向上的衰减遵循**幂律衰减公式** `B = B0 / (dz + z0)^alpha`
		- 其中 `dz` 是传感器与采样点在 y 轴上的距离差;
		- `z0` 是防止距离过近时产生无限大磁场的一个常数;
		- `alpha` 是垂直衰减的指数。
- **水平衰减**：
	- 水平方向上的衰减遵循高斯衰减公式 `B = B0 * exp(-r_h^2 / (2 * sigma^2))`
		- 其中 `r_h` 是传感器和采样点在 x 和 z 轴上的水平距离;
		- `sigma` 控制水平方向的衰减宽度。
- 这两个衰减函数共同决定了从磁条到传感器的磁场强度。

#### 2.2.2.4 **磁场矢量的计算**：
- 每个采样点对磁场的贡献不仅依赖于磁场强度（标量），还依赖于磁场的方向。
- 磁场的方向是从采样点指向传感器的，因此 `direction = delta.normalized`，`delta` 是从采样点到传感器的向量。
- 最终的磁场矢量是由每个采样点的磁场强度和方向的乘积得到的。
- 所有采样点的贡献累加起来得到总的磁场矢量 `totalMagneticField`。

#### 2.2.2.5 **有效范围**：
- 如果传感器距离磁条超过了设定的 **有效范围**（`effectiveRange`，在脚本中是 0.2f，即 20cm），则该采样点的磁场对传感器的影响会被忽略。

#### 2.2.2.6 **缩放系数**：
- 最终计算出的磁场矢量会乘上一个 **缩放系数**（`scale`），用来调整磁场的强度。

### 2.2.3 主要方法：
1. **`GetMagneticField(Vector3 sensorPosition)`**：
    - 计算并返回传感器位置感受到的磁场矢量。
    - 对每个采样点（通过线段的离散积分）计算磁场，最后累加得到传感器感受到的总磁场。

2. **`GetMagneticFieldScalar(Vector3 sensorPosition)`**：
    - 计算并返回磁场强度的标量值（即磁场矢量的大小 `magnitude`）。

3. **`OnDrawGizmosSelected()`**：
    - 用于在 Unity 编辑器中可视化磁条的每个采样点位置。每个采样点都会用绿色小球表示，用于调试和查看磁条的分布。

### 2.2.4 总结：
- **磁条模拟**：通过积分计算磁场，模拟了磁条上每个点对传感器产生的磁场影响。
- **衰减**：垂直和水平距离的衰减使得离磁条较远的传感器感受到的磁场较弱。
- **有效范围**：超出有效范围的磁场被忽略，避免了不相关的计算。
- **缩放**：使用 `scale` 来调整磁场强度，以适应不同的场景和需要。

这个实现通过模拟磁条的磁场衰减和传感器位置，能够较为准确地计算出传感器所受的磁场，并支持在 3D 场景中进行可视化。




## 2.3 磁传感器
### 2.3.1 物理特性


### 2.3.2 仿真处理


- 10 10 无信号


## 2.4 车身本体
车体 70cm 50cm
车轮 15cm 5cm

20 27.5

MyCar_Agent 上的 Rigidbody.mass = 整车总质量
  ↑ 这包括了车体 + 4个轮子的总重量
  ↑ 例如：2000 kg 就是整个车（从车架到轮子）的总重
  ↑ 关乎整车的线性运动、加速度、重力等

WheelCollider.mass = 轮子的转动特性参数
  ↑ 这不是"轮子有多重"
  ↑ 这只是影响轮子自转的转动惯量
  ↑ 不计入整车质量（整车质量已经在 Rigidbody.mass 里了）
  ↑ 决定轮子转起来需要多大扭矩

## 2.5 训练
mlagents-learn ML-Agents/Config/CarTest.yaml --run-id=CarTEST1120 --force --results-dir "D:/XiaoYiFei/Project/Unity/XYF_Car_Test/Assets/ML-Agents/Results"

mlagents-learn ML-Agents/Config/RealCarTest.yaml --run-id=RealCarTEST1120 --force --results-dir "D:/XiaoYiFei/Project/Unity/XYF_Car_Test/Assets/ML-Agents/Results"


mlagents-learn ML-Agents/Config/CarTest.yaml --run-id=CarTEST1120
mlagents-learn CarTest.yaml --run-id=CarTEST1119_first5e
ob



## 2.6 平台
### 2.6.1 必选组件
#### 2.6.1.1 **Collider 碰撞器组件**
平台必须拥有一个 **碰撞体**，用来与车体进行碰撞检测。如果平台没有碰撞体，车体将无法与其交互，可能导致车体穿过平台或掉落。
- **常用组件：** `BoxCollider` 或 `MeshCollider`
##### 2.6.1.1.1 `BoxCollider`（适用于简单的矩形平台）
- **作用：** 为平台提供一个简单的矩形或立方体形状的碰撞体。
- **配置：**
    - **Size:** 设置平台的宽度、高度和深度，确保它覆盖整个平台的表面。
    - **Center:** 设置碰撞体的位置，通常保持默认（0, 0, 0），除非需要调整位置。

##### 2.6.1.1.2 `MeshCollider`（适用于复杂形状的平台）
- **作用：** 为平台提供一个与网格形状匹配的碰撞体，适用于复杂形状的物体。
- **配置：**
    - **Mesh:** 选择与平台形状匹配的网格。一般情况下，选择平台本身的网格（可以直接选平台的 Mesh）。
    - **Convex:** 如果平台需要与动态物体发生碰撞，确保勾选 **Convex** 选项（凸面模式）。对于复杂的平台形状，勾选此选项可以确保性能更加高效。
    - **Is Trigger:** 如果你不希望平台与其他物体发生物理碰撞（比如让车体穿过平台），可以勾选 **Is Trigger**，这将使平台变成触发器。

#### 2.6.1.2 **Rigidbody 刚体组件**
如果平台本身是 **静态的**，通常不需要 `Rigidbody`，因为它不会移动或者受物理引擎的影响。你可以通过勾选 `Is Kinematic` 来保持平台的静态性质。但如果平台需要在物理引擎中动态响应（例如受力、移动等），则需要添加 `Rigidbody`。

- **作用：** 为平台提供物理模拟支持，允许平台受到力、重力等影响。
- **配置：**
    - **Is Kinematic:** 如果平台是静态的（不会移动），勾选此选项以禁用物理引擎对平台的影响。这样平台不会受到重力或其他力的作用，但仍然可以与其他物体发生碰撞。
    - **Use Gravity:** 如果你希望平台受重力影响，可以勾选此选项。一般来说，平台不会自由下落，但可以让它受重力影响时增加一些物理效果。
    - **Constraints:** 如果你希望平台不发生旋转或位移，可以使用 `Constraints` 限制平台的移动。例如，锁定平台的 **X、Y、Z 轴旋转**，防止平台发生任何旋转。

### 2.6.2 可选组件
#### 2.6.2.1 **Surface Material（地面材质）**
地面材质可以通过 **Physics Material** 来改变平台与车轮之间的摩擦系数，进而影响车轮在平台上的行为。你可以根据需要添加 `PhysicMaterial` 来调整摩擦力和弹性。

- **作用：** 修改平台表面的摩擦力，影响车轮与平台的滑动或抓地效果。
- **配置：**
    - **Friction Combine:** 控制摩擦力合并模式（例如 `Average`、`Minimum`、`Multiply` 等）。
    - **Dynamic Friction:** 动态摩擦系数，影响物体在平台上滑动时的摩擦力。
    - **Static Friction:** 静态摩擦系数，影响物体在平台上静止时的摩擦力。
    - **Bounciness:** 弹性系数，控制物体与平台碰撞时的反弹效果。

#### 2.6.2.2 **Light（可选）**
如果你希望平台有视觉效果（例如，光照），可以给平台添加一个 **Light** 组件（例如，`PointLight` 或 `SpotLight`）。这通常用于动态照亮场景中的平台，或者在测试时需要光照。

- **作用：** 给平台添加光源，模拟不同的光照效果。
- **配置：**
    - **Intensity:** 设置光源的亮度。
    - **Range:** 设置光源的照射范围。
    - **Color:** 设置光源的颜色。

#### 2.6.2.3 **Visual Effects（可选）**
可以为平台添加视觉效果（例如，`ParticleSystem` 或自定义的材质效果）来增强平台的外观。
- **作用：** 为平台添加特效，增加视觉表现（如平台移动时的尘土、风或撞击效果）。
- **配置：** 根据特效需求配置粒子系统的发射速率、速度、颜色等。

#### 2.6.2.4 **AudioSource（可选）**
如果你希望在平台与车体发生碰撞时播放声音（例如，车体碰到平台时的音效），可以给平台添加 **AudioSource** 组件。
- **作用：** 播放与平台相关的声音效果（例如撞击声、车轮与地面摩擦声等）。
- **配置：**
    - **AudioClip:** 设置需要播放的音频文件。
    - **Play On Awake:** 设置是否在平台初始化时自动播放声音。

### 2.6.3 完整配置示例：

#### 2.6.3.1 简单平台（矩形，静态）

- **`BoxCollider`**：设置平台的碰撞体。
    
    - `Size`: 设置为平台的大小。
        
    - `Center`: 根据需要调整位置。
        
- **`Rigidbody`**：
    
    - 勾选 **Is Kinematic**，确保平台静止不动。
        
- **`PhysicMaterial`**（可选）：设置适当的摩擦系数。
    

#### 2.6.3.2 复杂平台（网格形状）

- **`MeshCollider`**：为平台网格创建碰撞体。
    
    - `Convex`: 勾选此项以确保优化性能。
        
    - `Is Trigger`: 如果不想让平台与物体发生实际物理碰撞，可以勾选此项。
        
- **`Rigidbody`**：
    
    - **Is Kinematic**：如果平台不需要移动，可以勾选此项。
        

#### 2.6.3.3 动态平台（可移动平台）

- **`BoxCollider`** 或 **`MeshCollider`**：为平台设置碰撞体。
    
- **`Rigidbody`**：
    
    - 取消 **Is Kinematic**，使平台能受到物理模拟。
        
    - 勾选 **Use Gravity**，让平台受重力影响。
        
    - 根据需要设置 **Constraints**，限制平台的旋转或位置。
        

### 2.6.4 结论

对于一个静态的无人车平台，**`BoxCollider`** 和 **`Rigidbody`（`Is Kinematic` 勾选）** 是必选组件。其他如 **`PhysicMaterial`**（设置摩擦系数）和 **`AudioSource`**（音效）可以根据需求选择添加。

如果你的平台需要动态响应（例如移动或受到力的作用），你可以考虑添加 **`Rigidbody`** 并适当调整参数。


### 2.6.5 整体规划
一阶段：强化学习大模型训练与验证
	采集传感器读数 → 传感器示数归一化 → 大模型(增添Domain Randomization) → 输出策略 → 实际控制
	- 
	   传感器读数 → 特征提取（归一化+计算centroid等） 
           ↓
       大模型训练（离散动作空间：9个策略）
           ↓
       Domain Randomization（训练时随机化物理参数）
           ↓
       输出策略ID（0-8）→ 映射为控制量 → 实际控制
           ↓
       多场景验证（测试鲁棒性）
二阶段：大模型到规则库
	使用可行的大模型，继续使用测试场景，收集信息和决策，使用python决策树模拟
三阶段：实车测试
	把决策树布置到实际车轮，进行测试，微调决策判断的阈值






# 3 指令速查
## 3.1 备份环境
conda create --name IFR_mlagents_cpu --clone IFR_mlagents

## 3.2 显卡状态查看
nvidia-smi

## 3.3 **mlagents-learn**
- mlagents-learn <配置文件> [选项]
```
mlagents-learn 
ML-Agents/Config/MyCarAgent_251217_UTF8.yaml 
--run-id=MyCarAgent_251217_BL  
--resume  / --force
--results-dir "D:/XiaoYiFei/Project/Unity/XYF_Car_Test/Assets/ML-Agents/Results" 
--time-scale=20 
--num-envs=4 
--env="D:/XiaoYiFei/Project/Unity/XYF_Car_Test/Output/BL_251217/XYF_Car_Test.exe"  
--width=640 --height=480 / ----no-graphics
```

mlagents-learn ML-Agents/Config/MyCarAgent_251217.yaml --run-id=MyCarAgent_251217 --force --results-dir "D:/XiaoYiFei/Project/Unity/XYF_Car_Test/Assets/ML-Agents/Results"



**恢复训练**
mlagents-learn ML-Agents/Config/MyCarAgent_251217.yaml --run-id=MyCarAgent_251217_BL --force --results-dir "D:/XiaoYiFei/Project/Unity/XYF_Car_Test/Assets/ML-Agents/Results" --time-scale=20

**可执行文件并行训练**
1. **File → Build Settings**
2. 选择 **Windows, Mac, Linux**
3. 点击 **Build**
4. 保存到如 `D:/XiaoYiFei/Project/Unity/XYF_Car_Test/Build/CarGame.exe`

mlagents-learn ML-Agents/Config/MyCarAgent_251217_UTF8.yaml --run-id=MyCarAgent_251217_BL --resume --results-dir "D:/XiaoYiFei/Project/Unity/XYF_Car_Test/Assets/ML-Agents/Results" --time-scale=20 --num-envs=4 --env="D:/XiaoYiFei/Project/Unity/XYF_Car_Test/Output/BL_251217/XYF_Car_Test.exe"  --width=640 --height=480



## 3.4 问题与修改总结

### 3.4.1 问题1：训练初期 - 原地摆动不前进
**原因**：
- 前进奖励权重(2.0)不够高
- 倒退惩罚(-2.0)不足以压制其他奖励组合
- 对齐、磁强差等奖励可累加到2.8分，倒退也能得正分

**修改**：重构为纯标量奖励函数
- 移除依赖矢量方向的计算（`fieldDir`、`align`、`velAlong`）
- 改用6个传感器标量强度的前后差、左右差
- 添加累计前进距离奖励

### 3.4.2 问题2：90度弯道无法转弯
**原因**：
- 稳定性奖励(0.5)惩罚角速度，抑制转向
- 前向速度奖励(2.0)只鼓励沿当前车头前进
- 缺乏左右转向引导信号

**修改**：添加转向引导机制
```
// 根据左右传感器强度差判断转向
lateralGradient = (右强度 - 左强度)
if (左强>右) → 奖励右转
if (右强>左) → 奖励左转

// 权重调整
w_stability: 0.5 → 0.2  (降低，允许转向)
w_turning: 新增 0.8     (引导转向)
```

### 3.4.3 核心改进

|奖励项|初版|修正后|作用|
|---|---|---|---|
|前向速度|2.0|2.0|保持前进|
|转向引导|❌|**0.8**|弯道转向|
|稳定性|0.5|**0.2**|允许转弯|
|前后梯度|1.0|1.0|车头对准磁带|



# 4 Git
## 4.1 建立仓库
1. **登录到 GitHub**：
    - 打开 [GitHub](https://github.com/) 网站并登录你的帐户。如果没有帐户，可以注册一个新帐户。
        
2. **创建一个新的仓库**：
    - 登录后，在右上角点击 **"+"**，然后选择 **"New repository"**。
    - 填写仓库信息：
        - **Repository name**：输入仓库的名称，例如：`MyUnityProject`。
        - **Description**：可以添加仓库的描述，说明它是什么项目（此项可选）。
        - **Visibility**：选择仓库的可见性，**Public**（公开）或 **Private**（私密)
        - **Initialize this repository with a README**：不要勾选这个选项（因为你将从本地上传文件，初始化仓库时不要添加 README 文件）。
    - 点击 **Create repository** 按钮。
        
3. **记下仓库的 URL**：
    - 创建完成后，GitHub 会显示一个新页面，提供了仓库的 URL（类似于 `https://github.com/YourUsername/MyUnityProject.git`）。稍后你需要使用这个 URL 来将本地仓库与 GitHub 仓库连接。
## 4.2 本地git
- **确保本地安装 Git**：
    - 如果还没有安装 Git，可以从 [Git 官网](https://git-scm.com/) 下载并安装 Git。
    - 安装后，在命令行中输入 `git --version` 来检查是否安装成功。

- **配置 Git 用户信息**：
    - 在命令行中设置你的 Git 用户名和电子邮件地址，这些信息将出现在你提交的记录中。
    - 这两个设置是 Git 的全局配置，适用于所有仓库。
```
git config --global user.name "Your Name" 
git config --global user.email "your.email@example.com"
```

- **初始化本地 Git 仓库**：
    - 打开命令行，进入你本地的项目文件夹。如果你的项目还没有 Git 仓库，使用以下命令初始化一个新的 Git 仓库：
    - 这会在项目文件夹中创建一个 `.git` 文件夹，标识该目录为 Git 仓库。
```
cd /path/to/your/project 
git init
```

## 4.3 建立联系
- **连接本地仓库和远程 GitHub 仓库**：
    - 在你的本地项目中，添加 GitHub 仓库作为远程仓库。使用以下命令来设置远程仓库 URL：
```
git remote add origin https://github.com/YourUsername/MyUnityProject.git`
```

- **创建 `.gitignore` 文件**（可选）：
    - Unity 项目通常会生成一些不需要版本控制的文件，如 `Library/` 和 `Temp/` 文件夹。在提交项目文件时，我们可以创建一个 `.gitignore` 文件，忽略这些不需要的文件。
    
    在项目根目录创建一个 `.gitignore` 文件，并添加如下内容：
```
# 4 Unity specific 
[Ll]ibrary/ 
[Tt]emp/ 
[Tt]argets/ 
obj/ 
UnityGenerated/

```

- **查看本地仓库状态**：
    - 使用 `git status` 命令查看仓库的当前状态。确保所有需要上传的文件都已经被 Git 跟踪：
    
- **添加文件到 Git 暂存区**：`git add .`
    - 将所有需要推送到 GitHub 的文件添加到 Git 暂存区：
    
- **提交文件**：`git commit -m "Initial commit"`

- **推送本地提交到远程 GitHub 仓库**：
    - 将本地提交推送到 GitHub 上的远程仓库。因为这是第一次推送，你需要使用 `--set-upstream` 参数来指定将本地分支（例如 `master` 或 `main`）与远程仓库的分支关联：
```
git push --set-upstream origin master
```

如果你的仓库使用的是 `main` 分支，使用以下命令：
```
git push --set-upstream origin main
```
这个命令会将本地的 `master`（或 `main`）分支推送到远程仓库，并将其设置为上游分支（即以后的 `git push` 可以直接使用，无需每次指定远程仓库和分支）。


- **验证上传**：
	- ssh / 账户密码
    - 上传完成后，访问 GitHub 仓库页面，确认文件已经成功上传。
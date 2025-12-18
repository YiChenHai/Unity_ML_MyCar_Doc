# 1 环境安装
## 1.1 参考资料
[Installation | ML Agents | 4.0.0](https://docs.unity3d.com/Packages/com.unity.ml-agents@4.0/manual/Installation.html)
[GitHub - Unity-Technologies/ml-agents: The Unity Machine Learning Agents Toolkit (ML-Agents) is an open-source project that enables games and simulations to serve as environments for training intelligent agents using deep reinforcement learning and imitation learning.](https://github.com/Unity-Technologies/ml-agents)

## 1.2 操作步骤
### 1.2.1 安装unity及ML库
#### 1.2.1.1 下载unity hub
[Unity官方下载_Unity新版_从Unity Hub下载安装 | Unity中国官网](https://unity.cn/releases)
![[Pasted image 20251118104101.png]]

![[Pasted image 20251118104134.png]]
#### 1.2.1.2 unity hub许可证获取
![[Pasted image 20251118104333.png]]
![[Pasted image 20251118104419.png]]

如果获取失败，修改当前用户对unity文件夹的写入权限，重新获取
![[Pasted image 20251118104551.png]]

#### 1.2.1.3 安装unity
![[Pasted image 20251118104625.png]]
![[Pasted image 20251118104635.png]]
#### 1.2.1.4 安装中文包
![[Pasted image 20251118113518.png]]
![[Pasted image 20251118113547.png]]
![[Pasted image 20251118113933.png]]
![[Pasted image 20251118113201.png]]
![[8d8a4df6-44ce-4066-9ae0-eff3198b7153 1.png]]
![[Pasted image 20251118114006.png]]
#### 1.2.1.5 安装ML库
![[Pasted image 20251118114026.png]]
![[Pasted image 20251118114220.png]]

### 1.2.2 安装Python 3.10.12
Conda安装好后，打开终端，执行以下命令，搭建并激活Python 3.10.12虚拟环境。
```
conda create -n mlagents python=3.10.12 --platform win-64
```

安装时，激活您的虚拟环境并执行以下命令：
```
python -m pip install mlagents==1.1.0
```

该系统将安装 PyPi 上最新版本的 ML-Agent Python 包及相关依赖。如果构建的Wheels失败，安装 pip 前执行以下命令：

```shell
conda install "grpcio=1.48.2" -c conda-forge
```

安装 Python 包时，[setup.py 文件](https://github.com/Unity-Technologies/ml-agents/blob/release/4.0.0/ml-agents/setup.py)中列出的依赖也已安装。其中包括[PyTorch](https://docs.unity3d.com/Packages/com.unity.ml-agents@4.0/manual/Background-PyTorch.html)。



## 1.3 运行demo验证环境
### 1.3.1 下载demo工程文件
[GitHub - Unity-Technologies/ml-agents: The Unity Machine Learning Agents Toolkit (ML-Agents) is an open-source project that enables games and simulations to serve as environments for training intelligent agents using deep reinforcement learning and imitation learning.](https://github.com/Unity-Technologies/ml-agents)

### 1.3.2 将例程放入工程目录下
![[Pasted image 20251118115903.png]]

### 1.3.3 打开例程场景
![[Pasted image 20251118115939.png]]

![[Pasted image 20251118120853.png]]
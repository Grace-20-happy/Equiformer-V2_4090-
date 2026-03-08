# Equiformer-V2_4090-
Equiformer V2_4090新环境搭建避坑
好吧其实是在OCP开源框架里安装的Equiformer-V2_4090
1报错现象 cp: -r not specified,
1根因分析 尝试用普通命令移动数据集文件夹。
1终极解法 使用 cp -r 进行递归复制。
2报错现象 lmdb.Error: Is a directory,
2根因分析 框架期待读取单文件，但被传入了一个同名的 .lmdb 文件夹。
2终极解法 将文件夹内的本体数据解包，把内部的 data.mdb 暴露出来。
3报错现象 AssertionError: No LMDBs found,
3根因分析 数据提取后变为 data.mdb，框架扫描器因缺少 .lmdb 后缀拒绝加载。
3终极解法 物理伪装法：直接将核心数据文件重命名：cp data.mdb data.lmdb，完美通过框架的扩展名校验。
注意我的3终极解法还没有实现，因为我不知道怎么把.mdb转化成.lmdb文件（上面提供的方法我实际上失败了）

在AutoDL上，我在下面的基础上
* **计算平台**: AutoDL
* **GPU**: RTX 4090 (24GB 显存)
* **操作系统**: Ubuntu 22.04
* **基础镜像**: PyTorch 2.1.0 / CUDA 12.1 / Python 3.10
搭建了equiformer V2-4090私有镜像，其中，在处理图神经网络（GNN）时，PyTorch Geometric (PyG) 的底层 C++ 算子极其依赖 CUDA 版本已对齐，且版本完美符合论文EQUIFORMERV2: IMPROVED EQUIVARIANT TRANSFORMER  FOR SCALING TO HIGHER-DEGREE REPRESENTATIONS的要求

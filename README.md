# -habitat-sim-
habitat-sim是habitat的仿真器本体
habitat-lab是高级api的集合，比如任务成功的判定等等
这两个模块都要安装，下面以跑通habitat中的example为例记录踩坑过程

## autodl服务器

首先进行学术加速
`source /etc/network_turbo`
如果在git clone的时候经常挂掉，这时候升级一下solver就行
`conda install -n base conda-libmamba-solver`

## 下面按照正常流程安装habitat-sim
```
git clone https://github.com/facebookresearch/habitat-sim.git
conda create -n habitat python=3.9 cmake=3.14.0
conda activate habitat
conda install -n base conda-libmamba-solver
conda config --set experimental_solver libmamba
conda install habitat-sim withbullet -c conda-forge -c aihabitat
```
### 下载habitat-lab
```
git clone --branch stable https://github.com/facebookresearch/habitat-lab.git
cd habitat-lab
pip install -e habitat-lab
pip install -e habitat-baselines  # install habitat_baselines
```
### 下载测试数据
No module named 'habitat_sim'怎么解决？
```
python -m habitat_sim.utils.datasets_download --uids habitat_test_scenes --data-path data/
python -m habitat_sim.utils.datasets_download --uids habitat_example_objects --data-path data/
```


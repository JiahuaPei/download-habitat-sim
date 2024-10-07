# -habitat-sim-
habitat-sim是habitat的仿真器本体
habitat-lab是高级api的集合，比如任务成功的判定等等
这两个模块都要安装，下面以跑通habitat中的example为例记录踩坑过程

##autodl服务器

首先进行学术加速
`source /etc/network_turbo`
如果在git clone的时候经常挂掉，这时候升级一下solver就行
`conda install -n base conda-libmamba-solver`

##下面按照正常流程安装habitat-sim
```
git clone https://github.com/facebookresearch/habitat-sim.git
conda create -n habitat python=3.9 cmake=3.14.0
conda activate habitat
#conda install -n base conda-libmamba-solver
conda install habitat-sim withbullet -c conda-forge -c aihabitat
```



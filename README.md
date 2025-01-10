# 在AutoDL中运行-habitat-sim-踩坑合集！！！
# 宝宝坐轮椅级别的文档
habitat-sim是habitat的仿真器本体
habitat-lab是高级api的集合，比如任务成功的判定等等
这两个模块都要安装，下面以跑通habitat中的example为例记录踩坑过程

## autodl服务器

首先进行学术加速，每次重新打开终端的时候都需要运行下面这条命令，特别是需要git clone的时候
`source /etc/network_turbo`
如果在git clone的时候经常挂掉，这时候升级一下solver就行
`conda install -n base conda-libmamba-solver`

## 下面按照正常流程安装habitat-sim
```
source /etc/network_turbo
git clone https://github.com/facebookresearch/habitat-sim.git
conda create -n habitat python=3.9 cmake=3.14.0
conda init
#restart the shell

conda activate habitat
conda install -n base conda-libmamba-solver
conda config --set experimental_solver libmamba
conda install habitat-sim withbullet -c conda-forge -c aihabitat
```

不出意外这时进入python import habitat和import habitat_sim会报错，缺少libEGL.so.1和libOpenGL.so.0文件
这个文件的路径如下，下面构造符号链接来实现添加文件

```
export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/libGL.so.1:$LD_LIBRARY_PATH
sudo ln -s /usr/lib/x86_64-linux-gnu/libGL.so.1 /usr/lib/libEGL.so.1
sudo apt-get update
sudo apt install libopengl0 -y
sudo apt-get install --reinstall libegl1-mesa
```
### 下载habitat-lab
```
git clone --branch stable https://github.com/facebookresearch/habitat-lab.git
cd habitat-lab
pip install -e habitat-lab
pip install -e habitat-baselines  # install habitat_baselines
```
### 下载测试数据
No module named 'habitat_sim'怎么解决？如上解决动态链接库即可
先下载一下git-lfs
```
sudo apt install git-lfs
git lfs install
```
然后再按照官网的下载进行
```
python -m habitat_sim.utils.datasets_download --uids habitat_test_scenes --data-path data/
python -m habitat_sim.utils.datasets_download --uids habitat_example_objects --data-path data/
```
### 下面就没必要做窗口测试，打开notebook教程即可
首先为notebook添加habitat内核
```
conda install ipython
conda install ipykernel
python -m ipykernel install --user --name habitat --display-name "habitat"
```

## 使用VNC远程桌面，按照教程
此外：
```
rm -rf /tmp/.X1*  # 如果再次启动，删除上一次的临时文件，否则无法正常启动
USER=root /opt/TurboVNC/bin/vncserver :1 -desktop X -auth /root/.Xauthority -geometry 1920x1080 -depth 24 -rfbwait 120000 -rfbauth /root/.vnc/passwd -fp /usr/share/fonts/X11/misc/,/usr/share/fonts -rfbport 6006
export DISPLAY=:1
startxfce4
#在vnc中测试图形化（暂时失败）
python examples/viewer.py --scene data/scene_datasets/habitat-test-scenes/skokloster-castle.glb
```


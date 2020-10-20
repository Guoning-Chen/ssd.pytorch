# 运行train.py
## 准备数据集
1. 选择一个放置数据集的位置，例如我想放在D盘，设置成D:/Dateset/ssd-pytorch，接下来我就用$指代这个路径

2. 准备数据集：
（1）data/scripts/文件夹下的三个.sh文件文件是用来下载和整理数据集的，
但是直接运行的话下载很慢，所以预先使用链接在迅雷中下载好下面这6个文件：
（COCO2014）
http://images.cocodataset.org/zips/train2014.zip
http://images.cocodataset.org/zips/val2014.zip
http://images.cocodataset.org/annotations/annotations_trainval2014.zip
（VOC2007）
http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtrainval_06-Nov-2007.tar
http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtest_06-Nov-2007.tar
（VOC2012）
http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar
下载好之后全部放到上面的路径中。
3. 整理数据集：
（1）安装软件git bash，然后cd到项目所在的位置（注意要用左下划线/），例如我是：
`cd C:/Users/chengn/Documents/Project/ssd.pytorch`
（2）运行.sh文件（里面其实就是一连串的shell命令），注意如果.sh文件把下面的$替换成步骤1中的路径：
COCO2014：
`sh data/scripts/COCO2014.sh $`
（运行40分钟左右，过程中屏幕会卡在`Combining train and val images`很长一段时间是正常的，不用担心。）
VOC2007：
`sh data/scripts/VOC2007.sh $`（约3分钟）
VOC2012：
`sh data/scripts/VOC2012.sh $`（约5分钟）
最后剩下annnotations、images、VOCdevkit三个文件夹和instances_trainval35k.json.zip文件
4. 在windows的根目录下创建软连接：管理员身份打开cmd运行
`mklink /d C:\Users\chengn\data D:\Dateset\ssd-pytorch`
## 解决因为版本不同而出现的 bug
1. 错误1：
`The shape of the mask [32, 8732] at index 0does not match the shape of the indexed tensor [279424, 1] at index 0`
解决方法：
将layers/modules/multibox_loss.py中的这两行交换位置：
`loss_c[pos] = 0  # filter out pos boxes for now
loss_c = loss_c.view(num, -1)`
2. 错误2
`IndexError: invalid index of a 0-dim tensor. Use tensor.item() to convert a 0-dim tensor to a Python number`
解决方法：
把错误行的 .data[0] 修改为 .item() ，注意一共有好几处，都需要替换
## 启用 visdom可视化
1. pip install visdom
2. python -m visdom.server（在运行train.py文件之前）
3. 用浏览器打开http://localhost:8097/
4. 运行train.py文件
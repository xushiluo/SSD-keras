# 代码使用指南
### 1.代码文件阐述
layers.py 中定义了两个层：L2归一化层和一个产生默认模板框的层

model.py 中定义了SSD模型以及损失函数类

utils.py 中定义了一些工具类/函数，其中BBoxUtility中的功能有：将训练数据中的box分配给模板框(即产生y_true)，
对模型的输出做非极大抑制处理等。函数write_priorboxes的作用是将模型的默认模板框输出到文件，
因为这些模板框每次运行都相同(PrioirBox的输出与x无关)，所以就写成文件。draw函数用于可视化预测结果

dataset.py中包含了一个训练数据生成器，一会儿会提到训练数据格式。

train.py 训练代码

test.py 测试代码

config.py 统一存放了一些训练有关的变量

### 2.训练格式
训练文件可以查看train_label.keras，其中每一行都代表一张图片。

每一行分为两个部分，以空格' '相连接，前半部分为图片的路径，后半部分为boxes

主要说一下后半部分：
box的参数为[xmin, ymin, xmax, ymax，class...]

即如果您的图片中的物体的classes的数量为NUM_CLASS（不用包含背景），
则每个box的大小为4 + NUM_CLASS。一张图片可以包含多个box，如果有n个
box，则后半部分数字的个数为n * (4 + NUM_CLASS)。

后半部分中的坐标信息无需经过归一化，数字之间以逗号进行分隔。

比如NUM_CLASS = 1，则下面的例子中，图片的路径为train_data\506a95a3364230b47e0217f1cdb03cb2.jpg，一共有3个box：
[66,140,198,301,0], [200,33,318,333,0], [305,123,454,302,0]

train_data\506a95a3364230b47e0217f1cdb03cb2.jpg 66,140,198,301,0,200,33,318,333,0,305,123,454,302,0

若您的box中的坐标已经进行了归一化，则可以将dataset.py中的第153，154行注释：

    # loc[:, ::2] /= img.shape[1]
    # loc[:, 1::2] /= img.shape[0]


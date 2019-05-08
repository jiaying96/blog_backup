mnist数据集下载：http://yann.lecun.com/exdb/mnist/

    from tensorflow.examples.tutorials.mnist import input_data
    
    # 导入 MNIST 数据
    mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)
    import tensorflow as tf
    import numpy as np
    import matplotlib.pyplot as plt
    
    # 查看数据集情况         
    #train训练集 test测试集  validation验证集
    #                                   样本数  多少维特征
    print(mnist.train.images.shape)      #(55000, 784)     图像是28px*28px  这28个点展开成一维的结果为28*28=784
    print(mnist.train.labels.shape)      #(55000, 10)
    print(mnist.test.images.shape)       #(10000, 784)
    print(mnist.test.labels.shape)       #(10000, 10)
    print(mnist.validation.images.shape) #(5000, 784)
    print(mnist.validation.labels.shape) #(5000, 10)
    
    ''' 
    训练数据的特征：是一个55000*784的Tensor    
    55000是第一个维度，是图片的编号     784是第二个维度，是图片中像素点的编号
    训练数据Label的特征：是一个55000*10的Tensor    
    55000是第一个维度，是图片的编号     10是第二个维度，有10个种类（0-9），只有一个值是1，其余为0 数值i代表下标为i对应的位置为1
    
    
    多分类任务：Softmax Regression模型 原理：将可以判定为某类的特征相加，将这些特征转化为判定这一类的概率
    
    '''
    
    #保存的图片路径
    picPath='images'
    # 打印训练集的前10张图片
    l = len(mnist.train.images[1])
    # 开平方  l_为28
    l0 = int(np.sqrt(l))
    
    #图片
    images = mnist.train.images
    # 标签
    labels = mnist.train.labels
    
    for i in range(10):
    	plt.subplot(2, 5, i + 1)
    	# 显示图片 imshow
    	plt.imshow(images[i].reshape(l0, l0))
    	# 添加标题为对应的label
    	# np.where 返回对应的索引
    	plt.title(int(np.where(labels[i] == 1)[0]))
    	# 保存 imsave
    	picName = str(i + 1) + '.jpg'
    	plt.imsave(picPath + "/" + picName, images[i].reshape((l0, l0)))
    plt.show()
    
    
    
    
    ''' 在训练集上训练模型，在验证集上检验效果并决定何时完成训练，在测试集上评测模型的效果''' 
    
    
    ''' InteractiveSession方法将这个session注册为默认的session    ！不同session之间的数据与运算应该是相对独立的''' 
    sess = tf.InteractiveSession()
    
    ''' palceholde为输入数据的地方 tf.float32是数据类型  [None, 784]None表示不限制条数的输入 784代表每条输入是一个784维度的向量''' 
    x = tf.placeholder(tf.float32, [None, 784])  
    
    ''' 
    存储数据的Tensor一旦使用掉就会消失，但是用来存储模型参数的Variable在模型训练迭代中是持久化的，长期存在并且在每轮迭代中被更新
    Variable用来存储模型参数
    ''' 
    W = tf.Variable(tf.zeros([784, 10]))
    b = tf.Variable(tf.zeros([10]))
    
    ''' 
    1.定义算法公式 就是神经网络前向传播（forward）时的计算
    算法Softmax Regression
    matmul为矩阵乘法  y=softmax(Wx+b)  
    '''   
    y = tf.nn.softmax(tf.matmul(x, W) + b)
    
    
    
    ''' 
    2.定义loss  损失函数（loss）为cross-entropy
    
    palceholde为输入数据的地方 tf.float32是数据类型  [None, 784]None表示不限制条数的输入 10代表每条输入是一个10维度的向量
    输入是真实的label(y_ ) ,用来计算损失函数cross-entropy 即loss
    H=-求和（yi_log(yi)） 
    tf.reduce_mean用来对每个batch数据结果求均值
    reduction_indices参数，表示函数的处理维度。当没有这个参数时取默认值None，将把input_tensor降到0维，也就是一个数
    reduction_indices=[1]代表横向压缩成一个数
    '''
    y_ = tf.placeholder(tf.float32, [None, 10])
    cross_entropy = tf.reduce_mean(-tf.reduce_sum(y_ * tf.log(y), reduction_indices=[1]))
    '''  
    3.选定优化器，并制定优化器优化loss
    优化算法采用随机梯度下降SGD（Stochastic Gradient Descent）
    定义好优化算法后，Tensorflow根据计算图自动求导，并根据反向传播算法进行训练，在每一轮迭代时更显参数来减小loss值
    0.5为设置的学习率 优化目标设定为cross-entropy          
    '''  
    train_step = tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)
    ''' 全局参数初始化器，直接执行run方法''' 
    tf.global_variables_initializer().run()
    '''  
    4.迭代的对数据进行训练
    随机抽取100个样本构成一个mini-batch,并feed给palceholder,调用train_step对这些样本进行训练
    随机梯度下降：使用一小部分样本进行训练（每次使用全部样本是传统的梯度下降）
    
    why随机梯度下降?
    如果每次都使用全部样本，计算量太大，有时也不容易跳出局部最优，只使用一小部分进行随机梯度下降，绝大部分比使用全部样本训练的收敛速度快很多
    '''
    for i in range(1000):
        batch_xs, batch_ys = mnist.train.next_batch(100)
        train_step.run({x: batch_xs, y_: batch_ys})
    
    '''
    5.在测试集或者验证集上对准确率进行测评，本例是在测试集
    
    （1）在对模型的准确率进行验证
    tf.argmax：从一个tensor中寻找最大值的序号 
    tf.argmax(y, 1)求该样本在各个预测数字中概率最大的那一个 
    tf.argmax(y_, 1)是样本的真是数字类别
    tf.equal判断预测的数字类别是否是正确的类别，最后返回计算分类是否正确的操作correct_prediction
    
    注：
    Tensorflow中的计算可以表示为一个有向图（或计算图），每一个运算操作作为一个节点，节点与节点之间的连接成为边
    在计算图的边中流动（flow）的数据称为张量（tensor）
    '''
    correct_prediction = tf.equal(tf.argmax(y, 1), tf.argmax(y_, 1))
    '''
    （2）统计全部预测的accuracy 
    先用tf.cast将之前correct_prediction输出的bool值转换为float32，再求平均
    '''
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    '''
    （3）将测试数据的特征和labe输入测评流程accuracy,计算模型在测试集上的准确率，再将结果打印出来
    accuracy.eval:
    在一个Seesion里面“评估”tensor的值（其实就是计算），首先执行之前的所有必要的操作来产生这个计算这个tensor需要的输入，
    然后通过这些输入产生这个tensor。在激发tensor.eval()这个函数之前，tensor的图必须已经投入到session里面，
    或者一个默认的session是有效的，或者显式指定session.
    '''
    print(accuracy.eval({x: mnist.test.images, y_: mnist.test.labels}))



![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110115848430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDEzNTI4Mg==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110115911858.png)
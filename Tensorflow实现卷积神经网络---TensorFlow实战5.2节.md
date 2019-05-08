    from tensorflow.examples.tutorials.mnist import input_data
    import tensorflow as tf
    mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)
    
    # 设置默认session
    sess = tf.InteractiveSession()
    
    # 定义初始化函数
    '''
    tf.truncated_normal(shape, mean=0.0, stddev=1.0, dtype=tf.float32, seed=None, name=None) 
    参数：
    shape: 一维的张量，也是输出的张量。   
    mean: 正态分布的均值。   
    stddev: 正态分布的标准差。
    dtype: 输出的类型。   
    seed: 一个整数，当设置之后，每次生成的随机数都一样。   
    name: 操作的名字。
    
    从截断的正态分布中输出随机值。 
    生成的值服从具有指定平均值和标准偏差的正态分布，如果生成的值大于平均值2个标准偏差的值则丢弃重新选择。
    '''
    # 给权重初始化一些随机的噪声来打破完全对称  比如截断的正态分布噪声 标准差设置为0.1
    def weight_variable(shape):
        initial = tf.truncated_normal(shape, stddev=0.1)
        return tf.Variable(initial)
    
    
    '''
    constant(value, dtype=None, shape=None, name="Const", verify_shape=False)
    value是必须的，可以是一个数值，也可以是一个列表
    后面四个参数可写可不写
    dtype表示数据类型，一般可以是tf.float32, tf.float64等
    shape表示张量的“形状”，即维数以及每一维的大小。如果指定了第三个参数，当第一个参数value是数字时，张量的所有元素都会用该数字填充：
    name可以是任何内容，主要是字符串就行
    verify_shape默认为False，如果修改为True的话表示检查value的形状与shape是否相符，如果不符会报错
    '''
    # 使用ReLU 给偏置增加一些小的正值（0.1）来避免死亡节点
    def bias_variable(shape):
        initial = tf.constant(0.1, shape=shape)
        return tf.Variable(initial)
    
    
    
    
    '''
    tf.nn.conv2d()是Tensorflow的二维卷积函数 
    def conv2d(input, filter, strides, padding, use_cudnn_on_gpu=None,data_format=None, name=None)
    input：输入
    filter：卷积的参数  [5,5,1,32] 
            前两个数字代表卷积核的尺寸 
            第三个数字代表有多少个channel（只有灰度单色所以为1，如果是彩色的RGB，channel应该是3） 
            第四个是卷积核的数量（也就是这个卷积层会提取多少类的特征）
    strides:卷积模板的步长  1代表不会遗漏的划过图片的每一个点 
    padding：边界的处理方式：SAME代表边界加上padding让卷积的输入和输出保持相同的尺寸（SAME）
    '''
    def conv2d(x, W):
        return tf.nn.conv2d(x, W, strides=[1, 1, 1, 1], padding="SAME")
    
    
    
    '''
    tf.nn.max_pool是Tensorflow的最大池化函数
    最大池化层会保留原始像素块中灰度最高的那一个像素 即保留最显著的特征
    value：需要池化的输入，一般池化层接在卷积层后面，所以输入通常是feature map，依然是[batch, height, width, channels]这样的shape
    ksize：池化窗口的大小，取一个四维向量，一般是[1, height, width, 1]，因为我们不想在batch和channels上做池化，所以这两个维度设为了1
    strides：和卷积类似，窗口在每一个维度上滑动的步长，一般也是[1, stride,stride, 1]
    padding：和卷积类似，可以取'VALID' 或者'SAME'
    
    返回一个Tensor，类型不变，shape仍然是[batch, height, width, channels]这种形式
    '''
    #使用2*2的最大池化 即将一个2*2的像素块降为1*1的像素块
    #因为希望整体上缩小图片尺寸，因此池化层的strides设置为横竖两个方向以2为步长
    def max_pool_2x2(x):
        return tf.nn.max_pool(x, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding="SAME")
    
    x = tf.placeholder(tf.float32,[None, 784])#特征
    y_ = tf.placeholder(tf.float32, [None, 10])#真实的label
    
    '''
    卷积神经网络会用到空间结构信息，需要将1D的输入向量转为2D的图片结构 1*784转为原始的28*28 
    [-1, 28, 28, 1]中第一个1表示样本数量不固定 最后一个1代表颜色通道数量
    tf.reshape是tensor的变形函数
    '''
    x_image = tf.reshape(x, [-1, 28, 28, 1])
    
    #第一个卷积层
    W_conv1 = weight_variable([5, 5, 1, 32])  #卷积核尺寸5*5 颜色通道是1 卷积核个数为32
    b_conv1 = bias_variable([32])
    h_conv1 = tf.nn.relu(conv2d(x_image, W_conv1) + b_conv1)  #使用从conv2函数进行卷积操作 加上偏置  再使用RELU激活函数进行非线性处理
    h_pool1 = max_pool_2x2(h_conv1)  #最大池化函数max_pool_2x2对卷积的输出结果进行池化操作
    
    #第二个卷积层   卷积核个数为64
    W_conv2 = weight_variable([5, 5, 32, 64])
    b_conv2 = bias_variable([64])
    h_conv2 = tf.nn.relu(conv2d(h_pool1, W_conv2) + b_conv2)
    h_pool2 = max_pool_2x2(h_conv2)
    
    W_fc1 = weight_variable([7 * 7 * 64, 1024])
    b_fc1 = bias_variable([1024])
    h_pool2_flat = tf.reshape(h_pool2, [-1, 7 * 7 * 64]) #变形转换为1D的向量
    h_fc1 = tf.nn.relu(tf.matmul(h_pool2_flat, W_fc1) + b_fc1)  #建立一个全连接层 隐含节点数为1024  再使用RELU激活函数
    
    keep_prob = tf.placeholder(tf.float32)
    h_fc1_drop = tf.nn.dropout(h_fc1, keep_prob)  #减轻过拟合 使用Dropout层
    
    
    '''
    过拟合：模型准确率在训练集上提高但是在测试集下降 
    使用Dropout层：通过一个placeholder传入keep_prob比率来控制
    训练的时候，随机丢弃一部分来减轻过拟合，预测时保留全部数据来追求最好的性能预测
    '''
    W_fc2 = weight_variable([1024, 10])
    b_fc2 = bias_variable([10])
    #dropout层的输出连接一个softmax层，得到最后的概率输出
    y_conv = tf.nn.softmax(tf.matmul(h_fc1_drop, W_fc2) + b_fc2)
    #cross_entropy为损失函数 优化器Adam  较小的学习速率1e-4
    cross_entropy = tf.reduce_mean(-tf.reduce_sum(y_ * tf.log(y_conv), reduction_indices=[1]))
    train_step = tf.train.AdamOptimizer(1e-4).minimize(cross_entropy)
    
    
    '''
    在测试集或者验证集上对准确率进行测评，本例是在测试集
    （1）在对模型的准确率进行验证
    tf.argmax：从一个tensor中寻找最大值的序号 
    tf.argmax(y_conv, 1)求该样本在各个预测数字中概率最大的那一个 
    tf.argmax(y_, 1)是样本的真实数字类别
    tf.equal判断预测的数字类别是否是正确的类别，最后返回计算分类是否正确的操作correct_prediction
    '''
    correct_prediction = tf.equal(tf.argmax(y_conv, 1), tf.argmax(y_, 1))
    
    
    '''
    （2）统计全部预测的accuracy 
    先用tf.cast将之前correct_prediction输出的bool值转换为float32，再求平均
    '''
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    
    #训练过程
    ''' 全局参数初始化器，直接执行run方法'''
    tf.global_variables_initializer().run()
    
    
    '''  
    设置训练时的dropout=0.5，随机抽取50个样本构成一个mini-batch，共进行20000次迭代训练，参与训练的样本数量为100万（20000*50）
    每100次训练，对准确率进行一次测评
    参数keep_prob的意思是:留下的神经元的概率,如果keep_prob为0的话, 就是让所有的神经元都失活
    '''
    for i in range(20000):
        batch = mnist.train.next_batch(50)
        if i % 100 == 0:
            train_accuracy = accuracy.eval(feed_dict={x: batch[0], y_: batch[1], keep_prob: 1.0})
            print("step %d, training accuracy %g" % (i, train_accuracy))
        train_step.run(feed_dict={x: batch[0], y_: batch[1], keep_prob: 0.5})
    
    
    '''
    全部训练完成后将测试数据的特征和label输入测评流程accuracy,计算模型在测试集上的准确率，再将结果打印出来
    '''
    print("test accuracy %g" % accuracy.eval(feed_dict={x: mnist.test.images, y_: mnist.test.labels, keep_prob: 1.0}))

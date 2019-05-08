按照某教程学习使用tensorboard时

代码：


    import tensorflow as tf
  
    def add_layer(inputs, in_size, out_size, activation_function=None):
        # add one more layer and return the output of this layer
        with tf.name_scope('layer'):
            with tf.name_scope('weights'):
                Weights = tf.Variable(tf.random_normal([in_size, out_size]), name='W')
            with tf.name_scope('biases'):
                biases = tf.Variable(tf.zeros([1, out_size]) + 0.1, name='b')
            with tf.name_scope('Wx_plus_b'):
                Wx_plus_b = tf.add(tf.matmul(inputs, Weights), biases)
            if activation_function is None:
                outputs = Wx_plus_b
            else:
                outputs = activation_function(Wx_plus_b, )
            return outputs
     
     
    # define placeholder for inputs to network
    with tf.name_scope('inputs'):
        xs = tf.placeholder(tf.float32, [None, 1], name='x_input')
        ys = tf.placeholder(tf.float32, [None, 1], name='y_input')
     
    # add hidden layer
    l1 = add_layer(xs, 1, 10, activation_function=tf.nn.relu)
    # add output layer
    prediction = add_layer(l1, 10, 1, activation_function=None)
     
    # the error between prediciton and real data
    with tf.name_scope('loss'):
        loss = tf.reduce_mean(tf.reduce_sum(tf.square(ys - prediction),
                                            reduction_indices=[1]))
     
    with tf.name_scope('train'):
        train_step = tf.train.GradientDescentOptimizer(0.1).minimize(loss)
     
    sess = tf.Session()
     
    # tf.train.SummaryWriter soon be deprecated, use following
    if int((tf.__version__).split('.')[1]) < 12 and int((tf.__version__).split('.')[0]) < 1:  # tensorflow version < 0.12
        writer = tf.train.SummaryWriter('logs/', sess.graph)
    else: # tensorflow version >= 0.12
        writer = tf.summary.FileWriter("logs/", sess.graph)
     
    # tf.initialize_all_variables() no long valid from
    # 2017-03-02 if using tensorflow >= 0.12
    if int((tf.__version__).split('.')[1]) < 12 and int((tf.__version__).split('.')[0]) < 1:
        init = tf.initialize_all_variables()
    else:
        init = tf.global_variables_initializer()
    sess.run(init)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109154947548.png)
生成logs文件夹

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109155044476.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/2019010915411348.png)
遇到
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109154339543.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDEzNTI4Mg==,size_16,color_FFFFFF,t_70)

**

## 解决方法

使用命令：tensorboard --logdir logs --host=127.0.0.1

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109154440198.png)

得到效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109154510782.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDEzNTI4Mg==,size_16,color_FFFFFF,t_70)
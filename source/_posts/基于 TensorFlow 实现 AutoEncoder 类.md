---

title: 基于 TensorFlow 实现 AutoEncoder 类

tag: [TensorFlow, Python]

date: 2018-06-19

---

上研期间接触到了 TensorFlow 以及深度学习中的 AutoEncoder 算法，但只是简单的进行了算法的实现。在最近的工作中，又一次需要利用 TensorFlow 实现 AutoEncoder 算法。考虑到代码的可移植性，因此直接将其整理为 AutoEncoder 类，后续直接调用该类即可实现 AutoEncoder 算法。  

<!--more-->

---

本文中设计的 AutoEncoder 类结构如下：

``` Python
class autoencoder(object):
    
    def __init__(self, sess, name, nodes):
        <code>
    def _build_net(self):
        <code>
    def _dense_layer(self):
        <code>
    def _activation(self):
        <code>
    def _loss_function(self):
        <code>
    def _optimizer(self):
        <code>
    def train(self):
        <code>
    def predict(self):
        <code>
    def get_feature(self):
        <code>
    def save(self):
        <code>
    def load(self):
        <code>
```

其中，`` __init__`` 函数是 autoencoder 类的构造器，包含3个参数，`` sess `` 为传入的tf会话，`` name`` 为实例化的 autoencoder 类的名字，`` nodes`` 为各编码层的节点数。``__init__`` 的具体实现如下：  

``` Python
def __init__(self, sess, name, nodes):
    self._sess = sess
    self._name = name
    self._nodes = nodes
    self._build_net()
```

将``sess``、``name``和``nodes``定义为外部可访问的属性：

```Python
@property
def sess(self):
    return self._sess

@property
def name(self):
    return self._name

@property
def nodes(self):
    return self._nodes
```

``_build_net``方法主要用于搭建 autoencoder 计算图，其具体实现如下：

``` Python
def _build_net(self):
    # check whether number of encode layers is valid
    if len(self._nodes) < 2:
        raise ValueError('At least 2 hidden layer should be specified.')
    
    with tf.variable_scope(self._name) as scope:
        # define placeholder
        self.X = tf.placeholder(tf.float32, [None, self._nodes[0]])    # input data placeholder
        self.learning_rate = tf.placeholder(tf.float32)    # learning rate placeholder
        self.keep_prob = tf.placeholder(tf.float32)    # placeholder for dropout
    
        layer = self.X
    
        # define encode layer
        for i, node in enumerate(self._nodes[:-2]):
            layer = self._dense_layer(shape=self._nodes[i:i+2], inputs=layer,
                                     layer_name='encoder_' + str(i+1))
            layer = self._activation(layer)
            layer = tf.nn.dropout(layer, keep_prob=self.keep_prob)
        
        # define last encode layer
        layer = self._dense_layer(shape=self._nodes[len(self._nodes)-2:len(self._nodes)], inputs=layer,
                                 layer_name='encoder_' + str(len(self._nodes)-1))
    
        # get hidden layer feature
        self.hidden_feature = layer
    
        layer = self._activation(layer)
    
        # define decode layer
        for i, node in enumerate(self._nodes[::-1][:-2]):
            layer = self._dense_layer(shape=self._nodes[::-1][i:i+2], inputs=layer,
                                     layer_name='decoder_' + str(i+1))
            layer = self._activation(layer)
            layer = tf.nn.dropout(layer, keep_prob=self.keep_prob)
        
        # define last decode layer
        self.outputs = self._dense_layer(shape=self._nodes[::-1][len(self._nodes)-2:len(self._nodes)], 
                                        inputs=layer, 
                                        layer_name='decoder_' + str(len(self._nodes)-1))
    
    # define loss function and optimizer
    self.loss = self._loss_function(self.outputs, self.X)
    self.optimizer = self._optimizer(self.loss)
```

``_dense_layer``方法定义全连接层，其实现如下：

``` Python
def _dense_layer(self, shape, inputs, layer_name):
    with tf.variable_scope(layer_name) as scope:
        W = tf.get_variable(layer_name + '_W', shape=shape, 
                            initializer=tf.contrib.layers.xavier_initializer())
        b = tf.Variable(tf.random_normal([shape[-1]]))
    return tf.add(tf.matmul(inputs, W), b)
```

``_activation``方法定义激活函数，实现如下：

```Python
def _activation(self, inputs):
    return tf.nn.relu(inputs)
```

``_loss_function``方法定义损失函数，其具体实现如下：

``` Python
def _loss_function(self, predict, labels):
    return tf.reduce_mean(tf.square(predict - labels))
```

``_optimizer``方法定义优化器，实现如下：

``` Python
def _optimizer(self, loss):
    return tf.train.AdamOptimizer(learning_rate=self.learning_rate).minimize(loss)
```

考虑到以上方法均不需要从外部进行访问，因此全部定义为私有方法。

``train``方法定义模型训练过程，其具体实现如下：

``` Python
def train(self, x_data, epochs=100, batch_size=128, learning_rate=0.001, keep_prob=0.7):
    losses = []
    for epoch in range(epochs):
        avg_loss = 0
        total_batch = int(x_data.num_examples / batch_size)
        for i in range(total_batch):
            batch_xs, _ = x_data.next_batch(batch_size)
            l, _ = self._sess.run([self.loss, self.optimizer],
                                feed_dict={self.X: batch_xs, 
                                           self.learning_rate: learning_rate,
                                           self.keep_prob: keep_prob})
            avg_loss += l / total_batch
        print("Epoch ", "%04d" %(epoch+1), "loss = ", "{:.9f}".format(avg_loss))
        
        losses.append(avg_loss)
    return losses 
```

``predict``方法定义模型的预测实现如下：

``` Python
def predict(self, x_data, keep_prob=1.0):
    return self._sess.run(self.outputs, 
                        feed_dict={self.X: x_data,
                                   self.keep_prob:keep_prob})
```

``get_feature``方法用于获取隐层的特征：

```Python
def get_feature(self, x_data, keep_prob=1.0):
    return self._sess.run(self.hidden_feature, 
                        feed_dict={self.X: x_data,
                                   self.keep_prob: keep_prob})
```

``save``方法用于保存训练好的模型：

```Python
def save(self, path):
    saver = tf.train.Saver()
    saver.save(self.sess, path)
```

``load``方法用于加载保存的模型：

```Python
def load(self, path):
    saver = tf.train.Saver()
    saver.restore(self.sess, path)
```

以上就是 AutoEncoder 类的全部实现，完整代码请移步[GitHub](https://github.com/floperry/Neural-Network)。
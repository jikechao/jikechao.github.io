---
layout:     post
title:      "pretrained model"
subtitle:   " \"框架自带的预训练模型使用\""
date:       2020-07-30 15：17
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - DL
    - tensorflow
    - mxnet
    - keras
---

# 一、模型导入

## 1、Keras内置model导入

```python
# keras中内置的部分model：
# [DenseNet121, InceptionResNetV2, MobileNet, InceptionV3, ResNet101, VGG16, Xception]

from tensorflow.keras.applications.mobilenet import MobileNet
model = MobileNet(weights='imagenet')
print(model.summary())
print(model.input, model.output)
```

## 2、keras导入h5 model

```python
model_path = 'mnist.h5'
predict_model = keras.models.load_model(model_path)
predict_model.summary()
```



## 3、MXnet 内置model导入

```python
import mxnet as mx
from mxnet import gluon
from mxnet.gluon.model_zoo.vision import get_model

def block2symbol(block):
    data = mx.sym.Variable('data')
    sym = block(data)
    args = {}
    auxs = {}
    for k, v in block.collect_params().items():
        args[k] = mx.nd.array(v.data().asnumpy())
        auxs[k] = mx.nd.array(v.data().asnumpy())
    return sym, args, auxs

block = get_model('resnet18_v1', pretrained=True)  
print(block.output)  # model的输出层信息

mx_sym, args, auxs = block2symbol(block)

# usually we would save/load it as checkpoint
# mx.model.save_checkpoint('resnet18_v1', 0, mx_sym, args, auxs)  
# mx_sym, args, auxs = mx.model.load_checkpoint('resnet18_v1', 0)

mod = mx.mod.Module(symbol=mx_sym, context=mx.cpu(), data_names=['data'], label_names=[])

# x为输入图片x.shape(n, 3, 224, 224)
mod.bind(for_training=False, data_shapes=[("data", x.shape)])
mod.set_params(args,auxs)

eval_iter = mx.io.NDArrayIter( x,shuffle=False)  
res = mod.predict(eval_iter, num_batch=len(x))
top1 = np.argmax(res.asnumpy())
```

## 4、tensorflow导入pb model

```python
pb = './pb/mnist.pb'
with tf.gfile.FastGFile(pb, 'rb') as f:
    graph_def = tf.GraphDef()
    graph_def.ParseFromString(f.read())
    graph = tf.import_graph_def(graph_def, name='')  # 将graph导入到tf中，session可感知
    
#  以下code，需要回退一个tab，注意！！！
    with tf.Session() as sess:
        sess.run(tf.global_variables_initializer())
        # 定义输入的张量名称,对应网络结构的输入张量
        # input:0作为输入图像,keep_prob:0作为dropout的参数,测试时值为1,is_training:0训练参数
        input_image_tensor = sess.graph.get_tensor_by_name("input:0")
        input_label_tensor = sess.graph.get_tensor_by_name("label:0")
        # 定义输出的张量名称
        output_tensor_name = sess.graph.get_tensor_by_name("softmax:0")
        output_accuracy = sess.graph.get_tensor_by_name("accuracy:0")
        output_res = sess.graph.get_tensor_by_name("res:0")

        # 读取测试图片
        im = pic
        # im = im[np.newaxis, :]
        out, accuracy ,res= sess.run
        	([output_tensor_name, output_accuracy,output_res], 
         	feed_dict=input_image_tensor:im,input_label_tensor:label})

        top1_ = tf.nn.top_k(out, 1)
        top1 = sess.run(top1_)
```

# 二、数据加载

## 1、 加载单张图片

```python
pic_path = '../data/cat.png'
img = Image.open(pic_path).resize((224,224))

# 使用keras内置的，图片处理函数
img = np.asarray(img)
img = img[np.newaxis,:]
process_input = tf.keras.applications.mobilenet.preprocess_input
x = process_input(img)

# 手动编写图片预处理
def transform_image(image):
    image = np.array(image) - np.array([123., 117., 104.])
    image /= np.array([58.395, 57.12, 57.375])
    image = image.transpose((2, 0, 1))
    image = image[np.newaxis, :]
    return image
```

## 2、加载npz批量图片

```python
def loadMnistDataset():
    data_path = "mnist.npz"
    data = np.load(data_path)
    # print(data.files)  # 获取data的所有key（如：x_test）
    x, y = data['x_test'], data['y_test']
    x=x[:10]  # 选取前10张图片
    x = x.astype('float32') / 255.0
    x = x.reshape(x.shape[0], 28, 28, 1)
    return x, y
```

# 三、格式转化

## 1、ckpt转化为pb

```python
import tensorflow as tf
from tensorflow.python.framework import graph_util
from tensorflow.python.platform import gfile
from tensorflow.examples.tutorials.mnist import input_data
import numpy as np

mnist = input_data.read_data_sets("MNIST_data", one_hot=True)

def freeze_graph(ckpt, output_graph):
    output_node_names = 'accuracy'
    saver = tf.train.import_meta_graph(ckpt + '.meta', clear_devices=True)
    graph = tf.get_default_graph()
    input_graph_def = graph.as_graph_def()

    with tf.Session() as sess:
        saver.restore(sess, ckpt)
        output_graph_def = graph_util.convert_variables_to_constants(
            sess=sess,
            input_graph_def=input_graph_def,
            output_node_names=output_node_names.split(',')
        )
        with tf.gfile.GFile(output_graph, 'wb') as fw:
            fw.write(output_graph_def.SerializeToString())
        print('{} ops in the final graph.'.format(len(output_graph_def.node)))


if __name__ == '__main__':
    ckpt = './model-temp/mnist_demo.ckpt'
    pb = './pb/mnist.pb'
    freeze_graph(ckpt,pb)
```

## 2、CKPT保存&恢复

```python
# 保存为ckpt
saver = tf.train.Saver()

with tf.Session() as sess:
    sess.run(init)
    for epoch in range(40):
        for batch in range(n_batch):
            batch_xs, batch_ys = mnist.train.next_batch(batch_size)
            sess.run(train_step, feed_dict={x:batch_xs, y:batch_ys})

        acc, top1, correct_prediction = \
            sess.run([accuracy, top1_,correct_prediction_], feed_dict={x:mnist.test.images, y: mnist.test.labels})
        print("Iter "+ str(epoch) + ", Testing Accuracy" + str(acc))

    saver.save(sess, "./model-temp/mnist_demo.ckpt")

# 6666666666666666我是分割线66666666666666666666
# 导入ckpt
saver = tf.train.Saver()
with tf.Session() as sess:
    sess.run(init)
    saver = tf.train.import_meta_graph('./model-temp/mnist_demo.ckpt.meta')
    saver.restore(sess, tf.train.latest_checkpoint('./model-temp'))
```



## 3、torch 转换为onnx

```python
import torch
from models.MobileNetV2 import mobilenet_v2 as mobilenetv2

model = mobilenetv2(pretrained=True)
example = torch.rand(1, 3, 224, 224)   # 假想输入

torch_out = torch.onnx.export(model,
                              example,''
                              "mobilenetv2.onnx",
                              verbose=True,
                              export_params=True   # 带参数输出
                              )
```


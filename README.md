# Auto Pilot

##I followed Sully chen project for learning purpose.And modify it to run in colab.

## 1.Dataset: 

Two types of file in dataset. one file contains **Images** and another **data.txt**.

[Dataset link](https://drive.google.com/file/d/0B-KJCaaF7elleG1RbzVPZWV4Tlk/view)

Images is the visualization of road.

And data.txt contains the image_name/path and angle.

## 2.Task:

Task is to predict the angle according to the image. 

## 3.Exploratory Data analysis: 

First run EDA.ipynb [click hee](https://github.com/hiddenntreasure/Self-Driving-object/blob/master/EDA.ipynb)

First load the **data.txt**. From data.txt split the images_name/path and angle. Where,

X = path and y = angle

Then split the X,y data into 80/20 for train and test purpose.

**Plotting:** plot histogram of train and test data.

**Note:** Normalization should be done on train/test data. Otherwise,plotting will be dense.

## 4.Modeling:

### Simple model:

Take the mean value of train as y^= train_mean_y (y^ is predicted value)

MSE(Mean Squarred Error) : Test MSE(MEAN) = (test_y-train_mean_y)

                                            = 0.191127

Even if predicted y^ = 0 for all test case. Then, Test MSE(zero) = (test_y-train_mean_y) 
                                                                 
                                                            = 0.190891

## 5.Driving_data.py:

Then Run Load_batch_wise.ipynb file [click here](https://github.com/hiddenntreasure/Self-Driving-object/blob/master/Load%20batch%20wise.ipynb)

- It also split the data.txt dataset into train and test dataset
- Then LoadTrainBatch,LoadTestBatch is used to load batch data. 
  - imread function is used to read the image from path. example : scipy.misc.imread('path')[-150:]
  
    Note : here [-150:] is representing width.which start -150.and we know that -1 is the last element.so,-150 will be the 150th last element.
  - imresize function is used to reshape the image.
  - train_batch_pointer/val_batch_pointer is to track the last loaded image path. So,it declared as global variable.

## 6. End to End Modeling:

Then run Model.py [For model details : end to end Deep Learning](https://devblogs.nvidia.com/deep-learning-self-driving-cars/)

## 9. Training our dataset:

```python
sess = tf.InteractiveSession()

L2NormConst = 0.001

train_vars = tf.trainable_variables()

loss = tf.reduce_mean(tf.square(tf.subtract(model.y_, model.y))) + tf.add_n([tf.nn.l2_loss(v) for v in train_vars]) * L2NormConst
train_step = tf.train.AdamOptimizer(1e-4).minimize(loss)
sess.run(tf.initialize_all_variables())

epochs = 30
batch_size = 100

for epoch in range(epochs):

  for i in range(int(driving_data.num_images/batch_size)):
    xs, ys = driving_data.LoadTrainBatch(batch_size)
    train_step.run(feed_dict={model.x: xs, model.y_: ys, model.keep_prob: 0.8})
    
    if i % 10 == 0:
      xs, ys = driving_data.LoadValBatch(batch_size)
      loss_value = loss.eval(feed_dict={model.x:xs, model.y_: ys, model.keep_prob: 1.0})
      
```

Uses 30 epochs and 100 batch_size to train. And after 10 step in a epoch evaluate validation data.

## 8. Test Dataset:

Run Test Dataset.ipynb

Pick test images one after another. Predict angle and rotate the steering wheel according to the angle.

It will give output like this:
youtube : https://www.youtube.com/watch?v=RS5HhfdvilE&list=PL81jj-odGf6Ohtqq1ujnOgykdshrFl4Ta&index=2&t=0s
![](ezgif.com-video-to-gif.gif)



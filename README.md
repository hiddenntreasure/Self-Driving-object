# Self-Driving-object

## 1.Dataset: 

Two types of file in dataset. one file contains **Images** and another **data.txt**.

Images is the visualization of road in pixels.

And data.txt contains the image_name/path and angle.

## 2.Task:

Task is to predict the angle according to the image. 

## 3.Exploratory Data analysis: 

to get Python file [click hee](https://github.com/hiddenntreasure/Self-Driving-object/blob/master/EDA.ipynb)

First load the **data.txt**. From data.txt split the images_name/path and angle. Where,

X = path and y = angle

Then split the X,y data into 80/20 for train and test purpose.

**Plotting:** plot histogram of train and test data. **Note:** Normalization should be done on train/test data. Otherwise,plotting will be dense.

## 4.Modeling:

### Simple model:

Take the mean value of train as y^= train_mean_y (y^ is predicted value)

MSE(Mean Squarred Error) : Test MSE(MEAN) = (test_y-train_mean_y)

                                          = 0.191127

Even if predicted y^ = 0 for all test case. Then, Test MSE(zero) = (test_y-train_mean_y) 
                                                                 
                                                                 = 0.190891

## 5.Driving_data.py:

To get driving_data.py file [click here](https://github.com/hiddenntreasure/Self-Driving-object/blob/master/driving_data_py.ipynb)

- It also split the data.txt dataset into two train and test
- Then LoadTrainBatch,LoadTestBatch is used to load batch data. 
  - imread function is used to read the image from path. example : scipy.misc.imread('path')[-150:]
  
    Note : here [-150:] is representing width.which start -150.and we know that -1 is the last element.so,-150 will be the 150th last element.
  - imresize function is used to reshape the image.
  - train_batch_pointer/val_batch_pointer is to track the last loaded image path. So,it declared as global variable.

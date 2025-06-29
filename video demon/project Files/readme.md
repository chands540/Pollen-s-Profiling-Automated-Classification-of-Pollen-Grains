# Data Exploration


import os
from collections import Counter
import matplotlib.pyplot as plt
from PIL import Image
import numpy as np
import tensorflow as tf
import cv2

#give the path of your dataset file that you downloaded
path =  'C:/Users/Akshay V/Downloads/archive1'
names = [name.replace(' ', '_').split('_')[0] for name in os.listdir(path)] #list of all first names from list of all files
classes = Counter(names)  #returns dictionary with key as name, count as value

print("Total number of images is {}".format(len(names)))

#bar graph of entire dataset
plt.figure(figsize = (12,8))
plt.title('Class Counts in Dataset')
plt.bar(*zip(*classes.items())) 
plt.xticks(rotation='vertical')
plt.show()

print(classes)

path_class  = {key:[] for key in classes.keys()} #dict of class and path to come
for name in os.listdir(path):
    key = name.replace(' ', '_').split('_')[0] #assigning each key the path
    path_class[key].append(path + '/' + name) #adding the path into a list which is the value of all images of the species
print(path_class) #comment this to stop printing the dataset image path


#images in the dataset
fig = plt.figure(figsize=(15, 15))
for i, key in enumerate(path_class.keys()):
    img1 = Image.open(path_class[key][0]) #opens first three images of each class
    img2 = Image.open(path_class[key][1]) 
    img3 = Image.open(path_class[key][2]) 

    ax = fig.add_subplot(8, 9,  3*i + 1, xticks=[], yticks=[])
    ax.imshow(img1)
    ax.set_title(key)
    
    ax = fig.add_subplot(8, 9,  3*i + 2, xticks=[], yticks=[])
    ax.imshow(img2)
    ax.set_title(key)

    ax = fig.add_subplot(8, 9,  3*i + 3, xticks=[], yticks=[])
    ax.imshow(img3)
    ax.set_title(key)
plt.show()

#choose a class for classifying
list_of_pollens=['anadenanthera', 'urochloa', 'arrabidaea', 'cecropia',
                 'chromolaena', 'combretum', 'croton', 'dipteryx',
                 'eucalipto', 'qualea', 'hyptis', 'mabea',
                 'matayba', 'mimosa', 'myrcia', 'protium',
                 'faramea', 'schinus', 'senegalia', 'serjania',
                 'syagrus', 'tridax', 'arecaceae']
print('\n\n\n\n\n\n\n\n\n\n',list_of_pollens)
image_import=input("Enter image name from the given list: ")
#Preprocessing`

def process_img(img, size = (128,128)):
    img = cv2.resize(img, size)  # resize image
    img = img/255                   # divide values by 255
    return img 

X, Y = [], []     #x list of processed images, y list of class of that image
for name  in os.listdir(path):   #apply resize to all images
    img = cv2.imread(path + '/' + name)
    X.append(process_img(img))
    Y.append(name.replace(' ', '_').split('_')[0])

X = np.array(X)

from sklearn.preprocessing import LabelEncoder
from tensorflow.keras.utils import to_categorical

le = LabelEncoder()
Y_le = le.fit_transform(Y)
Y_cat = to_categorical(Y_le, 23)

from sklearn.model_selection import train_test_split
X_train, X_test, Y_train, Y_test = train_test_split(X,Y_cat, test_size=0.285, stratify=Y_le)
print("Images in each class in Test set: {}".format(np.sum(Y_test, axis =0)))



#Model

#give the path of the cnn.hdf5 model that you downloaded
model=tf.keras.models.load_model("C:/Users/Akshay V/Downloads/cnn.hdf5")
model.summary()
score = model.evaluate(X_test, Y_test, verbose=0)
print('Test set accuracy: {}'.format(score[1]))



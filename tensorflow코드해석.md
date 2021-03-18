## **Tensorflow 코드를 자세히 해석합니다**

model.fit(x=scaled_train_samples, y=train_labels, batch_size=10, epchos=30, shuffle=True, verbose=2)

default로 shuffle=True로 설정되어있어 train data(짝을 이루는 데이터 scaled_train_samples 와 train_labels)는 무작위로 섞이게 하여 머신이 데이터 순서에 대한 학습을 하지 못하도록 설정합니다.

### **validation_split**

model.fit(x=scaled_train_samples, y=train_lables, **validation_split=0.1** batch_size=10, epchos=30, shuffle=True, verbose=2)

validation_split=0.1은 train data에서 마지막 10%를 가지고 validation을 확인하도록 합니다. 

이때 model.fit 함수에서 shuffle=True여도 shuffle이 되기 전 10%를 사용하므로 

***만약 validation_fit를 사용할 것이면 overfitting을 방지하기 위해 미리 train data를 shuffle하는 데이터 전처리 과정을 거치는 것이 좋습니다.***

accuracy는 모델의 정확도를 측정하고 val_accuarcy는 그 모델이 general하게 적용될 수 있는지 확인하는 척도입니다.

모델의 accuarcy는 높은데 val_accuracy가 낮다는 의미는 이 모델은 train data에 대해서만 모델이 너무 잘맞고 그 외에 데이터에는 잘 맞지 않는 overfitting이 일어난 것입니다.

***따라서 accuracy와 val_accuracy 사이의 차이를 비교함으로써 overfitting 현상이 일어났는지 확인해 볼 수 있습니다.***

**test_data는 validation이후 앞으로 기대되는 모델의 성능(prediction) 을 한번 더 확인하기 위해 사용합니다.**

---

### test data는 shuffle하지 않습니다.

test_batches = ImageDataGenerator(preprocessing_function=tf.keras.applications.mobilnenet.preprocess_input).flow_from_directory(directory=test_path, target_size=(224,224), batch_size=10, **shuffle=False**)

We set **shuffle=False** so that we can later appropriately plot our prediction results to a ***confusion matrix!!***

---

model.fit(x=train_batches, validation_data=valid_batches, epochs=10, verbose=2) 

We are not specifying y which is our target data usually, and that's because when data is stored as a generator as we have here, the generator itself acutally contains the corresponding labels, so we do not need to specify them seperately whenever we call fit, because that actually contains within the generator itself.

---

**Shape 활용 (shape은 메서드가 아니고 속성값입니다)**

What is the meaning of axis=-1 when a.shape = (19, 19, 5, 80)? And also what will be the output of keras.argmax(a, axis=-1) and keras.max(a, axis=-1)?

This means that the index that will be returned by argmax will be taken from the last axis.

Your data has some shape (19,19,5,80). This means:

Axis 0 = 19 elements

Axis 1 = 19 elements

Axis 2 = 5 elements

Axis 3 = 80 elements

Now, negative numbers work exactly like in python lists, in numpy arrays, etc. Negative numbers represent the inverse order:

Axis -1 = 80 elements

Axis -2 = 5 elements

Axis -3 = 19 elements

Axis -4 = 19 elements

When you pass the axis parameter to the argmax function, the indices returned will be based on this axis. Your results will lose this specific axes, but keep the others.

See what shape argmax will return for each index:

K.argmax(a,axis= 0 or -4) returns (19,5,80) with values from 0 to 18

K.argmax(a,axis= 1 or -3) returns (19,5,80) with values from 0 to 18

K.argmax(a,axis= 2 or -2) returns (19,19,80) with values from 0 to 4

K.argmax(a,axis= 3 or -1) returns (19,19,5) with values from 0 to 79

---

model.fit(x=train_batches, validation_data=valid_batches, epochs=5, **verbose=2**)

**verbose=2** : We can the most comprehensive output from the model during training

**verbose=0** : no output

---

### Fine tuning

x = mobile.layers[-6].output 마지막 5개의 layers(6 to last layers)를 제거합니다.

output = Dense(units=10, activation='softmax')(x) 

units=10 : this is going to be our output layers, 10 units due to nature of our classes(0~9)

activation='softmax' : to give us probability distributions among those 10 outputs

(x) : The mobilemodel is actually a functional model so this is from the functional API from keras, not the sequential API. SO we kind of touch on this a little bit earlier whenver we fine tuned VGG16, we saw that VGG16 was also indeed a functional model but when we fine tuned iterated it over each of the layers and added them to sequential model at that point because we weren't ready to introduce the functional model yet. So here we are going to continue working with a functional model type so that's why we are basically taking all of the layers here(x=mobile.layer[-6].output) up to the sitxth to last and whenever we create this output layer and then call this previous layers stored in X here that is the way that the functional model works. ****We're bascially saying to this output layer pass all of the previous layers that we have stroed in X up to the sitxh to last layer in mobile net.***





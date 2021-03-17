## **Tensorflow 코드를 자세히 해석합니다**

model.fit(x=scaled_train_samples, y=train_lables, batch_size=10, epchos=30, shuffle=True, verbose=2)

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

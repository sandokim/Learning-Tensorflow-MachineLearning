## **Tensorflow 코드를 자세히 해석합니다**

model.fit(x=scaled_train_samples, y=train_lables, batch_size=10, epchos=30, shuffle=True, verbose=2)

default로 shuffle=True로 설정되어있어 train data(짝을 이루는 데이터 scaled_train_samples 와 train_labels)는 무작위로 섞이게 하여 머신이 데이터 순서에 대한 학습을 하지 못하도록 설정합니다.

### **validation_split**

model.fit(x=scaled_train_samples, y=train_lables, **validation_split=0.1** batch_size=10, epchos=30, shuffle=True, verbose=2)

validation_split=0.1은 train data에서 마지막 10%를 가지고 validation을 확인하도록 합니다. 

이때 model.fit 함수에서 shuffle=True여도 shuffle이 되기 전 10%를 사용하므로 

***만약 validation_fit를 사용할 것이면 overfitting을 방지하기 위해 미리 train data를 shuffle하는 데이터 전처리 과정을 거치는 것이 좋습니다***

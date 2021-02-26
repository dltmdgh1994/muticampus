# AI 용어 정리

* AI(인공지능)

  인간이 가지는 특유의 학습, 추론 능력을 컴퓨터로 구현하려는 가장 포괄적인 개념

* Machine Learning

  AI를 구현하기 위한 하나의 방법. 데이터의 특성과 패턴을 학습 후, 미지의 데이터에 대한 추정치를 계산하는 프로그래밍 기법

* Deep Learning

  ML 중 Neural Network를 사용하는 기법

* Data Mining

  데이터의 상관관계를 분석하여 새로운 feature를 알아내는 방법



# Machine Learning

데이터를 학습해서 미지의 데이터에 대한 prediction

Explicit program(Rule based based programming)으로 해결할 수 없는 문제를 해결하기 위해 등장 => 경우의 수가 너무 많아 Rule 또한 너무 많아 해결 불가능( ex) 바둑 )

학습 방법에 따라 크게 4가지로 구분

* 지도학습(Supervised Learning) => Data Set에 Label 유 

  ​															우리가 해결하는 거의 대부분의 문제

  1. Regression

     훈련 데이터 셋을 이용해 학습하고 나온 predict model이 연속적인 값을 예측(얼마나??)

     ![regression](md-images/regression.PNG)

  2. Binary Classification

     predict model이 어떤 부류에 속하는지를 예측(어떤 것??)

     ![binary_classification](md-images/binary_classification.PNG)

  3. Multinomial Classification

     ![multinomial_classification](md-images/multinomial_classification.PNG)

* 비지도학습(Unsupervised Learning) => Data Set에 Label 무 

* 준지도학습(Semi-supervised Learning) =>  Data Set에 Label 유무가 혼재 

* 강화학습(Reinforcement Learning) => 별개의 방법



## 수치 미분(Numerical Differentation)

프로그램적으로 계산을 통해 [미분](함수에 대해 특정 순간의 변화량)을 수행(약간의 오차 발생)

보통 중앙차분을 이용

![numerical_differentation](md-images/numerical_differentation.PNG)

편미분 : 입력변수(독립변수)가 2개 이상인 다변수 함수에서 미분하고자하는 변수를 제외한 나머			   지 변수들을 상수처리해서 미분을 진행

* 일변수 함수에 대한 수치 미분

  ```python
  # 입력으로 들어오는 x에서 아주 미세하게 변화할때
  # 함수 f가 얼마나 변하는지에 대해 수치적으로 계산해보아요!
  
  # 아래의 함수는 인자를 2개 받아요.
  # 한개는 미분하려는 함수, 특정 점에서 미분값을 구하기 위한 x값.
  def numerical_derivative(f,x):
      
      # f : 미분하려는 함수
      # x : 미분값을 알고자하는 입력값.
      # delta_x => 극한에 해당하는 값으로 아주 작은값을 이용.
      #            1e-8이하로 지정하면 소수점 연산 오류가 발생.
      #            일반적으로 1e-5정도로 설정하면 되요
      delta_x = 1e-5
      
      return (f(x+delta_x) - f(x-delta_x)) / (2 * delta_x) # 중앙 차분
      
      
  # 미분하려는 함수가 있어야 해요! (일변수 함수)
  def my_func(x):
      
      return x ** 2     # f(x) = x^2
  
  result = numerical_derivative(my_func,3)
  
  print('미분한 결과값은 : {}'.format(result))  # 6.000000000039306
  ```

* 다변수 함수에 대한 수치 미분

  ```python
  ### 일반적으로 다변수 함수의 수치미분 코드를 이용합니다.
  
  # 입력변수가 2개 이상인 다변수 함수인 경우
  # 입력변수는 서로 독립이기 때문에 수치미분 역시 변수의 개수만큼 개별적으로
  # 진행해야 해요!
  
  import numpy as np
  
  def numerical_derivative(f,x):
      
      # f : 미분하려고 하는 다변수 함수
      # x : 모든 값을 포함하는 numpy array  ex) f'(1.0, 2.0) = (8.0, 15.0)
      delta_x = 1e-4
      derivative_x = np.zeros_like(x)    # [0 0]
      
      it = np.nditer(x, flags=['multi_index'])
      
      while not it.finished:
          
          idx = it.multi_index   # 현재의 iterator의 index를 추출 => tuple형태로 나와요     
          
          tmp = x[idx]     # 현재 index의 값을 잠시 보존.
                           # delta_x를 이용한 값으로 ndarray를 수정한 후 편미분을 계산
                           # 함수값을 계산한 후 원상복구를 해 줘야 다음 독립변수에
                           # 대한 편미분을 정상적으로 수행할 수 있어요!   
          x[idx] = tmp + delta_x        
          fx_plus_delta = f(x)    # f([1.00001, 2.0])   => f(x + delta_x)
          
  
          x[idx] = tmp - delta_x
          fx_minus_delta = f(x)   # f([0.99999, 2.0])   => f(x - delta_x)
          
          derivative_x[idx] = (fx_plus_delta - fx_minus_delta) / (2 * delta_x)
          
          x[idx] = tmp
          
          it.iternext()
          
      return derivative_x
  
  
  def my_func(input_data):
      
      x = input_data[0]
      y = input_data[1]
      return 2*x + 3*x*y + np.power(y,3)     # f(x) = 2x + 3xy + y^3
  
  
  param = np.array([1.0,2.0])
  result = numerical_derivative(my_func,param)
  print('미분한 결과는 : {}'.format(result)) # [8, 15.0000001]
  ```



## Tensorflow

```python
# Tensorflow는 1.x버전과 2.x버전으로 나뉘어져요!
# 1.x버전은 low level의 코딩이 필요!
# 2.x버전은 상위 API(Keras)가 기본으로 포함. => 구현이 쉬워요!

import tensorflow as tf
print(tf.__version__) # 1.15.0

node1 = tf.constant('Hello World')

# 그래프를 실행하려면 1.x버전에서는 session이 필요
# 1.x버전에서만 사용되요. 2.x버전에서는 삭제
# session은 그래프안의 특정 노드를 실행시킬 수 있어요!
sess = tf.Session()

print(sess.run(node1).decode()) # Hello World

print(node1)  # Tensor("Const:0", shape=(), dtype=string)

node1 = tf.constant(10, dtype=tf.float32)
node2 = tf.constant(30, dtype=tf.float32)

node3 = node1 + node2

print(sess.run([node3, node1])) # [40.0, 10.0]
```



![model_graph1](md-images/model_graph1.PNG)

```python
  import numpy as np
  import pandas as pd
  import tensorflow as tf
  
  # 1. training data set
  x_data = (np.array([1,2,3,4,5])).reshape(5,1)   # 공부시간 
  t_data = (np.array([3,5,7,9,11])).reshape(5,1)  # 시험성적
    
  # 2. placeholder => 데이터는 아직. 모형만
  X = tf.placeholder(shape=[None,1], dtype=tf.float32)
  T = tf.placeholder(shape=[None,1], dtype=tf.float32)
  
  # 3. Weight & bias   
  W = tf.Variable(tf.random.normal([1,1]), name='weight')
  b = tf.Variable(tf.random.normal([1]), name='bias')
  
  # 4. Hypothesis or predict model
  H = tf.matmul(X,W) + b   # y = Wx + b => 2차원행렬로 처리 
    					   # => y = X dot W + b
  
  # 5. W,b를 구하기 위해 평균제곱오차를 이용한 최소제곱법을 통해
  #    loss function을 정의
  loss = tf.reduce_mean(tf.square(H - T))
  
  # 6. train
  train = tf.train.GradientDescentOptimizer(learning_rate=1e-3).minimize(loss)
  
  # 7. session & 초기화
  sess = tf.Session()
  sess.run(tf.global_variables_initializer())   # 초기화 (2.x 넘어오면서 삭제)
  
  # 8. 학습을 진행
  # 반복학습을 진행 ( 1 epoch : training data set 전체를 이용하여 1번 학습)
  for step in range(30000):
      
      _, W_val, b_val, loss_val = sess.run([train,W,b,loss], 
                                           feed_dict={X : x_data, T : t_data})
      
      if step % 3000 == 0:
          print('W : {}, b : {}, loss : {}'.format(W_val, b_val, loss_val))
          
  # 9. 학습이 종료된 후 최적의 W와 b가 계산되고 이를 이용한 model이 완성
  #    prediction(예측)
  result = sess.run(H, feed_dict={X : [[9]]})
  print('예측값은 : {}'.format(result)) # 예측값은 : [[18.999674]]
```



## 데이터 전처리

머신러닝에서 학습이 잘 되기 위해서는 **양질의 데이터**가 필요 => **데이터 전처리**

1. **이상치 처리**

   이상치(outlier) : 관측된 값이 일반적인 값에 비해 편차가 큰 값을 지칭

   독립변수(feature)에 있는 이상치 => 지대점

   종속변수(label)에 있는 이상치 => 이상치

   이상치 검출 & 처리 방법

   1. Tukey Fences

      Box plot의 사분위를 이용해서 이상치를 graph에서 확인

      ![boxplot](md-images/boxplot.PNG)

      IQR value : 3사분위값 - 1사분위값

      이상치 : 1사분위값 - (IQR Value * 1.5) 미만의 값들

      ​			   3사분위값 + (IQR Value * 1.5) 초과의 값들

      ​			  이 경계들을 Tukey Fences라 부른다.

      ```python
      import numpy as np
      import matplotlib.pyplot as plt
      
      data = np.array([1,2,3,4,5,6,7,8,9,10,11,12,13,14,22.1])
      
      fig = plt.figure()   # 새로운 figure를 생성
      fig_1 = fig.add_subplot(1,2,1)
      fig_2 = fig.add_subplot(1,2,2)
      
      fig_1.set_title('Original Data')
      fig_1.boxplot(data)
      
      # numpy를 이용해서 사분위수를 구해보아요! percentile()
      print(np.median(data))         # 중위값(2사분위) => 8.0
      print(np.percentile(data,25))  # 1사분위 => 4.5
      print(np.percentile(data,50))  # 2사분위 => 8.0
      print(np.percentile(data,75))  # 3사분위 => 11.5
      
      # IQR value
      iqr_value = np.percentile(data,75) - np.percentile(data,25)
      print(iqr_value)  # 7.0
      
      upper_fense = np.percentile(data,75) + (iqr_value * 1.5)
      lower_fense = np.percentile(data,25) - (iqr_value * 1.5)
      
      print(upper_fense)  # 22.0
      print(lower_fense)  # -6.0
      
      ## 이상치를 출력해보아요!!
      print(data[(data > upper_fense) | (data < lower_fense)])  # [22.1]
      
      result = data[(data <= upper_fense) & (data >= lower_fense)]
      
      fig_2.set_title('Remove Outlier')
      fig_2.boxplot(result)
      
      fig.tight_layout()
      plt.show()
      ```

      ![boxplot_result](md-images/boxplot_result.PNG)

      

      

   2. Z-score

      정규분포와 표준편차를 이용해서 이상치를 확인

      ![normal_distribution](md-images/normal_distribution.PNG)

      ```python
      import numpy as np
      from scipy import stats
      
      zscore_threshold = 1.8 # (2.0이 optimal value) => 상위 95% 이상, 하위 95% 이하인 값
      
      data = np.array([1,2,3,4,5,6,7,8,9,10,11,12,13,14,22.1])
      
      # outlier 출력
      outlier = data[np.abs(stats.zscore(data)) > zscore_threshold]
      
      # 이상치를 제거한 데이터
      data[np.isin(data,outlier, invert=True)]
      

      # 또 다른 방법
      for col in data.columns:
          tmp = ~(np.abs(stats.zscore(data[col])) > zscore_threshold)
          data = data.loc[tmp]
      ```
      
      

2. **정규화(Normalization)**

   비율을 이용해서 data의 scale을 조정

   1. Min-Max 정규화

      data의 scale을 최소값 0 ~ 최대값 1 사이로 조정

      이상치에 상당히 민감한 방식이기 때문에 이상치 처리를 미리 해야 한다.

      ![min_max_normalization](md-images/min_max_normalization.PNG)

      

   2. Z-score 정규화(Strandardization)

      data의 scale을 평균과 표준편차를 이용해 조정

      이상치에 크게 영향을 받지 않지만, 동일한 scale을 적용할 수 없다.

      ![z-score_normalization](md-images/z-score_normalization.PNG)

      

## Regression

Regression Model(회귀 모델)은 어떠한 데이터에 대해서 그 값에 영향을 주는 조건을 고려하여 데이터의 평균을 구하기 위한 함수. 즉, 그 데이터를 가장 잘 표현하는 함수.

* Model을 만드는 이유?

  우리가 해결해야 하는 현실은 너무 복잡 => 단순화. 따라서, 가정이 필요

  * 오차항은 정규분포를 따른다.

  * 독립변수와 종속변수가 선형관계

  * 데이터에 이상치가 없다.

  * 독립변수와 오차항은 독립 등등

    

* Classical Linear Regression Model

  ![classical_linear_regression_model](md-images/classical_linear_regression_model.PNG)

  * MSE

    ![MSE](md-images/MSE.PNG)

    

  * 손실 함수(Loss Function) = 비용 함수(Cost Function)

    훈련 데이터 셋의 정답 t와 입력 x에 대한 y(모델의 예측값)의 차이를 모두 더해 수식으로 나타낸 식 => MSE를 이용

    최소제곱법을 이용해서 loss function을 만들고 그 값이 최소가 되게 하는 w와 b를 학습 과정을 통해 찾는다.

    ![loss_function](md-images/loss_function.PNG)

    

  * 경사하강법(Gradient Descent Algorithm)

    loss function의 값이 최소가 되게 하는 w를 찾기 위한 방법으로,  loss function의 미분값이 0이 되는 w를 찾기 위해 w를 조금씩 줄여가면서 찾는다.

    ![gradient_descent_algorithm](md-images/gradient_descent_algorithm.PNG)

    여기서, epoch와 learning late를 통해 접근을 조절

  
1. data 전처리

```python
  import numpy as np
  import pandas as pd
  from sklearn.preprocessing import MinMaxScaler
  from scipy import stats
  
  # 1. csv 파일 로딩
  df = pd.read_csv('./ozone.csv')
  train_data = df[['Temp','Ozone']]
  
  # 2. 결측치 제거
  train_data = train_data.dropna(how='any')
  
  # 3. 이상치 제거
  zscore_threshold = 1.8
  
  for col in train_data.columns:
      tmp = ~(np.abs(stats.zscore(train_data[col])) > zscore_threshold)
      train_data = train_data.loc[tmp]
  
  x_data = train_data['Temp'].values.reshape(-1,1)
  t_data = train_data['Ozone'].values.reshape(-1,1)
  
  # 4. 정규화
  scaler_x = MinMaxScaler()  # 객체 생성
  scaler_t = MinMaxScaler()  # 객체 생성
  
scaler_x.fit(x_data)
  scaler_t.fit(t_data)

  scaled_x_data = scaler_x.transform(x_data)
  scaled_t_data = scaler_t.transform(t_data)
```

  

  1. Tensorflow를 이용해서 Simple Linear Regression을 구현

  ```python
  import tensorflow as tf  
      
  # 1. placeholder => 데이터는 아직. 모형만
  X = tf.placeholder(shape=[None,1], dtype=tf.float32)
  T = tf.placeholder(shape=[None,1], dtype=tf.float32)
    
  # 2. Weight & bias   
  W = tf.Variable(tf.random.normal([1,1]), name='weight')
  b = tf.Variable(tf.random.normal([1]), name='bias')
    
  # 3. Hypothesis or predict model
  H = tf.matmul(X,W) + b
    
  # 4.loss function
  loss = tf.reduce_mean(tf.square(H - T))
    
  # 5. train
  train = tf.train.GradientDescentOptimizer(learning_rate=1e-3).minimize(loss)
    
  # 6. session & 초기화
  sess = tf.Session()
  sess.run(tf.global_variables_initializer())
    
  # 7. 학습을 진행
  for step in range(600000):
      
      _, W_val, b_val, loss_val = sess.run([train, W, b, loss], 
                                           feed_dict={X: scaled_x_data, T: scaled_t_data})
      if step % 30000 == 0:
          print('W : {}, b : {}, loss : {}'.format(W_val, b_val, loss_val))
            
  # 8. 예측
  predict_data = np.array([[80]])
  scaled_predict_data = scaler_x.transform(predict_data)
  print("tensorflow 예측 결과 : {}".format(scaler_t.inverse_transform(sess.run(H, feed_dict={X: scaled_predict_data})))) # tensorflow 예측 결과 : [[42.08464]]
  
  ```



2. python를 이용해서 Simple Linear Regression을 구현

``` python
  # 1. Weight & bias
  W1 = np.random.rand(1,1)   
  b1 = np.random.rand(1)
  
  # 2. Hypothesis
  def predict(x):
      
      y = np.dot(x,W1) + b1   
      
      return y
  
  # 3. loss_function
  def loss_func(input_obj):
      
      input_W = input_obj[0]
      input_b = input_obj[1]
      
      y = np.dot(scaled_x_data,input_W) + input_b
      
      return np.mean(np.power((scaled_t_data - y),2))
  
  # 4. 편미분을 위한 함수
  def numerical_derivative(f,x):
      
      delta_x = 1e-4
      derivative_x = np.zeros_like(x)    # [0 0]
      
      it = np.nditer(x, flags=['multi_index'])
      
      while not it.finished:
          
          idx = it.multi_index        
          tmp = x[idx] 
          x[idx] = tmp + delta_x        
          fx_plus_delta = f(x)    # f([1.00001, 2.0])   => f(x + delta_x)
          
          x[idx] = tmp - delta_x
          fx_minus_delta = f(x)   # f([0.99999, 2.0])   => f(x - delta_x)
          
          derivative_x[idx] = (fx_plus_delta - fx_minus_delta) / (2 * delta_x)
          
          x[idx] = tmp
          
          it.iternext()
          
      return derivative_x
  
  # learning rate 설정
  learning_rate = 1e-4
  
  # 5. 학습을 진행
  for step in range(600000):
      input_param = np.concatenate((W1.ravel(), b1.ravel()), axis=0)  
      
      derivative_result = learning_rate * numerical_derivative(loss_func,input_param)
      
      W1 = W1 - derivative_result[:1].reshape(1,1)  # W 갱신
      b1 = b1 - derivative_result[1:]               # b 갱신
  
      if step % 30000 == 0:
          print('W : {}, b : {}'.format(W1,b1))
          
  # 6. 예측
  predict_data = np.array([[80]])
  scaled_predict_data = scaler_x.transform(predict_data)
  print("python 예측 결과 : {}".format(scaler_t.inverse_transform(predict(scaled_predict_data))))
  # python 예측 결과 : [[42.06771086]]
```

  

  3. sklearn을 이용해서 Simple Linear Regression을 구현
  
     sklearn은 자동으로 data를 정규화하기 때문에 사전에 이상치 제거만 하면 된다.

  ```python
  from sklearn import linear_model
  
  # 1. linear regression model 생성
  model = linear_model.LinearRegression()
  
# 2. 학습진행
  model.fit(x_data, t_data)

  # 3. Weight, bias 확인
print('W : {}, b : {}'.format(model.coef_, model.intercept_))
  
  # 4. 예측
  predict_data = np.array([[80]])
  print("sklearn 예측 결과 : {}".format(model.predict(predict_data)))
  # sklearn 예측 결과 : [[42.07085704]]
  ```




* Multiple Linear Regression Model

  ![multiple_linear_regression_model](md-images/multiple_linear_regression_model.PNG)

  

  1. data 전처리

  ```python
  # Multiple Linear Regression
  import numpy as np
  import pandas as pd
  import matplotlib.pyplot as plt
  import tensorflow as tf
  from sklearn import linear_model
  from sklearn.preprocessing import MinMaxScaler
  from scipy import stats
  
  df = pd.read_csv('./ozone.csv')
  
  # 학습에 필요한 데이터부터 추출
  train_data = df[['Temp','Wind','Solar.R','Ozone']]
  
  # 결측치 처리
  train_data = train_data.dropna(how='any')
  
  # 이상치 처리
  zscore_threshold = 1.8
  
  for col in train_data.columns:
      tmp = ~(np.abs(stats.zscore(train_data[col])) > zscore_threshold)
      train_data = train_data.loc[tmp]
      
  x_data = train_data[['Temp','Solar.R','Wind']].values
  t_data = train_data['Ozone'].values.reshape(-1,1)
  
  # 정규화 처리
  scaler_x = MinMaxScaler()  # 객체 생성
  scaler_t = MinMaxScaler()  # 객체 생성
  
  scaler_x.fit(train_data[['Temp','Solar.R','Wind']].values)
  scaler_t.fit(train_data['Ozone'].values.reshape(-1,1))
  
  scaled_x_data = scaler_x.transform(train_data[['Temp','Solar.R','Wind']].values)
  scaled_t_data = scaler_t.transform(train_data['Ozone'].values.reshape(-1,1))
  ```

  

  2. tensorflow을 이용해서 Simple Linear Regression을 구현

  ```python
  X = tf.placeholder(shape=[None,3], dtype=tf.float32)
  T = tf.placeholder(shape=[None,1], dtype=tf.float32)
  
  W = tf.Variable(tf.random.normal([3,1]), name='weight')
  b = tf.Variable(tf.random.normal([1]), name='bias')
  
  H = tf.matmul(X,W) + b
  
  loss = tf.reduce_mean(tf.square(H - T))
  
  train = tf.train.GradientDescentOptimizer(learning_rate=1e-4).minimize(loss)
  
  sess = tf.Session()
  sess.run(tf.global_variables_initializer())
  
  for step in range(600000):
      
      _, W_val, b_val, loss_val = sess.run([train, W, b, loss], 
                                           feed_dict={X: scaled_x_data, T: scaled_t_data})
      if step % 30000 == 0:
          print('W : {}, b : {}, loss : {}'.format(W_val, b_val, loss_val))
  
  predict_data = np.array([[80.0, 150.0, 10.0]])
  scaled_predict_data = scaler_x.transform(predict_data)
  
  print("tensorflow 예측 결과 : {}".format(scaler_t.inverse_transform(sess.run(H, feed_dict={X: scaled_predict_data}))))
  # tensorflow 예측 결과 : [[38.7607]]
  
  ```

  

  3. sklearn을 이용해서 Simple Linear Regression을 구현

     sklearn은 자동으로 data를 정규화하기 때문에 사전에 이상치 제거만 하면 된다.

  ```python
  model2 = linear_model.LinearRegression()
  
  model2.fit(x_data,t_data)
  
  print('W: {}, b: {}'.format(model2.coef_, model2.intercept_))
  
  print("sklearn 예측 결과 : {}".format(model2.predict(predict_data)))
  # sklearn 예측 결과 : [[38.8035437]]
  ```

  







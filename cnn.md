# CNN

일반신경망

- 이미지 전체를 하나의 데이터로 입력 처리
- 이미지 위치가 달라지거나 왜곡된 경우에는 올바르게 작동불가







합성곱신경망

- 필터를 사용하여 이미지의 특징을 추출
- 위치가 아닌 특성이 있다 없다로 학습



cnn에서는 가중치의 집합을 필터라고 한다능



가중치 업데이트...

### Optimizer

- Momentum

- AdaGrad 

- 위 두개를 합치면 Adam

 

자연어 처리 시 RNN 꼭 필요



---

# tensorflow

placeholder - 공간확보



오류를 구하는 것 loss function



### X 와 Y 의 상관관계를 분석하는 기초적인 선형 회귀 모델을 만들고 실행

```python

import tensorflow as tf

x_data = [1, 2, 3]
y_data = [1, 2, 3]

W = tf.Variable(tf.random_uniform([1], -1.0, 1.0)) # 변수 // 웨이트값이랑 바이어스를 균등분포를 통해 랜덤값으로 바꿔 시작..?
b = tf.Variable(tf.random_uniform([1], -1.0, 1.0))

# name: 나중에 텐서보드등으로 값의 변화를 추적하거나 살펴보기 쉽게 하기 위해 이름을 붙여줍니다.
X = tf.placeholder(tf.float32, name="X") # 비어있던 공간에 데이터를 할당
Y = tf.placeholder(tf.float32, name="Y")
print(X)
print(Y) # 아직 세션으로 실행한게 아니라서 얘는 tensor입니다~ 라고만 뜬다
```



```python
# X 와 Y 의 상관 관계를 분석하기 위한 가설 수식을 작성합니다.
# y = W * x + b
# W 와 X 가 행렬이 아니므로 tf.matmul(행렬곱) 이 아니라 기본 곱셈 기호를 사용했습니다.
hypothesis = W * X + b

# 손실 함수를 작성합니다.
# mean(h - Y)^2 : 예측값과 실제값의 거리를 비용(손실) 함수로 정합니다. (손실함수가 줄어드는게 목표)
cost = tf.reduce_mean(tf.square(hypothesis - Y))
# 텐서플로우에 기본적으로 포함되어 있는 함수를 이용해 경사 하강법 최적화를 수행합니다.
optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.1) #경우에 따라 학습률(얼마나 반영할 것인지)조절 가능. 이 경우는 10%씩 반영하겠다는 의미
# 옵티마이저관련 패키지는 train안에 있다넹
# 비용을 최소화 하는 것이 최종 목표
train_op = optimizer.minimize(cost)

# 세션을 생성하고 초기화합니다.
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())

    # 최적화를 100번 수행합니다.
    for step in range(100):
        # sess.run 을 통해 train_op 와 cost 그래프를 계산합니다.
        # 이 때, 가설 수식에 넣어야 할 실제값을 feed_dict 을 통해 전달합니다.
        _, cost_val = sess.run([train_op, cost], feed_dict={X: x_data, Y: y_data})
        #반환값의 두번째만 받기 위해 앞의 것은 안받겠다는 의미 (_,)

        print(step, cost_val, sess.run(W), sess.run(b)) # cost_val(오류값) : 적을수록 좋다

    # 최적화가 완료된 모델에 테스트 값을 넣고 결과가 잘 나오는지 확인해봅니다.
    print("\n=== Test ===")
    print("X: 5, Y:", sess.run(hypothesis, feed_dict={X: 5}))
    print("X: 2.5, Y:", sess.run(hypothesis, feed_dict={X: 2.5}))
```




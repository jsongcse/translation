# 전진과 후진 (Forward and Backward)

전진과 후진은 신경망 계산의 필수 요소입니다.
(The forward and backward passes are the essential computations of a Net.)

<img src="fig/forward_backward.png" alt="Forward and Backward" width="480" />

단순한 로지스틱 회귀 분류기를 생각해 보겠습니다.
(Let’s consider a simple logistic regression classifier.)

전진 과정(forward pass)은 예측에 필요한 입력이 주어졌을 때에 출력을 계산합니다. 전진 과정에서 카페는 모델이 표현하는 "함수(function)"를 계산하기 위해 각 레이어의 계산을 합성합니다. 이 과정은 아래에서 위로 갑니다.
(The forward pass computes the output given the input for inference. In forward Caffe composes the computation of each layer to compute the "function" represented by the model. This pass goes from bottom to top.)

<img src="fig/forward.jpg" alt="Forward pass" width="320" />

데이터 x가 g(x)를 위해 벡터 내적 레이어를 통과하고 이어 h(g(x))를 위해 소프트맥스를 통과하며, f_W(x)를 위해 소프트맥스 손실을 통과합니다.
(The data x is passed through an inner product layer for g(x) then through a softmax for h(g(x)) and softmax loss to give f_W(x).)

The backward pass computes the gradient given the loss for learning. In backward Caffe reverse-composes the gradient of each layer to compute the gradient of the whole model by automatic differentiation. This is back-propagation. This pass goes from top to bottom.

<img src="fig/backward.jpg" alt="Backward pass" width="320" />

The backward pass begins with the loss and computes the gradient with respect to the output ∂fW∂h∂fW∂h. The gradient with respect to the rest of the model is computed layer-by-layer through the chain rule. Layers with parameters, like the INNER_PRODUCT layer, compute the gradient with respect to their parameters ∂fW∂Wip∂fW∂Wip during the backward step.

These computations follow immediately from defining the model: Caffe plans and carries out the forward and backward passes for you.

The Net::Forward() and Net::Backward() methods carry out the respective passes while Layer::Forward() and Layer::Backward() compute each step.
Every layer type has forward_{cpu,gpu}() and backward_{cpu,gpu}() methods to compute its steps according to the mode of computation. A layer may only implement CPU or GPU mode due to constraints or convenience.
The Solver optimizes a model by first calling forward to yield the output and loss, then calling backward to generate the gradient of the model, and then incorporating the gradient into a weight update that attempts to minimize the loss. Division of labor between the Solver, Net, and Layer keep Caffe modular and open to development.

For the details of the forward and backward steps of Caffe’s layer types, refer to the layer catalogue.
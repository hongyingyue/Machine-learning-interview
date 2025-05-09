# 逻辑回归

## 1. 最小经验损失函数

在二分类中，y_hat的含义是预测类别为1的概率为y_hat，相应的为0的概率为(1-y_hat)

最小化cross entropy等价于最大化log softmax probability

$$
y = \frac{1}{1+e^{-(w^{T} x + b)}}
$$

sigmoid function

$$
g(z) = \frac{1}{1+e^{-z}} \space\space\space\space 0 < g(z) <1
$$

sigmoid函数的导数为: `sigmoid(x) * (1 - sigmoid(x))`

### 对数损失函数 / Logistic loss function

**Log loss is convex (注意负号)** 

$$
\mathcal{L}(f(x), y) =
\begin{cases}
-\log(f(x)) & \text{if } y = 1 \\
-\log(1 - f(x)) & \text{if } y = 0
\end{cases}
$$

**Simplified loss function**

$$
\mathcal{L}(f(x), y) = - \left[ y \cdot \log(f(x)) + (1 - y) \cdot \log(1 - f(x)) \right]
$$
**Simplified cost function**
$$
\begin{align*}
J(w, b) &= \frac{1}{m} \sum_{i=1}^{m} \mathcal{L}(f(x^{(i)}), y^{(i)}) \\
       &= -\frac{1}{m} \sum_{i=1}^{m} \left[ y^{(i)} \log(f(x^{(i)})) + \left(1 - y^{(i)}\right) \log\left(1 - f(x^{(i)})\right) \right]
\end{align*}
$$

Codes
```python
# log loss
log_loss = -np.mean(y_true * np.log(y_pred) + (1 - y_true) * np.log(1 - y_pred))

# cross entropy
cross_entropy = -np.mean(np.sum(y_true * np.log(y_pred), axis=1))
```
**softmax loss**

- 多分类损失函数
- PyTorch: `log_softmax + NLLLoss = CrossEntropyLoss`

**KL散度** (Kullback-Leibler Divergence)

- 衡量两个分布的差异
- 交叉熵就是 KL 散度加信息熵，而信息熵是一个常数

$$
D_{KL}(p||q)=\sum_{x\in X} p(x)\log\frac{p(x)}{q(x)}=-\sum_{x\in X} p(x)[\log q(x) - \log p(x)]
$$

逻辑回归中，[损失函数对模型参数的梯度计算结果](https://www.python-unleashed.com/post/derivation-of-the-binary-cross-entropy-loss-gradient)形式上看起来与线性回归类似：

1. 计算损失函数对y_hat的微分 (损失函数的grad)
2. 计算y_hat对sigmoid函数的微分 (传播到激活函数)
3. 计算sigmoid对权重w的微分 (传播到层)

## 2. 最大似然法 Maximum likelihood estimation

最大似然与最小交叉熵损失函数等价，可以从最大似然推导出逻辑回归使用的交叉熵损失函数。

- 假设样本服从[伯努利分布/二项分布 Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution)

$$
L(w)=\prod[p(x_{i})]^{y_{i}}[1-p(x_{i})]^{1-y_{i}}
$$

$$
L(w)=\prod_{i=1}^{m}{p^{y}\cdot(1-p)^{1-y}}
$$

## 3. 最大熵法

最大熵原理：对于概率模型，在所有可能分布的概率模型中，熵最大的模型是最好的模型。熵用来形容一个系统的无序程度

$$
H(X) = -\sum_{x \in X}p(x_i) \log p(x_i)
$$

## 4. 最大后验法

与[线性回归](./02_linear_regression.md)相同

## 5. 优化

### 5.1 在线学习 FTRL

[在线最优化求解(Online Optimization)-冯扬](https://github.com/wzhe06/Ad-papers/blob/master/Optimization%20Method/%E5%9C%A8%E7%BA%BF%E6%9C%80%E4%BC%98%E5%8C%96%E6%B1%82%E8%A7%A3%28Online%20Optimization%29-%E5%86%AF%E6%89%AC.pdf)

- 论文: https://dl.acm.org/doi/pdf/10.1145/2487575.2488200
- 问题: Lasso引入L1正则项使模型的训练结果具有稀疏性,稀疏不仅有变量选择的功能，同时大大减少线上预测运算量。在线学习场景,利用SGD进行权重参数(W)的更新,每次只使用一个样本，权重参数的更新具有很大的随机性,无法将权重参数准确地更新为0
- 用ftrl优化可以得到稀疏权重，从而降低serving的复杂度

## 6. naive bayes 朴素贝叶斯

- 朴素贝叶斯的条件概率 P(X|Y=c) 服从高斯分布时，它计算出来的 P(Y=1|X) 形式跟逻辑回归一样
- Pros
  - Real time predictions: It is very fast and can be used in real time
  - Scalable with Large datasets
  - Insensitive to irrelevant features
  - Multi class prediction is effectively done in Naive Bayes
  - Good performance with high dimensional data(no. of features is large)
- Cons
  - Independence of features does not hold
  - Bad estimator: Probability outputs from predict_proba are not to be taken too seriously
  - Training data should represent population well

## 7. 问答

- pros:

  - interpretable and explainable method
  - less prone to overfitting when using regulation
  - applicable for multi-class predictions
  - Robustness to Feature Noise

- cons:

  - assuming linearity between input and output (Linear Separability)
  - can overfit with small, high dimensional data
  - High reliance on proper presentation of data
  - Multicollinearity impact on interpretation

- 正负样本不平衡

  - [欠采样（undersampling）和过采样（oversampling）会对模型带来怎样的影响？](https://www.zhihu.com/question/269698662/answer/350806067)

- how do you deal with a categorical variable with high cardinality

- 并行

  - 逻辑回归并行化主要是对目标函数梯度计算的并行化。逻辑回归可以认为是极简的MLP。

- 为什么逻辑回归不用mse做损失函数

  - [本质是分布不同，最大似然可以看出，参数正态推导出MSE, 参数二项推导出cross entropy；公式推导发现容易导致梯度消失，很大的话梯度有一项会趋于0；陷入局部最优](https://zhuanlan.zhihu.com/p/453411383)
  - [用平方差后 损失函数是非凸的(non-convex)，很难找到全局最优解](https://towardsdatascience.com/why-not-mse-as-a-loss-function-for-logistic-regression-589816b5e03c)
  - follow up: 为什么GBDT做分类也用回归树呢？损失函数的选择

- Logistic regression和naive bayes的区别

  - LR数据较多时优于NB。NB假设前提是个体独立，无法处理词组

- Logistic regression和SVM的区别

  - 任务不同: 分类，SVM可以解决回归问题
  - loss不同: BCE与hinge loss
  - 输出不同: LR概率输出，SVM score

- LR中连续特征为什么要做离散化

  - 数据角度：离散化的特征对异常数据有很强的鲁棒性；离散化特征利于进行特征交叉。
  - 模型角度：当数据增加/减少时，利于模型快速迭代；离散化相当于为模型引入非线性表达；离散化特征简化了模型输入，降低过拟合风险；LR中离散化特征很容易根据权重找出bad case。
  - 计算角度：稀疏向量内积计算速度快。（在计算稀疏矩阵内积时，可以根据当前值是否为0来直接输出0值，这相对于乘法计算是快很多的。）

- 什么是最大似然估计，其假设是什么？

- 一个分类器，有的 token 在 vocabulary 里面没出现导致概率是0怎么办

  - softmax，概率取对数再求指数

- Multiclass versus multilabel classification
  - multilabel: 每个label都当作一个二分类任务, BCE loss

## 8. 代码

```python
import numpy as np
from scipy.stats import norm
np.random.seed(1)

class LogisticRegression(object):
    def __init__(self):
        self.w = None
        self.b = None
        self.epsilon = 1e-8
        self.activate=self.sigmoid

    def fit(self, x, y):
        batch_size, feature_size = x.shape
        self.w = np.random.random((feature_size, 1))
        self.b = np.random.random(1)
        self._gradient_descent(x, y)

    def predict(self, x):
        prob = self.activate(np.dot(x, self.w) + self.b)
        return prob

    def _gradient_descent(self, x, y, learning_rate=10e-4, epoch=100):
        for i in range(epoch):
            y_pred = self.activate(np.dot(x, self.w) + self.b)
            loss = np.mean(-y * np.log(y_pred + self.epsilon) - (1 - y) * np.log(1 - y_pred + self.epsilon))
            w_gradient = np.dot(x.T, (y_pred - y))
            b_gradient = np.mean(y_pred - y)
            self.w = self.w - learning_rate * w_gradient
            self.b = self.b - learning_rate * b_gradient
            print('step:', i, 'Loss:', loss)

    def sigmoid(self, input):
        return 1 / (1 + np.exp(-input))

    def __str__(self):
        return 'weights\t:%s\n bias\t:%f\n' % (self.w, self.b)

def generate_data():
    x0 = np.hstack((norm.rvs(2, size=40, scale=2), norm.rvs(8, size=40, scale=3)))
    x1 = np.hstack((norm.rvs(4, size=40, scale=2), norm.rvs(5, size=40, scale=2)))
    x = np.transpose(np.vstack((x0, x1)))
    y = np.vstack((np.zeros((40, 1)), np.ones((40, 1))))
    return x,y

if __name__ == "__main__":
    x,y = generate_data()
    lr = LogisticRegression()
    lr.fit(x,y)
    y_hat = lr.predict(x)
```

## 参考

- [【机器学习】逻辑回归](https://zhuanlan.zhihu.com/p/74874291)
- [Cross-entropy](https://en.wikipedia.org/wiki/Cross-entropy)
- [逻辑回归为什么用Sigmoid？ - 雨林丶的文章 - 知乎](https://zhuanlan.zhihu.com/p/59137998)
- [softmax,sigmoid函数在使用上的区别是什么？ - 初识CV的回答 - 知乎](https://www.zhihu.com/question/269431756/answer/1779010934)
- [softmax derivative](https://towardsdatascience.com/derivative-of-the-softmax-function-and-the-categorical-cross-entropy-loss-ffceefc081d1)

---
jupyter:
  jupytext:
    formats: ipynb,md
    main_language: python
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.17.3
  kernelspec:
    display_name: Python 3
    name: python3
---

<!-- #region id="MBUPVi0NmIO_" -->
<center>
<img src="https://raw.githubusercontent.com/dvgodoy/PyTorch101_ODSC_Europe2020/master/images/linear_dogs.jpg" height="200">

# Семинар 1: введение в PyTorch
</center>
<!-- #endregion -->

<!-- #region id="Y_cKCTOGmIPB" -->
В этой тетрадке решение задачек из основного семинара. 
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/", "height": 35} id="e8AYCUtMmIPB" outputId="209e7cac-d78e-4c6a-b7b6-908608c063b1"
import numpy as np
np.__version__
```

```python colab={"base_uri": "https://localhost:8080/", "height": 35} id="zlRDJn3qmIPB" outputId="9322caa0-1c33-402d-c4f2-3be7e116f174"
import torch
torch.__version__
```

```python id="U0mSMtHaMt8H"
import random
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from IPython.display import clear_output
```

<!-- #region id="czZAcA-TmIPN" -->
### Задание 1:

- При помощи numpy посчитайте сумму квадратов чисел от 1 до 10000
- Сделайте то же самое с помощью pytorch
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/"} id="rKjyPSmUmIPN" outputId="88ee225d-ba3d-47b7-ba41-8c72bf119fb8"
np.square(np.arange(1, 10_000 + 1)).sum()
```

```python colab={"base_uri": "https://localhost:8080/"} id="2uNxrlDImIPN" outputId="23ff3166-8e2d-4ede-8f03-741e38f89549"
torch.square(torch.arange(1, 10_000 + 1)).sum()
```

<!-- #region id="0lcMvEX4mIPO" -->
### Задание 2:

Реализуйте на PyTorch сигмоиду

$$ \sigma(x) = \frac{1}{1 + e^{-x}} $$
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/", "height": 430} id="QgZEt30Ktmtt" outputId="3eaf021a-bd90-4f08-ac6b-e2701f89a7cf"
x = torch.linspace(-20, 20, 100)

def sigm(x):
    return 1/(1 + torch.exp(-x))

y = sigm(x)

plt.plot(x, y);
```

<!-- #region id="3bv6XrlimIPO" -->
### Задание 3:

Реализуйте на PyTorch среднюю квадратичную ошибку.

$$
MSE(\hat y, y) = \frac{1}{n} \cdot \sum_{i=1}^n (\hat y - y)^2
$$
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/"} id="4UVaBIUomIPO" outputId="94bf8737-183e-4a5e-ca20-092e2b2d423b"
y_pred = torch.tensor([1, 2, 3, 4, 5], dtype=torch.float)
y_test = torch.tensor([-1, 3, 4, 6, -5], dtype=torch.float)

def mse(y_true, y_pred):
    return torch.mean((y_true - y_pred)**2)

z = mse(y_test, y_pred)
z.item()
```

<!-- #region id="uqFcJNvMmIPO" -->
### Задание 4:

Что будет в переменной `x` в результате выполнения следующего кода? Почему?
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/"} id="HHRnMNNNmIPO" outputId="138fa2b5-01fa-4a6e-e286-7314ad4eac55"
x = torch.tensor([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
y = x
y[2] = torch.ones(3)

print(y)
print('')
print(x)
```

<!-- #region id="NS8SmXANmIPO" -->
Как это можно исправить?
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/"} id="d2fADbnnuCx_" outputId="f810f805-c7be-42bd-966c-5d941e7a5eae"
x = torch.tensor([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
y = x.clone()
y[2] = torch.ones(3)

print(y)
print('')
print(x)
```

<!-- #region id="x-ZtbG4BmIPP" -->
### Задание 5:

Реализуйте расчёт градиента для функции

$$
f(w) = \prod_{i,j} \ln(\ln(w_{ij} + 7)
$$

в точке `w = [[5,10], [1,2]]`
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/"} id="-xHZtAT3vCWq" outputId="5642c1e8-3b85-442d-c87f-ed62a80d4c74"
# ваш код
w =  torch.tensor([[5.,10.],[1.,2.]], requires_grad=True)

function =  torch.prod(torch.log(torch.log(w + 7)))
function.backward()

print(w.grad)
```

<!-- #region id="NZwVXOxymIPQ" -->
### Задание 6:

Решите задачу

$$
f(w) = \prod_{i,j} \ln(\ln(w_{ij} + 7) \to \min_{w}
$$

с помощью градиентного спуска. Выведите минимальное значение.
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/"} id="Hnp8pXx7v_N1" outputId="3a9303c5-ba9c-4930-f2c4-3efa8476c98c"
w = torch.tensor([[5., 10.], [1., 2.]], requires_grad=True)
alpha = 0.001

for _ in range(500):
    function = (w + 7).log().log().prod()
    function.backward()
    w.data -= alpha * w.grad
    w.grad.zero_()

print(w)
```

<!-- #region id="vOP0amcbmIPU" -->
### Задание 7:

Напишите `DataLoader` для тренировочной и тестовой выборок. Попробуйте проитерироваться по нескольким его первым объектам с помощью цикла.

В обучающей выборке данные должны перемешиваться каждую эпоху. Размер батча поставьте равным 64. В тестовой выборке данные перемешивать не надо.
<!-- #endregion -->

```python id="n0HNWClT1NNG"
# берем датасет, перемешиваем его и формируем батчи
train_loader = torch.utils.data.DataLoader(
    train_set, batch_size=64, shuffle=True
)

test_loader = torch.utils.data.DataLoader(
    test_set, batch_size=64, shuffle=False
)
```

```python colab={"base_uri": "https://localhost:8080/"} id="xtsOf55Z1Oea" outputId="fbeb973a-8884-4632-acba-1bd4f8b5378d"
for images, labels in train_loader:
    print(images.shape, labels.shape)
    break
```

<!-- #region id="vNhnw4TsmIPU" -->
### Задание 8:

Напишите  двухслойную полносвязную нейросеть. Выходной слой должен состоять из $10$ нейронов.
<!-- #endregion -->

```python id="GP9wH0VhmIPU"
class NeuralNet(nn.Module):
    def __init__(self, in_features, num_classes, hidden_size):
        super().__init__()
        self.model = nn.Sequential(
            nn.Linear(in_features=in_features, out_features=hidden_size),
            nn.ReLU(),
            nn.Linear(in_features=hidden_size, out_features=hidden_size, bias=False),
            nn.LeakyReLU(0.1),
            nn.Linear(in_features=hidden_size, out_features=num_classes),
        )

    def forward(self, x):
        return self.model(x)
```

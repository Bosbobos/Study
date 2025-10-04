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
# Выводы из семинара
![[3 - Disciplines/DeepLearning/Notes#3 - convnet optimization|Notes]]


<!-- #region id="kQE-pczIE1iO" -->
# Кошки против собак


<!-- #endregion -->

```python id="k_G6I5cwHZsg"
import matplotlib.pyplot as plt
import numpy as np

import seaborn as sns
from tqdm.auto import tqdm
```

```python id="_-VIukcbCrQN"
import torch
import torchvision

import torch.nn.functional as F
from torch import nn
from torch.utils.data import DataLoader

from torchvision import transforms as T
from torchvision.datasets import ImageFolder
```

```python colab={"base_uri": "https://localhost:8080/", "height": 225} id="QahwvpYJKgVE" outputId="78db0eab-82dc-4ebe-f7bf-99d9e80346de"
import wandb
wandb.login()
```

```python colab={"base_uri": "https://localhost:8080/", "height": 35} id="UsSo_Xa2FYKw" outputId="d4a124aa-0aa3-47b3-a171-6364ffdd91cc"
device = "cuda" if torch.cuda.is_available() else "cpu"
device
```

<!-- #region id="OKBrggFGK7jD" -->
## 1. Данные
<!-- #endregion -->

<!-- #region id="uDb72LXaDtdI" -->
На этой паре мы обучим нейросетку отличать кошек от собак. Если вы ещё не забыли лекцию про свёрточные сетки, в $2013$ году это было непростой задачкой. Настолько, что [к соревнованию на Kaggle](https://www.kaggle.com/c/dogs-vs-cats) висит такое предысловие:

> В 1997 году Deep Blue обыграл в шахматы Каспарова.  В 2011 Watson обставил чемпионов Jeopardy. Сможет ли ваш алгоритм в 2013 году отличить Бобика от Пушистика?

В этом семинаре мы попробуес сделать Transfer learning и посмотрим какое качество будет у нашей модели.
<!-- #endregion -->

```bash id="3MGnWzP0C58q"
wget https://storage.googleapis.com/mledu-datasets/cats_and_dogs_filtered.zip
```

```bash id="M86t8kKzC5_c"
unzip -q cats_and_dogs_filtered.zip
rm cats_and_dogs_filtered.zip
rm cats_and_dogs_filtered/vectorize.py
```

```python colab={"base_uri": "https://localhost:8080/"} id="Sz4FfbUzC6Bn" outputId="fbc789f4-afcb-4821-cba3-832de9ce2b04"
!du -sh cats_and_dogs_filtered
```

<!-- #region id="P4YUMWg-E_P1" -->
Все файлы сейчас лежат по папкам, которые соотвествуют классам.

<pre style="font-size: 10.0pt; font-family: Arial; line-height: 2; letter-spacing: 1.0pt;" >
<b>cats_and_dogs_filtered</b>
|__ <b>train</b>
    |______ <b>cats</b>: [cat.0.jpg, cat.1.jpg, cat.2.jpg ....]
    |______ <b>dogs</b>: [dog.0.jpg, dog.1.jpg, dog.2.jpg ...]
|__ <b>validation</b>
    |______ <b>cats</b>: [cat.2000.jpg, cat.2001.jpg, cat.2002.jpg ....]
    |______ <b>dogs</b>: [dog.2000.jpg, dog.2001.jpg, dog.2002.jpg ...]
</pre>
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/", "height": 35} id="HHav6DIvEyHG" outputId="53e69915-cf9c-4a75-a74e-2f60a42e62c6"
path = !pwd
path = path[0] + '/'
path
```

```python id="XNeqKf_4Ez32"
normalize = T.Normalize(
    mean=[0.485, 0.456, 0.406],
    std=[0.229, 0.224, 0.225]
)

test_transform = T.Compose([
    T.Resize(256),
    T.CenterCrop(224),
    T.ToTensor(),
    normalize,
])

aug_transform = T.Compose([
    T.ColorJitter(hue=0.05, saturation=0.05),
    T.RandomHorizontalFlip(),
    T.RandomRotation(20),
    T.Resize(256),
    T.RandomResizedCrop(224, scale=(0.5, 1.0)),
    T.ToTensor(),
    normalize,
])

dataset_pets = ImageFolder(
    path +  'cats_and_dogs_filtered/train/',
    transform = aug_transform #test_transform
 )

# создаём встроенными методами два датасета :)
train_dataset, val_dataset = torch.utils.data.random_split(
    dataset_pets, [int(0.8 * len(dataset_pets)), len(dataset_pets) - int(0.8 * len(dataset_pets))]
)

test_dataset = ImageFolder(
    path +  'cats_and_dogs_filtered/validation/',
    transform = test_transform
 )
```

```python id="RCBKnUpZUMBy"
train_loader = torch.utils.data.DataLoader(
    train_dataset,
    batch_size     = 32,
    shuffle        = True,
    pin_memory     = True,
    num_workers    = 0
)

val_loader = torch.utils.data.DataLoader(
    val_dataset,
    batch_size    = 4096,
    shuffle       = False,
    pin_memory     = True,
    num_workers   = 0
)

test_loader = torch.utils.data.DataLoader(
    test_dataset,
    batch_size    = 4096,
    shuffle       = False,
    pin_memory     = True,
    num_workers   = 0
)
```

```python colab={"base_uri": "https://localhost:8080/"} id="JNsAgDhuG18p" outputId="63a11e7c-ac44-4b4c-ff7c-c8228b01772b"
dataset_pets.classes
```

```python colab={"base_uri": "https://localhost:8080/"} id="72nisCFfEyOI" outputId="73d31927-1cb0-4d14-9f12-88c1b4d24077"
image, label = next(iter(dataset_pets))
dataset_pets.classes[label]

image.shape
```

```python id="wgvs6arTISTK"
# print(image.shape)
# plt.imshow(np.transpose(image, (1,2,0)));
```

```python colab={"base_uri": "https://localhost:8080/"} id="WNQMSMzpJeB0" outputId="7f484c6e-cc3a-41b7-a427-c6da30d9fc34"
len(dataset_pets)
```

<!-- #region id="THrGUUyrKw-O" -->
## 2. Циклы для обучения
<!-- #endregion -->

```python id="JdHZ3LhlK2v4"
from IPython.display import clear_output

def plot_losses(train_losses, test_losses, train_accuracies, test_accuracies):
    clear_output()
    fig, axs = plt.subplots(1, 2, figsize=(13, 4))
    axs[0].plot(range(1, len(train_losses) + 1), train_losses, label='train')
    axs[0].plot(range(1, len(test_losses) + 1), test_losses, label='test')
    axs[0].set_ylabel('loss')

    axs[1].plot(range(1, len(train_accuracies) + 1), train_accuracies, label='train')
    axs[1].plot(range(1, len(test_accuracies) + 1), test_accuracies, label='test')
    axs[1].set_ylabel('accuracy')

    for ax in axs:
        ax.set_xlabel('epoch')
        ax.legend()

    plt.show()
```

```python id="nduibWk1J3CF"
def training_epoch(model, optimizer, criterion, train_loader, tqdm_desc, wandb_project=None):
    """Одна эпоха обучения"""
    train_loss, train_accuracy = 0.0, 0.0
    model.train()
    for images, labels in tqdm(train_loader, desc=tqdm_desc):
        images = images.to(device)
        labels = labels.to(device)

        optimizer.zero_grad()
        logits = model(images)
        loss = criterion(logits, labels)
        loss.backward()
        optimizer.step()

        train_loss += loss.item() * images.shape[0]
        train_accuracy += (logits.argmax(dim=1) == labels).sum().item()

        if wandb_project:
            metrics = {
                "batch-train/loss": loss.item()
            }
            wandb.log(metrics)

    train_loss /= len(train_loader.dataset)
    train_accuracy /= len(train_loader.dataset)
    return train_loss, train_accuracy


@torch.no_grad()
def validation_epoch(model, criterion, val_loader, tqdm_desc):
    """Прогнозы на валидации"""

    val_loss, val_accuracy = 0.0, 0.0
    model.eval()
    for images, labels in tqdm(val_loader, desc=tqdm_desc):
        images = images.to(device)
        labels = labels.to(device)

        logits = model(images)
        loss = criterion(logits, labels)

        val_loss += loss.item() * images.shape[0]
        val_accuracy += (logits.argmax(dim=1) == labels).sum().item()

    val_loss /= len(val_loader.dataset)
    val_accuracy /= len(val_loader.dataset)
    return val_loss, val_accuracy


def train(
    model, optimizer, criterion, train_loader, val_loader, num_epochs, scheduler=None,
    wandb_project=None, config=None
    ):
    """Обучение модели"""
    train_losses, train_accuracies = [], []
    val_losses, val_accuracies = [], []

    # подключаем wandb
    if wandb_project:
        wandb.init(
            project=wandb_project,
            config=config
        )
        wandb.watch(model)

    for epoch in range(1, num_epochs + 1):
        clear_output()
        train_loss, train_accuracy = training_epoch(
            model, optimizer, criterion, train_loader,
            tqdm_desc=f'Training {epoch}/{num_epochs}',
            wandb_project=wandb_project
        )
        val_loss, val_accuracy = validation_epoch(
            model, criterion, val_loader,
            tqdm_desc=f'Validating {epoch}/{num_epochs}'
        )

        train_losses.append(train_loss)
        train_accuracies.append(train_accuracy)
        val_losses.append(val_loss)
        val_accuracies.append(val_accuracy)

        # функция для смены lr по расписанию
        if scheduler is not None:
            scheduler.step()

        if wandb_project:
            metrics = {
                "train/loss": train_loss / len(train_loader.dataset),
                "train/accuracy": train_accuracy / len(train_loader.dataset),
                "val/loss": val_loss,
                "val/accuracy": val_accuracy
            }
            wandb.log(metrics)
        else:
          plot_losses(train_losses, val_losses, train_accuracies, val_accuracies)

        plt.show()
    # печатаем метрики
    print(f"Epoch: {epoch}, loss: {np.mean(val_loss)}, accuracy: {np.mean(val_accuracy)}")
```

<!-- #region id="HVVg3oCbK-jB" -->
## 3. Модели
<!-- #endregion -->

```python id="BlgWXEscKbK-"
class ConvNet(nn.Module):

    def __init__(self, image_channels=3):
        super().__init__()

        self.encoder = nn.Sequential(  # 28 x 28
            nn.Conv2d(in_channels=image_channels, out_channels=4,
                      kernel_size=3, padding='same'),  # 28 x 28
            nn.ReLU(),
            nn.MaxPool2d(2),  # 14 x 14

            nn.Conv2d(in_channels=4, out_channels=8,
                      kernel_size=3, padding='same'),  # 14 x 14
            nn.ReLU(),
            nn.MaxPool2d(2),  # 7 x 7

            nn.Conv2d(in_channels=8, out_channels=16,
                      kernel_size=3, padding='same'),  # 7 x 7
            nn.ReLU(),
            nn.MaxPool2d(2)  # 3 x 3
        )

        self.head = nn.Sequential(
            nn.LazyLinear(out_features=32),
            nn.ReLU(),
            nn.Linear(in_features=32, out_features=10)
        )

    def forward(self, x):
        # x: B x 1 x 28 x 28
        out = self.encoder(x)   # out: B x 392
        out = nn.Flatten()(out) # out: B x 128
        out = self.head(out)    # out: B x 10
        return out

    def get_embedding(self, x):
        out = self.encoder(x)
        return nn.Flatten()(out)
```

```python id="nQGwWtlwLAH4"
model_cnn = ConvNet( )
```

```python id="rfTuCML7LbK3"

```

```python id="6MJ82Z8gLAMk"

```

<!-- #region id="bYBeAvLWLATW" -->
## 4. Обучение
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/"} id="lL0zP54sPDTz" outputId="e39a5abb-57d7-43e6-af67-4160850a3976"
x_batch, y_batch = next(iter(train_loader))
x_batch.shape
```

```python colab={"base_uri": "https://localhost:8080/", "height": 98, "referenced_widgets": ["66ff4562ee274a8e9cf99c4471a09f2f", "fda0f98548fd4c47b75a3d790ae45b80", "be53f9f5f86847bbb9442e8d3f328b43", "775e3d8c5a1d475d850de22ece8f4fc2", "86ebf358bad344318d646b83b692d1dd", "b8da0107792f41f6808398ca03741c6d", "e1147197dc4047eab5785c6cd2aca40f", "92e8560103e94fcf96d69b605ecf6cc5", "12a3408fa8924bcd96f0192e072f5020", "0e54829a777146cb8e00022878595198", "aba5e0eff5234925812cba69f86aed69", "1f6ea96b8de94039b10627cd7ed39d86", "6cffb0c874a842ac87a787c5556d05c6", "c1f2098910094f5496a01e8a0efcf839", "5ad0205d1edd40be8cb8e833534109d2", "20c8ef0ea786460c83d8ca59ec67de98", "3a240c7ba7dc4ae8a7c925616bbcc15d", "18b86ff6854b4df681fc6bd25555de58", "e9c1a5e461bd4da8b5f658bb3c207df2", "91ce359e1a1244eba3f64f66206f8d34", "20ad592e273243a98c766760b9e2b07e", "02ba89b24d7c47f7884ac92d22d26ef5"]} id="bPKn6Xo-J3Er" outputId="ced22dc4-21a4-4437-d3f0-492bbab00f98"
NUM_EPOCH = 10
LR = 0.01

model_cnn = ConvNet().to(device)
model_cnn(x_batch.to(device)) # костыль для lazy init

criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(model_cnn.parameters(), lr=LR, momentum=0)

config_model = {
    "architecture": 'CNN',
    "learning_rate": LR,
    "scheduler": 'None',
    "epochs":  NUM_EPOCH,
    "optimizer": 'SGD',
    "experiment": 'Augmented CNN'
}

train(model_cnn, optimizer, criterion, train_loader, val_loader, NUM_EPOCH,
      wandb_project='cats_vs_dogs', config = config_model
);
```

```python colab={"background_save": true, "base_uri": "https://localhost:8080/", "height": 49, "referenced_widgets": ["d4e40f292ba14a5284455594e2e420a0", "9c684b806c1e47a88f3388ec74983a9e"]} id="nviuxNP5J3HR" outputId="276b9154-c660-4b4e-f326-500c5f65dedb"
NUM_EPOCH = 10
LR = 0.001

model_cnn = ConvNet().to(device)
model_cnn(x_batch.to(device)) # костыль для lazy init

criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model_cnn.parameters())

config_model = {
    "architecture": 'CNN',
    "learning_rate": LR,
    "scheduler": 'None',
    "epochs":  NUM_EPOCH,
    "optimizer": 'Adam',
    "experiment": 'Augmented CNN'
}

train(model_cnn, optimizer, criterion, train_loader, val_loader, NUM_EPOCH,
      wandb_project='cats_vs_dogs', config = config_model
);
```

```python id="hVqPUztIHJs5"
NUM_EPOCH = 10
LR = 0.001

model_cnn = ConvNet().to(device)
model_cnn(x_batch.to(device)) # костыль для lazy init

criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.AdamW(model_cnn.parameters())

config_model = {
    "architecture": 'CNN',
    "learning_rate": LR,
    "scheduler": 'None',
    "epochs":  NUM_EPOCH,
    "optimizer": 'AdamW',
    "experiment": 'Augmented CNN'
}

train(model_cnn, optimizer, criterion, train_loader, val_loader, NUM_EPOCH,
      wandb_project='cats_vs_dogs', config = config_model
);
```

```python id="6MA-XgPwP_aF"

```

<!-- #region id="b6WaBzsFVPVC" -->
### Различные трюки:

1. Начните с маленькой сети. Не забывайте прикидывать сколько наблдюдений  тратится на оценку каждого из  параметров. Если величина очень маленькая, не забывайте о регуляризации.
2. Всегда оставляйте часть выборки под валидацию на каждой эпохе.
3. Усложняйте модель, пока качество на валидации не начнёт падать.
4. Не забывайте проскалировать ваши наблюдения для лучшей сходимости.
5. Можно попробовать ещё целую серию различных трюков:
  - Архитектура нейросети
    - Больше/меньше нейронов
    - Больше/меньше слоёв
    - Другие функции активации (tanh, relu, leaky relu, elu etc)
    - Регуляризация (dropout, l1,l2)
  - Более качественная оптимизация
    - Можно попробовать выбрать другой метод оптимизации
    - Можно попробовать менять скорость обучения, моментум и др.
    - Разные начальные значения весов
  - Попробовать собрать больше данных
  - Для случая картинок объёмы данных можно увеличить искусственно с помощью подхода, который называется Data augmemntation

И это далеко не полный список. Обратите внимание, что делать grid_search для больших сеток это довольно времязатратное занятие...

Логгируйте свои эксперименты. За один прогон пробуйте одно изменение. Иначе будет непонятно какие именно изменения улучшили качество, а какие ухудшили.
<!-- #endregion -->

```python id="kvPWepS3VOv4"

```

```python id="nFayfYAVVOzI"

```

```python id="KtrYG3cAVCXK"

```

```python id="er_XaSIHVCeL"

```

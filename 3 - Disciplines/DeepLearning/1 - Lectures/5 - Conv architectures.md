annotation-target::lecture05-optimization-convnets.pdf

>%%
>```annotation-json
>{"created":"2025-10-02T11:46:13.048Z","text":"Похожая идея была при использовании bag of words, когда мы записывали количество вхождений слова на общее количество слов","updated":"2025-10-02T11:46:13.048Z","document":{"title":"lecture05-optimization-convnets.pdf","link":[{"href":"urn:x-pdf:7150d46c686eb5a15196d105929d55ba"},{"href":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf"}],"documentFingerprint":"7150d46c686eb5a15196d105929d55ba"},"uri":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","target":[{"source":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","selector":[{"type":"TextPositionSelector","start":271,"end":290},{"type":"TextQuoteSelector","exact":"Layer Normalization","prefix":" среднее и дисперсию к 𝛽\" и 𝛾\"","suffix":"𝑥!!⋯𝑥!\"⋮⋱⋮𝑥#!⋯𝑥#\"•𝑛 — разме"}]}]}
>```
>%%
>*%%PREFIX%%среднее и дисперсию к 𝛽" и 𝛾"%%HIGHLIGHT%% ==Layer Normalization== %%POSTFIX%%𝑥!!⋯𝑥!"⋮⋱⋮𝑥#!⋯𝑥#"•𝑛 — разме*
>%%LINK%%[[#^89gsmccan58|show annotation]]
>%%COMMENT%%
>Похожая идея была при использовании bag of words, когда мы записывали количество вхождений слова на общее количество слов
>%%TAGS%%
>
^89gsmccan58


>%%
>```annotation-json
>{"created":"2025-10-02T11:47:19.104Z","text":"Проблема - 90% параметров находилась в FC-части, то есть мы плохо извлекали параметры, и потом жестко с ними работали","updated":"2025-10-02T11:47:19.104Z","document":{"title":"lecture05-optimization-convnets.pdf","link":[{"href":"urn:x-pdf:7150d46c686eb5a15196d105929d55ba"},{"href":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf"}],"documentFingerprint":"7150d46c686eb5a15196d105929d55ba"},"uri":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","target":[{"source":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","selector":[{"type":"TextPositionSelector","start":2408,"end":2415},{"type":"TextQuoteSelector","exact":"AlexNet","prefix":"nvolutional-neural-networks.pdf ","suffix":" (2012)https://papers.nips.cc/pa"}]}]}
>```
>%%
>*%%PREFIX%%nvolutional-neural-networks.pdf%%HIGHLIGHT%% ==AlexNet== %%POSTFIX%%(2012)https://papers.nips.cc/pa*
>%%LINK%%[[#^2sprjnumflp|show annotation]]
>%%COMMENT%%
>Проблема - 90% параметров находилась в FC-части, то есть мы плохо извлекали параметры, и потом жестко с ними работали
>%%TAGS%%
>
^2sprjnumflp


>%%
>```annotation-json
>{"created":"2025-10-02T11:48:12.667Z","text":"Переносит большее количество параметров в сверточную часть, чтобы модель была более сбалансированной.","updated":"2025-10-02T11:48:12.667Z","document":{"title":"lecture05-optimization-convnets.pdf","link":[{"href":"urn:x-pdf:7150d46c686eb5a15196d105929d55ba"},{"href":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf"}],"documentFingerprint":"7150d46c686eb5a15196d105929d55ba"},"uri":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","target":[{"source":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","selector":[{"type":"TextPositionSelector","start":2784,"end":2787},{"type":"TextQuoteSelector","exact":"VGG","prefix":"s://arxiv.org/pdf/1409.1556.pdf ","suffix":" (2014)https://arxiv.org/pdf/140"}]}]}
>```
>%%
>*%%PREFIX%%s://arxiv.org/pdf/1409.1556.pdf%%HIGHLIGHT%% ==VGG== %%POSTFIX%%(2014)https://arxiv.org/pdf/140*
>%%LINK%%[[#^jxrllz84vca|show annotation]]
>%%COMMENT%%
>Переносит большее количество параметров в сверточную часть, чтобы модель была более сбалансированной.
>%%TAGS%%
>
^jxrllz84vca


>%%
>```annotation-json
>{"created":"2025-10-02T11:48:37.830Z","text":"Оказалось, что много 3х3 слоёв сильно лучше, чем мало 11х11 слоёв.","updated":"2025-10-02T11:48:37.830Z","document":{"title":"lecture05-optimization-convnets.pdf","link":[{"href":"urn:x-pdf:7150d46c686eb5a15196d105929d55ba"},{"href":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf"}],"documentFingerprint":"7150d46c686eb5a15196d105929d55ba"},"uri":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","target":[{"source":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","selector":[{"type":"TextPositionSelector","start":2831,"end":2855},{"type":"TextQuoteSelector","exact":"Только маленькие свёртки","prefix":"://arxiv.org/pdf/1409.1556.pdf •","suffix":"•Меньше параметров•Больше нелине"}]}]}
>```
>%%
>*%%PREFIX%%://arxiv.org/pdf/1409.1556.pdf •%%HIGHLIGHT%% ==Только маленькие свёртки== %%POSTFIX%%•Меньше параметров•Больше нелине*
>%%LINK%%[[#^tkddb87qbvr|show annotation]]
>%%COMMENT%%
>Оказалось, что много 3х3 слоёв сильно лучше, чем мало 11х11 слоёв.
>%%TAGS%%
>
^tkddb87qbvr


>%%
>```annotation-json
>{"created":"2025-10-02T11:49:13.058Z","text":"Проблема - чтобы обучить более сложную версию, надо для начала обучить более простую и ей инициализировать следующую. Это долго и сложно - вместо одной нейронки обучаем 5.\n\nПотом выяснилось, что добавление батчнорма позволяет сразу обучать большую модель, так что проблема была в слишком сложном ландшафте функционала ошибки.","updated":"2025-10-02T11:49:13.058Z","document":{"title":"lecture05-optimization-convnets.pdf","link":[{"href":"urn:x-pdf:7150d46c686eb5a15196d105929d55ba"},{"href":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf"}],"documentFingerprint":"7150d46c686eb5a15196d105929d55ba"},"uri":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","target":[{"source":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","selector":[{"type":"TextPositionSelector","start":3035,"end":3055},{"type":"TextQuoteSelector","exact":"Хитрая инициализация","prefix":" двух первых полносвязных слоёв•","suffix":" (сначала обучается вариант A со"}]}]}
>```
>%%
>*%%PREFIX%%двух первых полносвязных слоёв•%%HIGHLIGHT%% ==Хитрая инициализация== %%POSTFIX%%(сначала обучается вариант A со*
>%%LINK%%[[#^4ylgqqcr94v|show annotation]]
>%%COMMENT%%
>Проблема - чтобы обучить более сложную версию, надо для начала обучить более простую и ей инициализировать следующую. Это долго и сложно - вместо одной нейронки обучаем 5.
>
>Потом выяснилось, что добавление батчнорма позволяет сразу обучать большую модель, так что проблема была в слишком сложном ландшафте функционала ошибки.
>%%TAGS%%
>
^4ylgqqcr94v


>%%
>```annotation-json
>{"created":"2025-10-02T11:49:48.955Z","text":"Свертка 1 на 1 это как в методе главных компонент - мы все каналы одного \"пикселя\" клеим в один\nЗдесь в целом идет понижение размерности - мы разными способами достаем разные зависимости из параметров. Но это совсем капец и наука свернула не туда.","updated":"2025-10-02T11:49:48.955Z","document":{"title":"lecture05-optimization-convnets.pdf","link":[{"href":"urn:x-pdf:7150d46c686eb5a15196d105929d55ba"},{"href":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf"}],"documentFingerprint":"7150d46c686eb5a15196d105929d55ba"},"uri":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","target":[{"source":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","selector":[{"type":"TextPositionSelector","start":3302,"end":3320},{"type":"TextQuoteSelector","exact":"«projection layer»","prefix":"4)http://arxiv.org/abs/1409.4842","suffix":"свёртки делаются с паддингом!Goo"}]}]}
>```
>%%
>*%%PREFIX%%4)http://arxiv.org/abs/1409.4842%%HIGHLIGHT%% ==«projection layer»== %%POSTFIX%%свёртки делаются с паддингом!Goo*
>%%LINK%%[[#^ohaw6d2mxl|show annotation]]
>%%COMMENT%%
>Свертка 1 на 1 это как в методе главных компонент - мы все каналы одного "пикселя" клеим в один
>Здесь в целом идет понижение размерности - мы разными способами достаем разные зависимости из параметров. Но это совсем капец и наука свернула не туда.
>%%TAGS%%
>
^ohaw6d2mxl


>%%
>```annotation-json
>{"created":"2025-10-02T11:51:00.642Z","text":"Значит backprop не доходит до первых слоев модели (скорее всего). Так что погнали просто к результату слоя добавим то, что пришло к нему не вход.","updated":"2025-10-02T11:51:00.642Z","document":{"title":"lecture05-optimization-convnets.pdf","link":[{"href":"urn:x-pdf:7150d46c686eb5a15196d105929d55ba"},{"href":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf"}],"documentFingerprint":"7150d46c686eb5a15196d105929d55ba"},"uri":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","target":[{"source":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","selector":[{"type":"TextPositionSelector","start":3772,"end":3858},{"type":"TextQuoteSelector","exact":"Хотя возможностей для переобучения больше, сеть почему-то не может ими воспользоваться","prefix":"дшает качество даже на обучении•","suffix":"ResNet (2015)https://arxiv.org/a"}]}]}
>```
>%%
>*%%PREFIX%%дшает качество даже на обучении•%%HIGHLIGHT%% ==Хотя возможностей для переобучения больше, сеть почему-то не может ими воспользоваться== %%POSTFIX%%ResNet (2015)https://arxiv.org/a*
>%%LINK%%[[#^8on0hzr2h49|show annotation]]
>%%COMMENT%%
>Значит backprop не доходит до первых слоев модели (скорее всего). Так что погнали просто к результату слоя добавим то, что пришло к нему не вход.
>%%TAGS%%
>
^8on0hzr2h49


>%%
>```annotation-json
>{"created":"2025-10-02T11:51:32.599Z","text":"Свертка со страйдом 2 заменяет макпулинг в более новых моделях (оно понижает размерность!!)\nПри этом когда размерность меняется (к примеру 128х128х128 -> 64х64х256) мы пространственно сжимаем (берем каждый второй пиксель), а каналы добиваем нулями","updated":"2025-10-02T11:51:32.599Z","document":{"title":"lecture05-optimization-convnets.pdf","link":[{"href":"urn:x-pdf:7150d46c686eb5a15196d105929d55ba"},{"href":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf"}],"documentFingerprint":"7150d46c686eb5a15196d105929d55ba"},"uri":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","target":[{"source":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","selector":[{"type":"TextPositionSelector","start":3904,"end":3910},{"type":"TextQuoteSelector","exact":"ResNet","prefix":"ttps://arxiv.org/abs/1512.03385 ","suffix":" (2015)ResNet blockGlobal Averag"}]}]}
>```
>%%
>*%%PREFIX%%ttps://arxiv.org/abs/1512.03385%%HIGHLIGHT%% ==ResNet== %%POSTFIX%%(2015)ResNet blockGlobal Averag*
>%%LINK%%[[#^eg4mpd22rn|show annotation]]
>%%COMMENT%%
>Свертка со страйдом 2 заменяет макпулинг в более новых моделях (оно понижает размерность!!)
>При этом когда размерность меняется (к примеру 128х128х128 -> 64х64х256) мы пространственно сжимаем (берем каждый второй пиксель), а каналы добиваем нулями
>%%TAGS%%
>
^eg4mpd22rn


>%%
>```annotation-json
>{"created":"2025-10-02T11:52:01.589Z","text":"Global average pooling на последнем сверточном слое нужен, чтобы картинки на вход можно было подавать любого размера (по крайней мере в теории, на деле работает только с маленькими изменениями типа 200х200->250х250)","updated":"2025-10-02T11:52:01.589Z","document":{"title":"lecture05-optimization-convnets.pdf","link":[{"href":"urn:x-pdf:7150d46c686eb5a15196d105929d55ba"},{"href":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf"}],"documentFingerprint":"7150d46c686eb5a15196d105929d55ba"},"uri":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","target":[{"source":"vault:/3%20-%20Disciplines/DeepLearning/9%20-%20Files/lecture05-optimization-convnets.pdf","selector":[{"type":"TextPositionSelector","start":3929,"end":3951},{"type":"TextQuoteSelector","exact":"Global Average Pooling","prefix":".03385 ResNet (2015)ResNet block","suffix":"ResNet (2015)https://arxiv.org/a"}]}]}
>```
>%%
>*%%PREFIX%%.03385 ResNet (2015)ResNet block%%HIGHLIGHT%% ==Global Average Pooling== %%POSTFIX%%ResNet (2015)https://arxiv.org/a*
>%%LINK%%[[#^li2hn2795m|show annotation]]
>%%COMMENT%%
>Global average pooling на последнем сверточном слое нужен, чтобы картинки на вход можно было подавать любого размера (по крайней мере в теории, на деле работает только с маленькими изменениями типа 200х200->250х250)
>%%TAGS%%
>
^li2hn2795m

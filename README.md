# Segment-Bio


Training project
Samples: Image(original image - user image+user label) Number of class: 2 Image: RGB x1200

Goal: train Unet for very small set (17 images(labels))

Model: pretrained VGG16 + UNet

loss - cross-entropy metric - DICE coeff. (https://towardsdatascience.com/metrics-to-evaluate-your-semantic-segmentation-model-6bcb99639aa2#:~:text=3.-,Dice%20Coefficient%20(F1%20Score),of%20union%20in%20section%202).)

Задача обучения на малой выборке (17 картинок/разметок)

Основная идея: разделить на подкадры + сделать аугментацию

Создадим сеть на основе претренированной модели VGG16 (Imagenet - там похожие по смыслу картинки при обучении)

Грузим VGG16 и определяем список слоев для работы :

смотрим на схему сжатия и не идем дальше 25х25 (нужно будет переделать на нормальные размеры типа 256х1024 - при чтении исходных кадров, тогда можно сжимать до 2х2 :))
отделяю выход 13-го слоя, для работы в моей сети
все слои замораживаю (все 18 слоев)
От последнего рабочего слоя VGG16 (block4_conv3 (Conv2D)) ,буду надстраивать свою сеть

При обучении используем размер тензоров (,200, 200, 3)(,200,200,14).

После обучения перестраиваю сеть на размер (,200, 600, 3)(,200,600,14) .

Это позволяет работать с любой картинкой нужного размера, а учить на меньших объектах, экономим память и время обработки, повышаем разнообразие в каждом пакекте (для медицинских фоток типа гистологий работает отлично).

Второй пример(пока в работе) UNet_COVID19_CT  из https://www.kaggle.com/andrewmvd/covid19-ct-scans - ноутбук работает под kaggle https://www.kaggle.com/mariiakorliakova/unet-covid19-ct
сегментация КТ картин для ковида - схема таже

# SIMPLify-x Instalation Guide

* [SMPLify-x](#smplify-x)
* [PyTorch](#pytorch)
* [SMPL-X](#smpl-x)
* [VPoser](#vposer)
* [Homogenus](#homogenus)
* [OpenPose](#openpose)

## SMPLify-x
Just clone SMPLify-x from [repo](https://github.com/vchoutas/smplify-x)

`git clone https://github.com/vchoutas/smplify-x.git`

## OpenPose
To install openPose from docker image:
1. [Install nvidia-container-toolkit](https://github.com/NVIDIA/nvidia-docker#ubuntu-160418042004-debian-jessiestretchbuster)
2. [Install nvidia-docker 2.0](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0))

To run the container, use the following commmand (Ubuntu):
At first run, docker will download an image.
```bash
sudo docker run -it -v /home/dimon/openposedata/:/openpose/data/ --net=host -e DISPLAY --runtime=nvidia exsidius/openpose
```
If we want sharing files between your system and docker container (for example, processing any media from filesystem), we need to `-v /home/dimon/openposedata/:/openpose/data/`.
Where:
* `/home/dimon/openposedata/` is a path to your folder, that you need get access inside docker container.
* `/openpose/data/` is a path inside docker container, linked to your real folder `/home/dimon/openposedata/`
* `exsidius/openpose` is a name of downloaded docker image


Processing images:
```bash
 ./build/examples/openpose/openpose.bin --image_dir ./data --display 0 --write_json ./data/result --write_images ./data/result --face --hand

```
Сноски и примечания[^1] задаются так[^2]:

Here is the steps, to download COCO (по-умолча) model, and save docker image ([how-to-commit-changes-to-docker-image](https://phoenixnap.com/kb/how-to-commit-changes-to-docker-image)):
* run container: `docker run -it --runtime=nvidia exsidius/openpose`
* install wget: `apt-get install wget`
* go to folder, and download COCO-model:
```
cd models/pose/coco/
wget https://github.com/foss-for-synopsys-dwc-arc-processors/synopsys-caffe-models/raw/master/caffe_models/openpose/caffe_model/pose_iter_440000.caffemodel
```
* Как только вы закончите модифицировать новый контейнер, выйдите из него:
`exit`
Предложите системе отобразить список запущенных контейнеров :
`sudo docker ps -a`
Вам потребуется **идентификатор КОНТЕЙНЕРА**, чтобы сохранить изменения, внесенные в существующее изображение. Скопируйте значение идентификатора из вывода.
* Наконец, создайте новое изображение, зафиксировав изменения, используя следующий синтаксис:
`sudo docker commit [CONTAINER_ID] [new_image_name]`
Поэтому в нашем примере это будет:
`sudo docker commit c0d4c6800b5d openpose`
Где *c0d4c6800b5d* находится **идентификатор КОНТЕЙНЕРА** и **openpose** имя нового Image.
* Ваше вновь созданное изображение теперь должно быть доступно в списке локальных изображений. Вы можете проверить, проверив список изображений снова:
`sudo docker images`

[^1]: Все сноски отображаются в конце страницы
[^2]: Просто не так ли?)

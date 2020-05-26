# SIMPLify-x Instalation Guide
### Table of contents
* [SMPLify-x](#smplify-x)
* [SMPL-X](#smpl-x)
* [PyTorch](#pytorch)
* [VPoser](#vposer)
* [Homogenus](#homogenus)
* [OpenPose](#openpose)

## SMPLify-x
[go up](#Table-of-contents)

Just clone SMPLify-x from [repo](https://github.com/vchoutas/smplify-x)

`git clone https://github.com/vchoutas/smplify-x.git`

## PyTorch
[go up](#Table-of-contents)

[Link to original](https://pytorch.org/get-started/locally/)

At first we need to install `conda`, just use [this](https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html) guide.

`conda install pytorch torchvision cudatoolkit=10.2 -c pytorch`

## VPoser
[go up](#Table-of-contents)

[Link to original](https://github.com/nghorbani/human_body_prior)

**Requirements**
- Python 3.7
- [Configer](https://github.com/nghorbani/configer)
- [PyTorch 1.1.0](https://pytorch.org/get-started/previous-versions/)
- [Torchgeometry 0.1.2](https://pypi.org/project/torchgeometry/0.1.2/)
- [Pyrender](https://pyrender.readthedocs.io/en/latest/install/index.html#osmesa) for visualizations

Install from this repository for the latest developments:
```bash
pip install git+https://github.com/nghorbani/configer
pip install git+https://github.com/nghorbani/human_body_prior
```

**Optional dependencies:**

If you want to use the feature to [Disentangle Self-Intersecting Poses](https://github.com/nghorbani/human_body_prior/tree/master/human_body_prior/body_model#disentangling-self-intersecting-novel-poses)
please install the optional package [mesh_intersection](https://github.com/vchoutas/torch-mesh-isect).

### Loading Trained Models

To download the trained *VPoser*  models go to the [SMPL-X project website](https://smpl-x.is.tue.mpg.de/) and register to get access to the downloads section. Afterwards, you can follow the [model loading tutorial](notebooks/vposer_poZ.ipynb) to load and use your trained VPoser models.

### Train VPoser
We train VPoser, using a [variational autoencoder](https://arxiv.org/abs/1312.6114)
that learns a latent representation of human pose and regularizes the distribution of the latent code to be a normal distribution.
We train our prior on data from the [AMASS](https://amass.is.tue.mpg.de/) dataset; 
specifically, the SMPL pose parameters of various publicly available human motion capture datasets. 
You can follow the [data preparation tutorial](human_body_prior/data/README.md) to learn how to download and prepare AMASS for VPoser.
Afterwards, you can [train VPoser from scratch](human_body_prior/train/README.md). 

## SMPL-X
[go up](#Table-of-contents)

[Link to original](https://github.com/vchoutas/smplx)

To install the model please follow the next steps in the specified order:

To install from PyPi simply run:

`pip install smplx[all]`

Clone this repository and install it using the `setup.py` script:

`git clone https://github.com/vchoutas/smplx
python setup.py install
`

To download the SMPL-X model go to [this project website](https://smpl-x.is.tue.mpg.de/) and register to get access to the downloads section.

To download the SMPL+H model go to [this project website](http://mano.is.tue.mpg.de/) and register to get access to the downloads section.

To download the SMPL model go to [this](http://smpl.is.tue.mpg.de/) (male and female models) and [this](http://smplify.is.tue.mpg.de/) (gender neutral model) project website and register to get access to the downloads section.

## OpenPose
[go up](#Table-of-contents)

To install openPose from docker image:
1. [Install nvidia-container-toolkit](https://github.com/NVIDIA/nvidia-docker#ubuntu-160418042004-debian-jessiestretchbuster)
2. [Install nvidia-docker 2.0](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0))

To run the container, use the following commmand (Ubuntu):
At first run, docker will download an image.
```bash
sudo docker run -it -v /home/dimon/openposedata/:/openpose/data/ --net=host -e DISPLAY --runtime=nvidia exsidius/openpose
```
> If we want sharing files between your system and docker container (for example, processing any media from filesystem), we need to `-v /home/dimon/openposedata/:/openpose/data/`.
> Where:
> * `/home/dimon/openposedata/` is a path to your folder, that you need get access inside docker container.
> * `/openpose/data/` is a path inside docker container, linked to your real folder `/home/dimon/openposedata/`

Than docker container was started, we can processing images:
```bash
 ./build/examples/openpose/openpose.bin --image_dir ./data --display 0 --write_json ./data/result --write_images ./data/result --face --hand

```

> OpenPose поддерживает 2 типа модели данных COCO (устар. но используется в проекте Videoavatars) и MODEL_25 (кажется, 25. используется в SMPL-X). Поэтому, если требуется COCO, то его необходимо скачать отдельно. И т.к. мы используем контейнер, нам необходимо его изменить сохранить изменения в новый образ.

Here is the steps, to download COCO model, and save docker image ([how-to-commit-changes-to-docker-image](https://phoenixnap.com/kb/how-to-commit-changes-to-docker-image)):
* run container: `docker run -it --runtime=nvidia exsidius/openpose`
* install wget: `apt-get install wget`
* go to folder, and download COCO-model:
```
cd models/pose/coco/
wget https://github.com/foss-for-synopsys-dwc-arc-processors/synopsys-caffe-models/raw/master/caffe_models/openpose/caffe_model/pose_iter_440000.caffemodel
```
Как только вы закончите модифицировать новый контейнер, выйдите из него:

`exit`

Предложите системе отобразить список запущенных контейнеров:

`sudo docker ps -a`

Вам потребуется **идентификатор КОНТЕЙНЕРА**, чтобы сохранить изменения, внесенные в существующее изображение. Скопируйте значение идентификатора из вывода.

Наконец, создайте новое изображение, зафиксировав изменения, используя следующий синтаксис:

`sudo docker commit [CONTAINER_ID] [new_image_name]`

Поэтому в нашем примере это будет:

`sudo docker commit c0d4c6800b5d openpose`

Где *c0d4c6800b5d* находится **идентификатор КОНТЕЙНЕРА** и **openpose** имя нового Image.

Ваше вновь созданное изображение теперь должно быть доступно в списке локальных изображений. Вы можете проверить, проверив список изображений снова:

`sudo docker images`

[go up](#Table-of-contents)

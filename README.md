# AnimalRecognitionDemo

This demo combines several [Redis](https://redis.io) data structures and [Redis Modules](https://redis.io/topics/modules-intro) to process a stream of images and filter out the images that contain cats.

It uses:

* Redis Streams to capture the input video stream: `all`
* [RedisGears](https://oss.redislabs.com/redisgears/) to process this stream
* [RedisAI](https://oss.redislabs.com/redisai/) to classify the images with MobilenetV2

It forwards the images that contain cats to a stream: `cats`

## Architecture
![Architecture](/architecture.png)

## Requirements
Docker and Python 2

## Running the Demo
To run the demo:

```
$ git clone https://github.com/RedisGears/AnimalRecognitionDemo.git
$ cd AnimalRecognitionDemo
```
You will find that very less files were able to check out. To fix this:

```
brew install git-lfs
```

```
ajeetraina@Ajeets-MacBook-Pro AnimalRecognitionDemo % git restore --source=HEAD :/
ajeetraina@Ajeets-MacBook-Pro AnimalRecognitionDemo % ls
LICENSE			README.md		architecture.png	docker-compose.yaml	redis
Makefile		app			camera			frontend		tests
ajeetraina@Ajeets-MacBook-Pro AnimalRecognitionDemo % git lfs install && git lfs fetch && git lfs checkout
Updated git hooks.
Git LFS initialized.
fetch: Fetching reference refs/heads/master
Checking out LFS objects: 100% (1/1), 24 MB | 0 B/s, done.
```

```
$ docker-compose up
```
If something went wrong, e.g. you skipped installing git-lfs, you need to force docker-compose to rebuild the containers
```
$ docker-compose up --force-recreate --build
```
Open a second terminal for the video capturing:

```
ajeetraina@Ajeets-MacBook-Pro camera % curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 1841k  100 1841k    0     0  2321k      0 --:--:-- --:--:-- --:--:-- 2321k
ajeetraina@Ajeets-MacBook-Pro camera % python3 get-pip.py
/usr/local/lib/python3.8/site-packages/setuptools/distutils_patch.py:25: UserWarning: Distutils was imported before Setuptools. This usage is discouraged and may exhibit undesirable behaviors or errors. Please use Setuptools' objects directly or at least import Setuptools first.
  warnings.warn(
Collecting pip
  Downloading pip-20.2.3-py2.py3-none-any.whl (1.5 MB)
     |████████████████████████████████| 1.5 MB 1.9 MB/s 
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 20.1.1
    Uninstalling pip-20.1.1:
      Successfully uninstalled pip-20.1.1
Successfully installed pip-20.2.3
ajeetraina@Ajeets-MacBook-Pro camera %
```

```
$ pip install -r camera/requirements.txt
$ python camera/read_camera.py
```

## UI
* `http://localhost:3000` shows all the captured frames
* `http://localhost:3001` shows only the framse with cats

## Limitations
This demo is designed to be easy to setup, so it relies heavily on docker.
You can get better performance and a higher FPS by runninng this demo outside docker.
To control the FPS, edit the [gear.py](https://github.com/RedisGears/AnimalRecognitionDemo/blob/master/app/gear.py#L53) file.

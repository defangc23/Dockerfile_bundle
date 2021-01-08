## Dockerfiles for Deeplearning Environment 

Images: https://hub.docker.com/r/eum814/deep_env

*Deep learning hand made recipe*. Combined with different env ingredients and useful tools such as  

- [x] Cuda
- [x] Cudnn
- [x] Python (numpy scikit-learn pandas rabbitMQ...)
- [x] Tensorflow
- [x] Pytorch
- [x] keras
- [x] OpenCV (ffmpeg support)
- [x] SSH remote access and debug

> Latest (Tag) image with dockerfile are created from CI autobuild for experimental usage, stable release version is referenced with docker tag v. (e.g.   *deepenv:v1.0* )  



#### NOTE:

- To activate SSH remote function, please first publish the available port on the Docker host and map it to port 22 in the container (e.g. `-p 10022:22`).  Then add command `/etc/init.d/ssh restart` when running the image. And you can test the internal container connection with root user by tapping `ssh root@docker-host-ip -p 10022`  The user password can refer to the  ssh configurations in Dockerfile.


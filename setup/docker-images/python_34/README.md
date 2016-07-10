docker build --tag teamlab/ml_python_tensor:0.1 .
docker run -it -p 8888:8888 -p 6006:6006 -p 22:22 --volume /D/workspace/:/notebooks teamlab/ml_python_tensor:0.1
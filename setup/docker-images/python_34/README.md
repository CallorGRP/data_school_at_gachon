## Dockerfile for Pydata Eco Systems with Tensorflow
본 Dockerfile은 가천대 산업경영공학과 데이터 과학 여름 학교의 강의용으로 제작되었습니다.

### Dockerfile Overview
- [Dockerfile for pydata with tensorflow](Dockerfile)
- Docker 설치 방법
    - [Windows 10에서 Docker for Windows 설치](http://bryan7.tistory.com/774)(2016, 강희석)
    - Docker 설치 가이드 영상 - 제작예정
- 이미지 생성 명령어
> ```bash
> docker build --tag <이미지명>:<빌드번호> .
> ```
- 이미지 생성 명령어
> ```bash
> docker build --tag teamlab/pydata-tensorflow:0.1
> ```
- 컨테이너 실행 (내 PC의 폴더를 지정)
> ```bash
> docker run -it -p 8888:8888 -p 6006:6006 --volume /<하드드라이브알파벳>/<공유할폴더명>/:/notebooks teamlab/pydata-tensorflow:0.1
> ```

- 컨테이너 실행 예시
> ```bash
> docker run -it -p 8888:8888 -p 6006:6006 --volume /D/workspace/:/notebooks teamlab/pydata-tensorflow:0.1
> ```

### Objectives
본 파일은 아래와 같은 목적을 가집니다.
- Window 상에서 Docker를 활용하여 Tensorflow와 Jupyter를 활용
- Tenrsoflow외에도 데이터 분석을 위해 사용하는 다양한 파이썬 패키지를 설치함

### Packages
본 파일은 다음의 패키지를 Docker 이미지에 설치합니다.
> 버전은 따로 지정되지 않음
- Numpy, Scipy
- Pandas
- Jupyter
- Scikit-learn
- Gensim
- BeautifulSoup4
- Tensorflow

### Issues
- matplotlib의 그래프를 inline으로 출력하고 저장시 저장불가 (원인파악중)
- matplotlib의 그래프는 실행후 결과를 확인하고 output clean을 한 후 저장 가능

### Versions
- 0.1: First open version to the public

### acknowledgement
- This dockerfile is modified from https://hub.docker.com/r/pdonorio/py3dataconda/~/dockerfile/

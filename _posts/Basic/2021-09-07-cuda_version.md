---

title: Downgrade CUDA

"date": "2021/09/07 19:32"

"categories": 
- basic
tags:
- basic
- CUDA
"toc": true
"toc_sticky": true

---





# cuda 버전 변경 



## 1. 버전별 호환성 확인 

이 곳에 cuda 버전을 변경하는 방법에 대해서 기록해 놓으려 한다. 



pytorch스터디를 진행하면서 노트북에 pytorch를 설치해야 했다. 하지만 pytorch 공식 홈페이지에서 설치하려하니 호환되는 cuda 버전이 11.1까지였고 내 노트북에는 11.4가 설치되어있었다. 따라서 cuda의 버전을 downgrade해야했고 그 과정을 여기에 기록하려 한다. 



**버전확인 방법**

```bash
# 그래픽 드라이버 버전확인 
nvidia-smi

#cuda 버전확인 
nvcc --version 

```



참고로 
cuda는 두개의  API를 가진다고 한다. 하나는 드라이버에 필요한 cuda 이며 나머지 하나는 런타임 구동에 필요한 cuda라고 한다. `nvidia-smi`입력시 나오는 cuda가 드라이버에 필요한 cuda 이며 이는 그래픽드라이버가 설치될 때 자동으로 같이 설치되는 것이고 우리가 모델을 돌릴때 사용되는  cuda 는 `nvcc --version`을 통해 출력되는 버전이라고 한다. 
그러므로 두 명령어로 출력되는 cuda의 버전이 달라도 걱정 노노! 



cuda의 버전을 변경하기 전에 변경할 버전이 현재 그래픽 드라이버에 호환가능한지의 여부를 살펴보아야 한다. 

![Screenshot from 2021-09-07 18-39-17](https://user-images.githubusercontent.com/77032455/132322753-ff01d2c8-4495-43ee-bf2f-43cf04910cee.png)버전

참고 : https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html 



cuda 와 그래픽드라이버와의 호환성은 위 표에서 확인할 수 있다. 
내가 설치하기 원하는 cuda 버전은 11.1이었고 그래픽드라이버의 버전이 455이상이면 호환이 되었다. 사용중인 그래픽드라이버의 버전은 470이었으므로 가능!! 

버전의 호환성 문제가 없음을 확인했으니 이제부터 cuda의 버전을 변경해 보자!!





## 2. 기존의 cuda 삭제 



기존의 설치된 cuda를 아래 코드를 이용하여 제거한다. 

```shell
$ sudo apt-get purge cuda* && sudo apt-get autoremove && sudo apt-get autoclean && sudo rm -rf /usr/local/cuda*
```



이후 재부팅한다. 

```shell
$ sudo reboot
```





## 3. 새로운 버전의 cuda 다운로드 



재부팅이 완료되면 https://developer.nvidia.com/cuda-toolkit-archive 에서 원하는 cuda 버전을 선택한 뒤 사양에 맞게 설정을 해준다. 



![Screenshot from 2021-09-07 18-50-26](https://user-images.githubusercontent.com/77032455/132324420-100f820c-c6e6-46cd-8a4d-78e5365a2e00.png)



그러면 아래에 설치 명령어가 제공된다. 

![Screenshot from 2021-09-07 18-52-43](https://user-images.githubusercontent.com/77032455/132324773-d641ffe9-f93e-461e-8ed6-900d4a742d8c.png)

위 코드를 터미널에 고대로 입력해 준다. 

이때, 버전을 바꾸어 설치를 해주어도 예전 버전이 설치되는 경우가 있다. apt list에 이전 버전이 존재하고 그 버전을 우선적으로 인식해서 설치하기 때문에 발생하는 문제라고 한다. 

이러한 경우에 위 명령에 에서 마지막 명령어만 버전을 명시하여 입력해주면 된다. 

```shell
# cuda 버전에 맞게 수정하여 입력! 
$ sudo apt-get install cuda-11-1 
```



설치가 완료되면 다시한번 **재부팅**해준다. 

이후에 `nvcc --version`을 통해 설치된 cuda의 버전을 확인해 주자.



만약 cuda 를 설치하고 `nvcc --version`을 입력했는데 nvcc가 없다며 문제가 생길경우 아래 설명을 따라하자.



```shell
$ sudo gedit ~/.profile
```

위 코드를 입력하여 파일을 열고 설치한 cuda의 변경사항을 적용하여야 한다.

열린 gedit파일 맨 아래에 두 줄만 추가해 주면 된다.

``` sh
export PATH=/usr/local/cuda-11.1/vin:PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.1/lib64:$LD_LIBRARY_PATH
```

 입력하고 저장하고 다시  `nvcc --version`을 입력하여 제대로 출력되는지 확인해 보자!





## 4.  cudnn 설치 



cudnn은 딥러닝 가속기라고 흔히 말하는데 딥러닝 모델의 학습시 속도면에서 이 cudnn의 유무가 엄청나다고 한다.

쨋든 cuda의 버전에 맞추어 설치해 주어야 하는 것이 cudnn이기 때문에 cudnn도 재설치 해준다. 

cudnn의 경우 cuda 의 디렉토리안에 설치해 주기 때문에 기존의 cudnn은 cuda를 삭제하는 과정에서 같이 삭제되었다고 보면된다. 



**cudnn버전 확인**

```shell
# 8.x 이전 버전
$ cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
# 8.x 이후 버전
$ cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
```



https://developer.nvidia.com/rdp/cudnn-archive
( 계정이 있어야 하고 다운받으려면 간단한 설문조사 해야됨 )

위 링크에서 설치한 cuda 버전에 맞는 cudnn을 다운로드 하여야 한다. 
상단에 있는 **cuDNN Library for Linux (x86_64)**을 다운받으면 된다. 

다운로드가 완료되면 압축해제한다. 

```shell
$ tar -xvf 방금_받은_파일.tgz
```



이후에 아래를 명령어대로 파일을 옮겨(복사해)준다.

```shell
# include내부의 파일을 cuda폴더로 옮겨준다. 
$ sudo cp Downloads/cuda/include/* /usr/local/cuda-11.1/incude

# 라이브러리 파일을 복사해 준다. 
$ sudo cp -P Downloads/cuda/lib64/* /usr/local/cuda-11.1/lib64

# 모든 사용자에게 라이브러리 실행권한 부여 
$ sudo chmod a+r /usr/local/cuda-11.1/lib64/libcudnn*
```



이후 cudnn의 버전을 확인하여 정상적인 버전이 설치되었는지 확인해 보자. 





## 5. 발생할 수 있는 issue 



cuda 를 정상적으로 설치하고 nvidia-smi를 입력했을 경우에 

```shell
$ nvidia-smi

NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running.
```

 위 처럼 에러가 발생하였다. 

이러한 경우 보통 nvidia GPU 드라이버에 문제가 생기면 발생하는 문제라고 하는데 CUDA를 재설치 할 경우에도 발생하는가 보다. 



그래서 그래픽 드라이버를 재설치 해주자! 

먼저 기존의 그래픽드라이버를 제거해 준다.

```shell
$ sudo apt remove nvidia-driver-[버전]
$ sudo apt autoremove
```

이후에 똑같은 버전을 설치할 것이기 때문에 같은 버전으로 설치해 준다.

```shell
$ sudo apt-get install nvidia-driver-[버전]
$ sudo reboot
```

그래픽 드라이버를 설치하면 항상 재부팅을 해줘야 한다. 

이후 `nvidia-smi`를 입력하여 제대로 출력이 되는지 확인한다. 





## Reference 

https://cafepurple.tistory.com/39

https://yoonchang.tistory.com/15

https://docs.nvidia.com/deploy/cuda-compatibility/index.html

https://bluecolorsky.tistory.com/52

https://stackoverflow.com/questions/53422407/different-cuda-versions-shown-by-nvcc-and-nvidia-smi

https://ghostweb.tistory.com/829
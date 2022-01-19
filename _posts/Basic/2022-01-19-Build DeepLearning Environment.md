---

title: Build DeepLearning Environment

"date": "2022/01/19 11:30"

"categories": 
- basic
tags:
- basic
- DeepLearning
"toc": true
"toc_sticky": true

---

  

# 딥러닝 환경 구축   

> 부팅usb를 통해 ubuntu를 설치하고 아무것도 하지 않은 상태 기준  

## 1. CUDA 설치   

```
$ sudo apt install nvidia-cuda-toolkit
$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Sun_Jul_28_19:07:16_PDT_2019
Cuda compilation tools, release 10.1, V10.1.243
```


## 2. Cudnn 설치   
[cudnn 설치 경로 ](https://developer.nvidia.com/rdp/cudnn-archive)에서 (1.)에서 설치한 CUDA의 버전에 맞게 Cudnn을 설치해 준다. (for Linux [x86]으로 설치 )

이후 아래 과정을 진행해 준다. 
 

1. 압축해제

```
$ tar -xvzf cudnn-10.1-linux-x64-v7.6.5.32.tgz
# 버전에 맞게 수정 
```

2. cuDNN을 CUDA와 연동  

```
$ sudo cp cuda/include/cudnn.h /usr/lib/cuda/include/
$ sudo cp cuda/lib64/libcudnn* /usr/lib/cuda/lib64/

# 권한설정
$ sudo chmod a+r /usr/lib/cuda/include/cudnn.h 
$ sudo chmod a+r /usr/lib/cuda/lib64/libcudnn*
```

3. 환경변수 등록     

```
$ sudo apt install vim 
$ sudo vim ~/.bashrc

맨 아래줄에 입력해줍니다.
#=================================================================
#CUDA ENV

export LD_LIBRARY_PATH=/usr/lib/cuda/lib64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/lib/cuda/include:$LD_LIBRARY_PATH

# ESC누른후 :(쉬프트 + 세미콜론)입력후 명령어 입력!
# :w(저장)
# :q(vi 종료) 
```  
  
## 3. nvidia driver 설치  
 

```
$ ubuntu-drivers devices
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00001F10sv00001043sd00001FFEbc03sc00i00
vendor   : NVIDIA Corporation
model    : TU106M [GeForce RTX 2070 Mobile]
driver   : nvidia-driver-460-server - distro non-free
driver   : nvidia-driver-460 - distro non-free
driver   : nvidia-driver-495 - distro non-free
driver   : nvidia-driver-418-server - distro non-free
driver   : nvidia-driver-470-server - distro non-free
driver   : nvidia-driver-450-server - distro non-free
driver   : nvidia-driver-470 - distro non-free recommended
driver   : xserver-xorg-video-nouveau - distro free builtin
```
위 결과를 보면 470버전이 recommended로 나왔기 때문에 해당 버전을 설치해 준다. 

```
$ sudo apt install nvidia-driver-470
$ sudo reboot
$ nvidia-smi
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 470.86       Driver Version: 470.86       CUDA Version: 11.4     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0  On |                  N/A |
| N/A   61C    P2    63W /  N/A |   7917MiB /  7982MiB |     33%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
```

## 5. anaconda / VScode 설치 

https://www.anaconda.com/products/individual#windows  
위 링크에서 anaconda를 설치하고   
https://code.visualstudio.com/download  
위 링크에서 VScode를 설치한다.     

VScode가 설치되면 python Extension을 설치한다. 

![Screenshot from 2022-01-19 10-49-40](https://user-images.githubusercontent.com/77032455/150048493-3f883d1d-40fb-4c05-9242-5c56fe6117bf.png)

**위 3개 설치!**

생성된 가상환경과 연결해 주기 위해 
[control + shift + p] 입력후 생성된 가상환경을 클릭해 준다. 

[가상환경 설치 방법](https://qpwoei1222.tistory.com/5)








## reference 
https://opentutorials.org/course/730/4561  
https://dolgogae.tistory.com/m/2?category=937082  
https://linuxhint.com/fix-vim-command-not-found-error-in-ubuntu/  
https://taeguu.tistory.com/72  

---
layout: post
title:  "윈도우에서 리눅스 환경 설치하기 (WSL)"
date:   2020-09-01 10:30:00
categories: Algorithm
tags : [알고리즘, 윈도우, 리눅스, windows, linux, WSL, Windows Subsystem for Linux]
comments: true
---

 리눅스 환경에서의 개발은 GUI 환경의 Windows에 비해 속도나, 단순성, 견고성에서 우위를 점한다. 
 윈도우에서 리눅스 명령어를 사용하려면, 여러가지 방법이 있지만 실제로 윈도우 환경에서 리눅스와 호환이 되게 만들어주는 WSL(Windows Subsystem for Linux)를 구축해보고자 한다.
 설치환경은 Windows 10 이상에서만 가능하다.

 ---

## 설치하기  

 참고자료 : [WSL설치 가이드](https://docs.microsoft.com/ko-kr/windows/wsl/install-win10)  


#### 1. Linux용 Windows 하위 시스템 설치  

 Windows에서 "Linux용 Windows 하위 시스템" 옵션 기능을 사용하여야 한다.  
 PowerShell을 관리자 권한으로 열어 실행하여 다음과 같이 입력한다.

 ```PowerShell
 dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
 ```

 ---

#### 2. WSL 2로 업데이트  
 > `windows키 + R`에서 `winver`를 입력한 다음 windows의 버전이 버전 1903 이하, 빌드 18362 이하인 경우 Windows 버전을 최신으로 업데이트 한 후 진행합니다.
  
 WSL2를 설치하기 전에 **가상 머신 플랫폼** 옵션 기능을 사용하도록 설정한다. 아까와 같이 PowerShell을 열어 다음을 입력한다.

 ```PowerShell
 dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
 ```

 PC를 재부팅하고 PowerShell을 관리자 권한으로 열고 다음 명령을 실행하여 새 Linux 배포를 설치할 때 WSL2를 기본 버전으로 설정한다.

 ```PowerShell
 wsl --set-default-version 2
 ```

 ---

#### 3. Microsoft Store에 들어가 "Ubuntu"를 검색하여 설치한다.  
 현재 Ubuntu 20.04 LTS가 가장 최신이므로 이걸 다운로드하였다.
 ![WSL1](/assets/images/WSL1.png)

 기다리면 설치가 완료 후, username과 password를 설정하는 커맨드가 뜬다. 비밀번호는 2번 입력하고 잊어버리지 않도록 하자.
 ![WSL2](/assets/images/WSL2.png)

 ---

#### 4. 패키지 업데이트 및 설치
* 모든 패키지 업데이트
```
$ sudo apt update
$ sudo apt upgrade
```

* C 컴파일러 설치 (gcc)
```
$ sudo apt install gcc
```

---

#### 5-1. 리눅스와 윈도우 간 파일 접근 방법 1
* 리눅스 홈디렉토리 찾기  
파일탐색기를 이용하여 위치를 찾아서 바로가기를 만들어 두면 접근하기 편하다. 
 
```
C:\Users\[USERNAME]\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc\LocalState\rootfs\home\계정명
```

###### 윈도우에서 만든 파일이 리눅스에서 열리지 않을 때 (Permission denied)  
열고자하는 파일이름이 abcd.c 인 경우, chmod 명령어로 파일의 mode를 변경한다. 즉 읽기, 쓰기, 실행 권한을 부여하는 것이다.  
```
$ chmod +rw abcd.c
```
가급적 touch 명령어를 이용하여 리눅스에서 파일을 생성한 후에 윈도우에서 편집하여 사용한다.  
```
$ touch abcd.c
```

---

#### 5-2. 리눅스와 윈도우 간 파일 접근 방법 2
다른 방법으로 윈도우에 리눅스를 위한 작업공간(폴더)를 아예 만들어 놓는 방법도 있다.
> 예) D:\ubuntu_data
  
이렇게 만들어 두는 경우, 윈도우에서 리눅스로 어떻게 접근하는 방법은 다음과 같다.
###### 리눅스에서 윈도우 폴더 접근 경로 예시
``` 예
$ /mnt/d/ubuntu_data/
```
> * mnt : 마운트를 뜻한다  
* d : D드라이브를 뜻한다  
* ubuntu_data : 위에서 만들어 둔 폴더 이름  

위와 같은 경로로 리눅스에서 윈도우 폴더로 접근할 수 있게된다. 
위 경로는 심볼릭 링크를 생성하여 쉽게 사용할 수 있다. 

###### 심볼릭 링크 생성
``` 예
$ ln -s /mnt/d/ubuntu_data/ win_data
```
> * -s : 심볼릭 링크를 만든다는 뜻  
* /mnt/d/ubuntu_data/  : 리눅스에서 윈도우 폴더 접근 경로 대상  
* win_data : win_data라는 이름으로 링크를 걸겠다라는 뜻.

 ![WSL3](/assets/images/WSL3.png)
 
 링크가 잘 생성된 것을 확인할 수 있다.  
 이제 `cd win_data`라고만 쳐도 해당 폴더로 쉽게 넘나들 수 있다.

 ---

#### 6. 텍스트 편집기 및 폰트 설치 
어떤 걸 써도 무방하지만 프로그래밍 개발용으로 무료로 배포되는 프로그램을 소개한다.
###### [Notepad++](https://notepad-plus-plus.org/download)  

또한 가독성을 위하여 개발자용 폰트도 설치하자.
###### [개발자용 폰트 (D2Coding)](https://github.com/naver/d2codingfont/)

Notepad++ 에서 폰트 설정은
> 설정 > 스타일 설정 > Global Styles > Default Style > 글꼴 스타일

에서 변경할 수 있다.
![WSL4](/assets/images/WSL4.png)

---

이제 기본적인 리눅스 사용 환경설정은 끝났다.

최종수정일 2020.09.01



> ###### References
> 고려대학교 202R 알고리즘, 이도길

{% highlight ruby %}
print("hello, neighbors!")
print("Sungwon Kim © 2020 • All rights reserved.")
{% endhighlight %}

[Github][githuburl]

[githuburl]: https://github.com/kpiswon


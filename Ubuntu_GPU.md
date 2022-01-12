Ubuntu 설치
링크에서 Ubuntu 18.04 또는 Ubuntu 20.04 iso 파일을 다운로드
홀수 버전(17.04, 19.04, 21.04 등)은 LTS 버전이 아니기 때문에 업데이트가 지속적으로 이루어지지 않을 수 있으니 반드시 짝수 버전중 LTS로 지정된 버전을 다운 받는다.
LTS(Long Term Support) : 일반적인 경우보다 장기간 또는 정해진 기간까지 지원하도록 특별히 고안된 소프트웨어의 버전 또는 에디션
링크에서 universal usb installer를 다운로드 받고 앞서 다운받은 ubuntu iso 파일로 설치 usb를 만든다.
만든 ubuntu 설치 usb로 PC를 부팅하고 ubuntu를 설치한다.
설치 후 이후 설정을 위해 인터넷 연결 및 텍스트 편집기(vim, nano)를 설치한다.
주의사항
nomodeset
설치하고자 하는 PC 또는 서버의 CPU에 내장 GPU가 있다면 그래픽 드라이버간 충돌이 발생해 부팅이 되지 않는 경우가 있으며 아래 설명은 이를 해결하는 방법이다.
부팅 시 <그림 1>의 화면이 지나고 나서 black screen에서 깜빡이는 커서의 상태로 멈추거나 block~~만 적힌 상태로 멈춘다면 <그림 1>의 화면에서 e를 누른다.


<그림 1> Ubuntu 설치 USB로 부팅했을때 첫 화면

<그림 1>의 화면에서 e를 누르면 <그림 2>의 화면이 출력되는데 'quiet splash' 두 단어 사이 또는 끝에 nomodeset을 추가하고 Shift + F10을 눌러 설치를 시작한다.


<그림 2> nomodeset 설정 화면

Ubuntu 설치가 모두 완료되고 나서 부팅 로고가 지나고나서 멈춘다면 멈추기 직전 화면에서 ESC 또는 Shift를 눌러 동일하게 nomodeset을 추가하여 부팅 후 grub 설정을 변경하여 문제를 해결한다.
/etc/default/grub 파일에도 동일하게 수정한 후 아래 명령어로 적용한다.
$ sudo vim /etc/default/grub
  # in /etc/default/grub
  GRUB_DEFAULT=0
  GRUB_TIMEOUT_STYLE=hidden
  GRUB_TIMEOUT=0
  GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
  GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nomodeset"
  GRUB_CMDLINE_LINUX=""
$ sudo update-grub
$ sudo reboot
Nvidia-driver 설치
링크를 참고하여 nvidia-driver를 설치한다.
해당 문서의 가장 아래 있는 주의사항을 반드시 유의하여 설치를 진행해야 한다.
CUDA 설치
CUDA를 설치할 때는 링크를 참고하여 설치를 진행해야 하며, 권장 사항으로는 가장 최신 CUDA 라이브러리는 업데이트를 진행 중이기 때문에 버그가 있을 수 있으니 반드시 하나 낮은 버전을 설치한다.
11.5가 최신일 경우 11.4의 가장 최신 버전을 설치한다.
가장 최신 버전 외의 버전은 링크의 아카이브를 참고한다.
본 문서의 작성 날짜 기준으로는 11.5가 가장 최신이기 때문에 하나 낮은 버전인 11.4.3을 설치한다.
wget https://developer.download.nvidia.com/compute/cuda/11.4.3/local_installers/cuda_11.4.3_470.82.01_linux.run
sudo sh cuda_11.4.3_470.82.01_linux.run
nvcc 환경 변수 설정(선택사항)
nvcc가 필요할 경우 CUDA 설치 이후에 아래 명령어를 이용해 환경변수를 설정한다.
$ vim ~/.bashrc
  # Add to bottom line in ~/.bashrc
  export CUDA_HOME=/usr/local/cuda
  export LD_LIBRARY_PATH=${CUDA_HOME}/lib64
  PATH=${CUDA_HOME}/bin:${PATH}
  export PATH
$ source ~/.bashrc
Docker, Docker-compose, Nvidia-docker 설치
Docker.ce 설치 : 링크
Docker-compose 설치 : 링크
Nvidia-docker 설치 : 링크

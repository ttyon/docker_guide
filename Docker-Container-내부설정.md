[Docker 컨테이너로의 sftp 사용](https://m.blog.naver.com/PostView.nhn?blogId=alice_k106&logNo=220650722592&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F)

1. ```
   $ apt-get update
   $ apt-get install vim
   ```

2. ```
   $ apt-get install ssh
   ```

3. ssh 키 생성

   ```
   $ cd ~/
   $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_dsa
   ```

4. sshd를 위한 폴더 생성

   ```
   $ mkdir /var/run/sshd
   ```

5. ~/.bashrc 파일에 다음을 추가

   ```shell
   # autorun
   /usr/sbin/sshd
   ```

6. 변경된 사항 적용

   ```
   $ source ~/.bashrc
   ```

7. (일반) 유저 추가

   ```
   $ adduser (유저명)
   ```
   
   7-1. 루트 계정 비밀번호 수정

   ```
   $ passwd
   ```
   
   7-2. ssh 루트 계정 접속제한 해제하기
   ```
   $ vi /etc/ssh/sshd_config
   ```
   ```
   # PermitRootLogin without-password 을 아래로 변경
   PermitRootLogin yes
   ```
   ```
   $ /etc/init.d/ssh restart
   ```

8. FileZilla 접속

   sftp://().sogang.ac.kr 로 접속

# 참고자료

- [가장 빨리 만나는 도커(Docker)](http://pyrasis.com/private/2014/11/30/publish-docker-for-the-really-impatient-book)
- [초보를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
- [컨테이너 기반 가상화 플랫폼 ‘도커(Doker)’의 이해](https://tacademy.sktechx.com/live/player/onlineLectureDetail.action?seq=125)
- [Docker 설치문서](https://github.com/sogang-mm/lab/wiki/Docker-%EC%84%A4%EC%B9%98-%EB%AC%B8%EC%84%9C) 
- [66. [Docker] Docker 컨테이너로의 sftp 사용 - filezilla를 통한 예제](https://m.blog.naver.com/PostView.nhn?blogId=alice_k106&logNo=220650722592&proxyReferer=https%3A%2F%2Fwww.google.com%2F) 
- [ssh 루트(root) 접속 제한(Permission denied) 해제하기](http://blog.naver.com/PostView.nhn?blogId=chandong83&logNo=220919303234&categoryNo=16&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=section)

1. Deployment 설정
2. Interpreter 설정
3. 프로젝트 연결 설정



## 1. Deployment 설정

1. FILE → Setting 들어가기
2. Build, Execution, Deployment → Deployment 들어가기
3. ssh 서버 등록하기 (이미 등록한 서버 이용 시에는 목록의 서버들 중 하나를 택함)
   1. ':heavy_plus_sign:' 버튼을 눌러 서버를 추가한 후,
   2. Connection: SFTP host, Port, Root path(root 계정일 경우 경로를 '/'로 설정 권장) 및 나머지 설정
   3. Mapping: Local path(로컬에서 프로젝트 경로), Deployment path on server '(계정)@(서버호스트):(포트번호)'(서버에서 개발 프로젝트 경로) 설정
   4. Apply

## 2. Interpreter 설정

1. FILE → Setting 들어가기 (이전과 동일)

2. Project: (프로젝트 명) → Project Interpreter 들어가기

3. ':gear:' 버튼 누른 후, 바로 인터프리터 추가 시 'Add...', 기존의 인터프리터 목록을 보고싶을 땐 'Show All...' 클릭

4. 인터프리터 추가하기

   1. Add Python Interpreter 창에서 SSH Interpreter 들어가기

   2. 새 서버 등록시 New server configuration에서 요구하는 정보 입력, 이미 등록된 서버를 이용 시 Existing server configuration에서 등록된 서버 선택 후 Next

   3. 서버에서 사용하고 싶은 인터프리터 위치를 입력

   4. 서버에서 3번의 인터프리터를 이용해 실행시키려는 프로젝트 위치 입력 

      :star2:중요:star2:**<project root>→/tmp/pycharm_project_488 = (1.3.2에서 설정한 root path)/tmp/pycharm_project_488** 이므로 서버에서 이미 만들어진 프로젝트 경로가 있다면 다시 설정해야한다!

   5. Finish

### 인터프리터 위치 확인하기 (python)

```python
import sys

print(sys.executable)
```



## 3. 프로젝트 연결 설정

### 파일 동기화

1. Tools → Deployment에서 파일 Upload 및 Download
2. 혹은 Remote Host 창을 열어 선택한 파일에 오른쪽 클릭 후 다운로드

### 파일 자동 업로드 설정

1. Tools → Deployment에서 파일 자동 업로드 설정: Automatic Upload(always) 체크 설정 및 Options...



## 그 외

- Getting Access to the Remote Host Tool Window: View → Tool Windows → Remote Host



# 참고자료

- [PyCharm SSH 연결하기](https://simonjisu.github.io/datascience/2018/06/24/pycharmssh.html)
- [Pycharm 원격 서버 연결하기](https://pytogether.tistory.com/1)


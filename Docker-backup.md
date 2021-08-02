Docker backup 
```
$ sudo docker ps -a | grep oracle // Oracle 컨테이너가 존재하는지 확인
$ sudo docker commit oracle oracle_backup  // oracle 컨테이너를  'oracle_backup' 이라는 이미지로 저장
$ sudo docker save oracle_backup > /backup/oracle_xx.tar  // 'oracle_xx.tar'  라는 이름으로 백업
``` 
ex)
``` 
docker commit mj vwii_2_backup
sudo docker save -o /home/mmlab/hdd2/mj/backup/vwii_2_backup.tar vwii_2_backup
``` 
Docker load
``` 
$ sudo docker load < /backup/oracle_xx.tar  // oracle_xx.tar 로 부터 oracle_backup 이미지 복원
$ docker iamges | grep oracle // 이미지가 복원되었는지 확인
``` 
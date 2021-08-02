## 세팅의 기본적 이해
1. Apache에서 Port를 Listen 할 수 있도록 해준다.
2. VirtualHost를 이용하여 별도의 사이트로 세팅한다. (별도 파일로 세팅하는 것을 권장)
3. WSGIDameonProcess를 세팅한다. (권장)
4. python-home은 venv 경로, python-path는 프로젝트 경로로 세팅한다. (venv가 필요)
5. WSGIScriptAlias로 프로젝트/site/wsgi.py를 설정한다. (url resolve를 django에서 담당한다.)
6. static, upload, media, video 등의 static file들은 별도로 Alias를 지정하여야 한다.
(지정하지 않아도 wsgi.py를 통해 접근 가능하나, 서버의 성능이 저하된다. 특히 video 파일은 별도로 지정하여 apache에서 직접 제공하지 않을 경우 seek가 불가능하다.)
7. 업로드나 경로 변경 등으로 파일을 엑세스하는 경우, chown을 이용하여 www-data 그룹으로 지정할 필요가 있다.
(permision deny가 발생한다.)

### 참고사항
apache2를 통한 video seek는 http range를 통해 제공되는 것으로, pseudo streaming의 일종이다.
동영상의 패킷 구조를 알아야 요청-응답이 가능하기 때문에, 일반적으로 ffmpeg 계열의 plug-in이 필요하다.
다만, html5에서 공식 지원하는 웹비디오(h.264)의 경우는 별도의 plug-in 없이 http range만으로 psuedo streaming이 가능하다.
동영상이 웹비디오에 적합하게 인코딩 되었는지 확인하는 방법은 크롬 브라우저에 해당 동영상을 drag&drop으로 플레이 가능 여부를 통해 쉽게 알 수 있다.

Apache에서 제공할 때에는 STATIC_ROOT를 지정하여 ./manage.py collectstatic을 사용하는 것을 권장한다.

## 환경 기준
Ubuntu 16.04 + Apache2 기준

## 세팅 방법
### apache2 포트 오픈
/etc/apache2/port.conf
Listen 8000 추가

### VirtualHost (site.conf)파일 작성

### /etc/apache2/sites-available로 설정 파일 복사

### mp4 MIME 등록
/etc/apache2/mods-available/mime.conf에 다음의 설정을 추가

        # Add for Video
        AddType video/mp4 .mp4 .m4v
        AddType video/ogg .ogv
        AddType video/webm .webm



### site 추가

`$ sudo a2ensite site.conf`

`$ sudo sevice apache2 reload`
## Dockerfile x Nginx 구성 및 실행

### 목적

```
Dockerfile을 통해 Nginx 구성하고 이미지 생성 및
컨테이너를 올려 구성한 html파일을 띄우기
```

### 프로젝트 구조

- Dockerfile : 해당 파일을 실행시켜 레이어를 구성하고 Nginx 이미지 받아 재구성에 사용
- config : 설정 파일 관리 디렉토리
- config/nginx.conf : Nginx 실행을 위한 환경설정 파일
- html : html 파일 관리하는 디렉토리
- html/index.html : 실행될 파일

### Dockerfile

```
FROM nginx
```

- Docker hub에 있는 컨테이너 이미지 중 Nginx 이미지를 기반으로 새롭게 구성할 Docker 이미지 생성

```
COPY ./config/nginx.conf /etc/nginx/conf.d/nginx.conf
```

- 새로운 Nginx 설정 파일(내가 구성한 설정 파일)을 Nginx 이미지 경로 /etc/nginx/conf.d/nginx.conf에 복사

```
COPY ./html/index.html /usr/share/nginx/html/index.html
```

- html 파일을 Nginx 이미지 경로 /usr/share/nginx/html/index.html에 복사

```
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

- Docker 컨테이너가 실행되면 자동으로 실행되는 명령어

### nginx.conf

```
listen 80;
```

- Nginx가 80번 포트에서 클라이언트 요청을 받음

```
server_name localhost;
```

- 웹 서버 호스트 이름 설정

```
access_log /var/log/nginx/host.access.log main;
```

- 접근 로그의 위치와 로그 포맷 지정

```
location / { root /usr/share/nginx/html; index index.html index.htm; }
```

- 클라이언트로부터 root URL에 대한 요청이 오면, /usr/share/nginx/html 디렉토리에서 index.html or index.htm 파일을 찾아 반환

### 실행

1. 컨테이너 이미지 생성

```
docker build -t nginx:simple-nginx .
```

- Docker 빌드해 nginx의 태그를 <code>simple-nginx</code>로 지정하여 수행

2. 컨테이너 생성 및 실행

```
docker run -d -p 8080:80 --name simple-nginx nginx:simple-nginx
```

- <code>simple-nginx</code>로 컨테이너 이름 지정

3. 로컬 서버 접속하기

- <code>http:localhost:8080</code>로 html 출력 확인

### 출처

https://adjh54.tistory.com/414

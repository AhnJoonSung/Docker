# Docker
This project involves system administration using Docker. 
It entails virtualizing several Docker images and deploying them on virtual machines.

# General guidelines
이 프로젝트는 가상 머신에서 수행되어야 합니다.

• 프로젝트 구성에 필요한 모든 파일은 srcs 폴더에 위치해야 합니다.

• Makefile이 필요하며, 디렉토리의 루트에 위치해야 합니다. 
이는 전체 애플리케이션을 설정해야 합니다(즉, docker-compose.yml을 사용하여 Docker 이미지를 빌드해야 합니다).

• 이 과제는 여러분의 배경에 따라 아직 배우지 않았을 수 있는 개념을 실천에 옮기는 것을 요구합니다. 
따라서 Docker 사용법 관련 문서뿐만 아니라 이 과제를 완료하는 데 도움이 될 수 있는 모든 자료를 많이 읽는 것을 주저하지 말라고 조언합니다. 

# Rules
이 프로젝트는 특정 규칙에 따라 다양한 서비스로 구성된 소규모 인프라를 설정하는 것입니다. 전체 프로젝트는 가상 머신에서 수행해야 하며, Docker Compose를 사용해야 합니다.

각 Docker 이미지는 해당 서비스와 동일한 이름을 가져야 합니다.

각 서비스는 전용 컨테이너에서 실행되어야 합니다.

성능 문제를 고려하여, 컨테이너는 최신 안정 버전 바로 이전의 Alpine 또는 Debian을 기반으로 빌드되어야 합니다. 이 선택은 여러분에게 달려 있습니다.

각 서비스마다 Dockerfile을 작성해야 하며, 이 Dockerfile은 Makefile을 통해 docker-compose.yml에서 호출되어야 합니다.

즉, 프로젝트의 Docker 이미지를 직접 빌드해야 하며, 미리 만들어진 Docker 이미지를 가져오거나 DockerHub와 같은 서비스를 사용하는 것은 금지됩니다 (Alpine/Debian은 이 규칙에서 제외됩니다).


다음과 같은 설정이 필요합니다:

• TLSv1.2 또는 TLSv1.3만을 포함하는 NGINX가 있는 Docker 컨테이너

• NGINX 없이 WordPress + php-fpm(설치 및 구성 완료)이 있는 Docker 컨테이너

• NGINX 없이 MariaDB만 있는 Docker 컨테이너

• WordPress 데이터베이스를 포함하는 볼륨

• WordPress 웹사이트 파일을 포함하는 두 번째 볼륨

• 컨테이너 간 연결을 설정하는 docker-network


컨테이너는 충돌 시 자동으로 재시작되어야 합니다.

## Warning
도커 컨테이너는 가상 머신이 아닙니다. 

따라서, 실행 시 'tail -f' 등을 사용하는 임시방편적인 패치를 사용하는 것은 권장되지 않습니다. 

데몬이 어떻게 작동하는지, 그리고 그것을 사용하는 것이 좋은지에 대해 알아보세요.

물론, network: host나 --link 또는 links:를 사용하는 것은 금지됩니다.

네트워크 라인은 docker-compose.yml 파일에 반드시 존재해야 합니다.

컨테이너는 무한 루프를 실행하는 명령어로 시작되어서는 안 됩니다. 

이는 엔트리포인트로 사용되는 명령어 또는 엔트리포인트 스크립트에 사용되는 명령어에도 적용됩니다.

다음은 금지된 몇 가지 임시방편적인 패치 예시입니다: tail -f, bash, sleep infinity, while true.

PID 1에 대해 읽고 Dockerfile 작성시 최고의 관행을 알아보세요.

• WordPress 데이터베이스에는 관리자를 포함하여 두 명의 사용자가 있어야 합니다. 

관리자의 사용자 이름에는 admin/Admin 또는 administrator/Administrator(예: admin, administrator, Administrator, admin-123 등)을 포함할 수 없습니다.

볼륨은 Docker를 사용하는 호스트 머신의 /home/login/data 폴더에서 사용 가능해야 합니다. 물론, login을 당신의 로그인으로 교체해야 합니다.

일을 간단하게 만들기 위해, 도메인 이름이 로컬 IP 주소를 가리키도록 설정해야 합니다.

이 도메인 이름은 login.42.fr이어야 합니다. 다시 말하지만, 당신의 로그인을 사용해야 합니다.

예를 들어, 당신의 로그인이 wil이라면, wil.42.fr은 wil의 웹사이트를 가리키는 IP 주소로 리다이렉트됩니다.

latest 태그는 금지됩니다.

Dockerfile에는 비밀번호가 있으면 안 됩니다.

환경 변수 사용은 필수입니다.

또한 환경 변수를 저장하기 위해 .env 파일을 사용하는 것이 강력히 권장됩니다. .env 파일은 srcs 디렉토리의 루트에 위치해야 합니다.

NGINX 컨테이너는 TLSv1.2 또는 TLSv1.3 프로토콜을 사용하여 포트 443만을 통해 인프라로의 유일한 진입점이어야 합니다.

명백한 보안상의 이유로, 모든 자격 증명, API 키, 환경 변수 등은 로컬의 `.env` 파일에 저장하고 git에 의해 무시되어야 합니다. 

공개적으로 저장된 자격 증명은 프로젝트 실패로 직결됩니다. 

# System Architecture
<img width="930" alt="image" src="https://github.com/AhnJoonSung/Docker/assets/53860803/06b6567c-97e8-4f57-bf1f-49b6bd72711d">

# Directory structure
```
$> ls -alR
total XX
drwxrwxr-x 3 wil wil 4096 avril 42 20:42 .
drwxrwxrwt 17 wil wil 4096 avril 42 20:42 ..
-rw-rw-r-- 1 wil wil XXXX avril 42 20:42 Makefile
drwxrwxr-x 3 wil wil 4096 avril 42 20:42 srcs
./srcs:
total XX
drwxrwxr-x 3 wil wil 4096 avril 42 20:42 .
drwxrwxr-x 3 wil wil 4096 avril 42 20:42 ..
-rw-rw-r-- 1 wil wil XXXX avril 42 20:42 docker-compose.yml
-rw-rw-r-- 1 wil wil XXXX avril 42 20:42 .env
drwxrwxr-x 5 wil wil 4096 avril 42 20:42 requirements
./srcs/requirements:
total XX
drwxrwxr-x 5 wil wil 4096 avril 42 20:42 .
drwxrwxr-x 3 wil wil 4096 avril 42 20:42 ..
drwxrwxr-x 4 wil wil 4096 avril 42 20:42 bonus
drwxrwxr-x 4 wil wil 4096 avril 42 20:42 mariadb
drwxrwxr-x 4 wil wil 4096 avril 42 20:42 nginx
drwxrwxr-x 4 wil wil 4096 avril 42 20:42 tools
drwxrwxr-x 4 wil wil 4096 avril 42 20:42 wordpress
./srcs/requirements/mariadb:
total XX
drwxrwxr-x 4 wil wil 4096 avril 42 20:45 .
drwxrwxr-x 5 wil wil 4096 avril 42 20:42 ..
drwxrwxr-x 2 wil wil 4096 avril 42 20:42 conf
-rw-rw-r-- 1 wil wil XXXX avril 42 20:42 Dockerfile
-rw-rw-r-- 1 wil wil XXXX avril 42 20:42 .dockerignore
drwxrwxr-x 2 wil wil 4096 avril 42 20:42 tools
[...]
./srcs/requirements/nginx:
total XX
drwxrwxr-x 4 wil wil 4096 avril 42 20:42 .
drwxrwxr-x 5 wil wil 4096 avril 42 20:42 ..
drwxrwxr-x 2 wil wil 4096 avril 42 20:42 conf
-rw-rw-r-- 1 wil wil XXXX avril 42 20:42 Dockerfile
-rw-rw-r-- 1 wil wil XXXX avril 42 20:42 .dockerignore
drwxrwxr-x 2 wil wil 4096 avril 42 20:42 tools
[...]
$> cat srcs/.env
DOMAIN_NAME=wil.42.fr
# certificates
CERTS_=./XXXXXXXXXXXX
# MYSQL SETUP
MYSQL_ROOT_PASSWORD=XXXXXXXXXXXX
MYSQL_USER=XXXXXXXXXXXX
MYSQL_PASSWORD=XXXXXXXXXXXX
[...]
$>
```

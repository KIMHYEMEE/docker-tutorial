#  Docker Docs

본 글은 다음 [링크](https://docs.docker.com/)의 글을 정리한 것입니다.


# Get started

## 1. Overview

**Container**
간단히 호스트 머신에서 발생하는 다른 프로세스와 분리되어 작동하는 sandboxed process

- 이미지의 실행가능한(runnable) 인스턴스. DockerAPI 나 CLI를 활용하여 생성, 시작, 중단, 이동, 삭제할 수 있다.
- 로컬머신, 가상머신, 클라우드에서 실행될 수 있다.
- Portable한 특징을 가진다. (어떤 OS에서도 작동할 수 있다.)
- 다른 컨테이너와 분리되어 있으며, 스스로의 SW, binaries, confiugration을 실행할 수 있다.

**Container Image**
컨테이너는 분리된 파일 시스템에서 작동하는데, 이때 파일시스템은 이미지를 통해 제공된다. 이미지는 파일시스템을 포함하고 있기 때문에, 어플리케이션을 실행하는데 필요한 모든 요소(dependencies, configurations, scripts, binaries, 환경변수, 실행을 위한 초기 명령 등)를 담고 있어야만 한다.

## 2. Containerize an application

**준비사항**
- Docker 설치
- Git client
- IDE (VS Code 추천)
- container와 image에 대한 개념적 이해

**App Container 실행해보기**
- 본 예시의 앱은 To Do List

1. 아래의 커맨드를 실행하여 repository를 clone

    ```
    git clone https://github.com/docker/getting-started.git
    ```
    - `getting-started/app` 내 package.json 파일과 spec, srt 디렉토리가 존재
    
2. app의 컨테이너 이미지 build
    - `Docker file`로 컨테이너 이미지 build
    - Docker file은 파일확장 없는 텍스트 기반의 파일로, Docker가 컨테이너 이미지를 생성하도록 지시하는 스크립트를 포함

    (1) `package.json`이 있는 `app` 디렉토리에 dockerfile 생성
    ```
    # 디렉토리 연결
    cd getting-started/app
    # Dockerfile 로 명명된 빈 파일 생성
    touch dockerfile # mac
    type nul > Dockerfile # windows
    ```
    (2) IDE를 활용하여 `dockerfile`에 아래의 내용을 입력
    ```
    # syntax=docker/dockerfile:1
   
    FROM node:18-alpine
    WORKDIR /app
    COPY . .
    RUN yarn install --production
    CMD ["node", "src/index.js"]
    EXPOSE 3000
    ```

    - `FROM`: 베이스 이미지 설정. `node:18-alpine`에서 이미지 빌드를 시작. 해당 이미지가 없는 경우 다운로드 됨
    - `WORKDIR`: -
    - `COPY`: -
    - `RUN`: 이미지 빌드 시, 함께 실행되서 베이스 이미지 위에 레이어를 쌓음
    - `CMD`: 이미지 실행 시, 처리되는 커맨드. 예를 들어 윈도우의 시작 프로그램
    - `EXPOSE`: 컨테이너의 포트 번호 (일종의 사용자 출입구)

    (3) app 디렉토리에서 다음의 커맨트를 활용하여 컨테이너 이미지 build
        
    - 이 때, 도커가 실행되어 있어야 함. 도커를 직접 실행시키거나 커맨드(sudo service docker start)를 입력하여 도커를 실행
    ```
    docker build -t getting-started /another-directory
    ```
    - `docker build`:
    - `-t`: tag. build 하는 이미지에 사람이 읽을 수 있는 이름을 붙여주는 것. 여기서는 getting-started 라고 명명함
    - `.`: 현재 디렉토리에서 dockerfile을 찾을 수 있다고 알려주는 것. 현재 디렉토리 내 다른 디렉토리(another-directory)에 dockerfile이 존재하는 경우, `.` 대산 상대 경로인 `./another-directory`를 입력

    (4) app container 실행
    - 이미지가 만들어졌기 때문에, 컨테이너 내 앱을 실행시킬 수 있음
    - 다음의 커맨드 실행 후, 브라우저를 통해 URL(http://localhost:3000)에 접속하여 작동 여부 테스트
    ```
    docker run -dp 3000:3000 getting-started
    ```
    - `-d`: 새로운 컨테이너를 백그라운드에서 detach 모드로 실행 (터미널에 로그가 발생하지 않음)
    - `-p`: 호스트포트 3000과 컨테이너포트 3000을 매핑(host-port:container-port). 매핑하지 않을 경우, 앱에 접근할 수 없음
        - 호스트포트 설정: host-port 및 URL의 포트 동일하게 설정
        - 컨테이너포트 설정: container-port, docker file 내 `EXPOSE container-port` 할당값 및 index.js 파일 내 `app.listen(container-port,~)`의 포트 동일하게 설정


## 3. Update the application

## 4. Persist the DB

## 5. Persist the DB

## 6. Use bind mounts

## 7. Multi-container apps

## 8. Use Docker Compose

## 9. Image-building best practices

## 10. What next?


# Language-specific guides - Python

## 1. Overview

## 2. Building images

## 3. Run containers

## 4. Develop your app

## 5. Configure CI/CD

## 6. Deploy your app
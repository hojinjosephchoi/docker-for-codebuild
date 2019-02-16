
* Run `docker build -t croquis/buildenv:1.0.0 .` to build Docker image locally

docker build -t croquis/buildenv:2.0.0 .

* To poke around in the image interactively, build it and run:
`docker run -it --entrypoint sh croquis/buildenv:1.0.0 -c bash`

* To let the Docker daemon start up in the container, build it and run:
`docker run -it --privileged croquis/buildenv:1.0.0 bash`


----

docker build --build-arg http_proxy=$$http_proxy --build-arg https_proxy=$$https_proxy --build-arg no_proxy=$$no_proxy -t croquis/buildenv:1.0.0 .


## ECR 에 이미지 올리는 방법
host (맥북)에서 aws ecr get-login 명령어로 docker 로그인 설정 명령어를 얻는다
```
aws ecr get-login --no-include-email --region ap-northeast-2 --profile aws1
```

docker login 명령어를 카피한 후 다시 터미널에 입력해서 로그인을 한다.
```
docker login -u AWS -p xxxxxx https://536772866969.dkr.ecr.ap-northeast-2.amazonaws.com
Login Succeeded
```

docker image 정보 확인
```
docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
croquis/buildenv    1.0.0               9802b0214b51        15 minutes ago      1.25GB
test/tag            latest              b2727ec03357        31 minutes ago      1.25GB
redis               latest              0f55cf3661e9        9 days ago          95MB
```

docker 특정이미지의 repository 정보를 ECR Repository URI로 변경
```
docker tag croquis/buildenv:1.0.0 536772866969.dkr.ecr.ap-northeast-2.amazonaws.com/hojin-ecr:latest

```


image 를 ECR repository로 업로드
```
docker push 536772866969.dkr.ecr.ap-northeast-2.amazonaws.com/hojin-ecr:latest
```

ECR 에 있는 image 를 dokcer host 에 내려 받으려면
```
docker pull 536772866969.dkr.ecr.ap-northeast-2.amazonaws.com/hojin-ecr:latest
```


[ECR + Codebuild 정책생성](https://docs.aws.amazon.com/ko_kr/codebuild/latest/userguide/sample-ecr.html)


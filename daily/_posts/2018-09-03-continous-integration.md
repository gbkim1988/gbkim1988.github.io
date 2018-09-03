---
title: Continuous Integration
key: 20180903
tags: CI Security Jenkins gitlab deployment
---

개발 환경(Windows)에서 gitlab 에 변경 사항을 저장 그리고 Jenkins 를 통해 deployment 를 하는 것을 테스트 한다.
단, gitlab 및 Jenkins 는 각각 docker 로 구성한다.

<!--more-->

**참조 링크**
- [Sample Link1]()
- [사내 docker 저장소(registry) 구축하기](http://www.kwangsiklee.com/2017/08/%EC%82%AC%EB%82%B4-docker-%EC%A0%80%EC%9E%A5%EC%86%8Cregistry-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0/)

# Intro

특정 언어 및 특정 프레임워크의 개발 환경을 구축이라기 보다는 지속적인 통합(Continuous Integration, CI)을 목적으로 구축한다. CI 는 버전 반영 및 빠른 배포에 적합하다.

## 구축 환경 정의

구축 환경을 아래와 같이 정의한다.

* Docker 기반의 Container 시스템
 * Jenkins Container
 * gitlab Container
 * etc ...
* Docker 호스트
 * Ubuntu 16.04

## docker 설치
우선 의식의 흐름대로 Docker 를 설치한다. 참조 링크를 활용해 docker-setup.sh_ 파일을 작성하여 설치하였다.

**참조 링크**
- [docker community edition install in Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-from-a-package)

docker-setup.sh_
```
# Set up the repository
sudo apt-get update

# Install packages to allow apt to use a repository over HTTPS
sudo apt-get install \
         apt-transport-https \
         ca-certificates \
         curl \
         software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify that you now have the key with the fingerprint.
# For example, 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.
sudo apt-key fingerprint 0EBFCD88

# Set up the stable repository.
# To install builds from `edge` or `test` repositories as well. add the word edge or test (or both) after the wrod stable in the commands below
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install docker-ce
```
※ 설치 중에 특이사항은 없었다.

### docker composer (?)

docker composer 가 필요한 이유는 단순히 container 를 설치하는 것 뿐만아니라 로그 수집의 위치등을 기입하여 한번에 설정하도록 함에 있다. 또한 플러그인 설치를 자동으로 수행함에도 그 의의가 있다.

**docker composer 의 쓰임(?)**
- 백업 경로 설정
- 로그 경로 설정
- 플러그인 설치 자동화
- WAS 세부적인 설정(Django 와 같은 것들은 말이지)

```
```
### docker registry

아래의 인용은 [도커 미러링](http://www.kwangsiklee.com/2017/08/%EC%82%AC%EB%82%B4-docker-%EC%A0%80%EC%9E%A5%EC%86%8Cregistry-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0/) 을 참조하였음.

> 또한 stackoverflow 글에서도 docker hub pull 기능과 docker push를 내부적으로 사용하기 위해서는 private registry와 mirror registry를 띄워야 한다고 의견이 모아지고 있다.

### Docker Swarm

- [Docker Swarm Mode Overview](https://docs.docker.com/engine/swarm/)

- Feature highlights
 - Cluster management integrated with Docker Engine
   - Docker Engine CLI 를 통해서 swarm 을 관리할 수 있다. 별도의 orchestration software 가 필요하지 않는다.
 - Decentralized design
   - you can build an entire swarm from a single disk image
 - Declarative service model
   -


## gitlab container 설치

자 이제 gitlab 컨테이너를 내려받아 설치할 차례이다. composer 를 사용할지는 아직 확실치 않으나 composer 파일을 구성하는 이유는 백업 등의 여러 복합적인 설정을 번거롭지 않게 한번에 처리하기 위함이다.





---

If you like the post, don't forget to give me a start :star2:.

<iframe src="https://ghbtns.com/github-btn.html?user=gbkim1988&repo=gbkim1988.github.io&type=star&count=true"  frameborder="0" scrolling="0" width="170px" height="20px"></iframe>

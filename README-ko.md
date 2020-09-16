# 쿠버네티스 문서화

[![Netlify Status](https://api.netlify.com/api/v1/badges/be93b718-a6df-402a-b4a4-855ba186c97d/deploy-status)](https://app.netlify.com/sites/kubernetes-io-master-staging/deploys) [![GitHub release](https://img.shields.io/github/release/kubernetes/website.svg)](https://github.com/kubernetes/website/releases/latest)

이 리포지터리는 [쿠버네티스 웹사이트와 문서](https://kubernetes.io)를 빌드하는데 필요한 자산을 포함합니다. 여러분이 기여를 원한다는 사실에 매우 기쁩니다!

# 본 리포지터리 사용하기

Hugo를 사용해서 로컬에서 웹사이트를 실행하거나, 컨테이너 런타임에서 실행할 수 있습니다. 컨테이너 런타임을 사용하기를 강하게 권장합니다. 실제 웹사이트와 배포 측면에서 일관성을 제공하기 때문입니다.

## 사전 준비

이 리포지터리를 사용하기 위해서는, 다음을 로컬에 설치할 필요가 있습니다.

- [yarn](https://yarnpkg.com/)
- [npm](https://www.npmjs.com/)
- [Go](https://golang.org/)
- [Hugo](https://gohugo.io/)
- [도커](https://www.docker.com/)와 같은 컨테이너 런타임

시작하기 전에, 위 의존 소프트웨어를 설치하세요. 리포지터리를 복제하고(clone) 해당 디렉터리에 진입하세요.

```
git clone https://github.com/kubernetes/website.git
cd website
```

쿠버네티스 웹사이트는 [Docsy Hugo 테마](https://github.com/google/docsy#readme)를 사용합니다. 웹사이트를 컨테이너에서 실행할 계획이라도, 서브모듈과 그 밖의 개발 의존 모듈을 내려받기를 강하게 권장합니다.

```
# 의존 모듈 설치
yarn

# Docsy 서브모듈 내려받기
git submodule update --init --recursive --depth 1
```

## 컨테이너로 웹사이트 실행하기

본 사이트를 컨테이너에서 빌드하려면, 다음을 실행해서 컨테이너 이미지를 빌드하고 실행합니다.

```
make container-image
make container-serve
```

브라우저에서 http://localhost:1313을 열어서 웹사이트를 보세요. 소스 파일에 변경을 가하면, Hugo가 웹사이트를 갱신하고 브라우저에서 새로고침이 되도록 합니다.

## Hugo를 사용해서 웹사이트를 로컬에서 실행하기

[`netlify.toml`](netlify.toml#L10) 파일 내 `HUGO_VERSION` 환경 변수에서 지정된 Hugo 확장 버전을 설치합니다.

본 사이트를 로컬에서 빌드하고 테스트하려면 다음을 실행합니다.

```bash
make serve
```

이는 로컬 Hugo 서버를 1313번 포트에서 기동시킵니다. 브라우저에서 http://localhost:1313을 열어서 웹사이트를 보세요. 소스 파일에 변경을 가하면, Hugo가 웹사이트를 갱신하고 브라우저에서 새로고침이 되도록 합니다.

### macOS에서 너무 많은 파일이 열리는 문제에 대한 트러블슈팅

macOS에서 `make serve`을 실행했을 때 다음의 에러를 만나게 되면,

```
ERROR 2020/08/01 19:09:18 Error: listen tcp 127.0.0.1:1313: socket: too many open files
make: *** [serve] Error 1
```

열린 파일의 상한을 확인합니다.

`launchctl limit maxfiles`

그리고 다음의 명령을 실행합니다.

```
#!/bin/sh

# These are the original gist links, linking to my gists now.
# curl -O https://gist.githubusercontent.com/a2ikm/761c2ab02b7b3935679e55af5d81786a/raw/ab644cb92f216c019a2f032bbf25e258b01d87f9/limit.maxfiles.plist
# curl -O https://gist.githubusercontent.com/a2ikm/761c2ab02b7b3935679e55af5d81786a/raw/ab644cb92f216c019a2f032bbf25e258b01d87f9/limit.maxproc.plist

curl -O https://gist.githubusercontent.com/tombigel/d503800a282fcadbee14b537735d202c/raw/ed73cacf82906fdde59976a0c8248cce8b44f906/limit.maxfiles.plist
curl -O https://gist.githubusercontent.com/tombigel/d503800a282fcadbee14b537735d202c/raw/ed73cacf82906fdde59976a0c8248cce8b44f906/limit.maxproc.plist

sudo mv limit.maxfiles.plist /Library/LaunchDaemons
sudo mv limit.maxproc.plist /Library/LaunchDaemons

sudo chown root:wheel /Library/LaunchDaemons/limit.maxfiles.plist
sudo chown root:wheel /Library/LaunchDaemons/limit.maxproc.plist

sudo launchctl load -w /Library/LaunchDaemons/limit.maxfiles.plist
```

이는 Catalina는 물론 Mojave macOS에서 작동합니다.


# SIG Docs에 참여하기

Learn more about SIG Docs Kubernetes community and meetings on the [community page](https://github.com/kubernetes/community/tree/master/sig-docs#meetings).

You can also reach the maintainers of this project at:

- [Slack](https://kubernetes.slack.com/messages/sig-docs) [Get an invite for this Slack](https://slack.k8s.io/)
- [Mailing List](https://groups.google.com/forum/#!forum/kubernetes-sig-docs)


## 문서에 기여하기

이 저장소에 대한 복제본을 여러분의 GitHub 계정에 생성하기 위해 화면 오른쪽 위 영역에 있는 **Fork** 버튼을 클릭 가능합니다. 이 복제본은 *fork* 라고 부릅니다. 여러분의 fork에서 원하는 임의의 변경 사항을 만들고, 해당 변경 사항을 보낼 준비가 되었다면, 여러분의 fork로 이동하여 새로운 풀 리퀘스트를 만들어 우리에게 알려주시기 바랍니다.

여러분의 풀 리퀘스트가 생성된 이후에는, 쿠버네티스 리뷰어가 명료하고 실행 가능한 피드백을 제공하는 책임을 담당할 것입니다. 풀 리퀘스트의 오너로서, **쿠버네티스 리뷰어로부터 제공받은 피드백을 수용하기 위해 풀 리퀘스트를 수정하는 것은 여러분의 책임입니다.** 또한, 참고로 한 명 이상의 쿠버네티스 리뷰어가 여러분에게 피드백을 제공하는 상황에 처하거나, 또는 여러분에게 피드백을 제공하기로 원래 할당된 사람이 아닌 다른 쿠버네티스 리뷰어로부터 피드백을 받는 상황에 처할 수도 있습니다.  그뿐만 아니라, 몇몇 상황에서는, 필요에 따라 리뷰어 중 한 명이 [쿠버네티스 기술 리뷰어](https://github.com/kubernetes/website/wiki/Tech-reviewers)로부터의 기술 리뷰를 요청할지도 모릅니다. 리뷰어는 제시간에 피드백을 제공하기 위해 최선을 다할 것이지만, 응답 시간은 상황에 따라 달라질 수도 있습니다.

쿠버네티스 문서화에 기여하기와 관련된 보다 자세한 정보는, 다음을 살펴봅니다:

* [기여 시작하기](https://kubernetes.io/docs/contribute/start/)
* [문서화 변경 사항 스테이징하기](http://kubernetes.io/docs/contribute/intermediate#view-your-changes-locally)
* [페이지 템플릿 사용하기](http://kubernetes.io/docs/contribute/style/page-templates/)
* [문서화 스타일 가이드](http://kubernetes.io/docs/contribute/style/style-guide/)
* [쿠버네티스 문서화 로컬라이징](https://kubernetes.io/docs/contribute/localization/)

## `README.md`에 대한 쿠버네티스 문서화 번역

### 한국어

`README.md` 번역 및 한국어 기여자를 위한 보다 자세한 가이드를 [한국어 README](README-ko.md) 페이지 혹은 [쿠버네티스 문서 한글화 가이드](https://kubernetes.io/ko/docs/contribute/localization_ko/)에서 살펴봅니다.

한국어 번역 메인테이너에게 다음을 통해 연락 가능합니다.

* 이덕준 ([GitHub - @gochist](https://github.com/gochist))
* [Slack channel](https://kubernetes.slack.com/messages/kubernetes-docs-ko)

## 도커를 사용하여 사이트를 로컬에서 실행하기

쿠버네티스 웹사이트를 로컬에서 실행하기 위한 추천하는 방식은 [Hugo](https://gohugo.io) 정적 사이트 생성기를 포함하는 특별한 [도커](https://docker.com) 이미지를 실행하는 것입니다.

> Windows에서 실행하는 경우, [Chocolatey](https://chocolatey.org)로 설치할 수 있는 명명 추가 도구를 필요로 할 것입니다. `choco install make`

> 도커를 사용하지 않고 웹사이트를 로컬에서 실행하기를 선호하는 경우에는, 아래 [Hugo를 사용한 로컬 사이트 실행하기](#hugo를-사용한-로컬-사이트-실행하기)를 살펴봅니다.

도커 [동작 및 실행](https://www.docker.com/get-started) 환경이 있는 경우, 로컬에서 `kubernetes-hugo` 도커 이미지를 빌드 합니다:

```bash
make container-image
```

해당 이미지가 빌드 된 이후, 사이트를 로컬에서 실행할 수 있습니다:

```bash
make container-serve
```

브라우저에서 http://localhost:1313 를 열어 사이트를 살펴봅니다. 소스 파일에 변경 사항이 있을 때, Hugo는 사이트를 업데이트하고 브라우저를 강제로 새로고침합니다.

## Hugo를 사용한 로컬 사이트 실행하기

Hugo 설치 안내를 위해서는 [공식 Hugo 문서화](https://gohugo.io/getting-started/installing/)를 살펴봅니다. [`netlify.toml`](netlify.toml#L9) 파일에 있는 `HUGO_VERSION` 환경 변수에서 지정된 Hugo 버전이 설치되었는지를 확인합니다.

Hugo가 설치되었을 때 로컬에서 사이트를 실행하기 위해 (다음을 실행합니다):

```bash
make serve
```

이를 통해 로컬 Hugo 서버를 1313번 포트에 시작합니다. 브라우저에서 http://localhost:1313 를 열어 사이트를 살펴봅니다. 소스 파일에 변경 사항이 있을 때, Hugo는 사이트를 업데이트하고 브라우저를 강제로 새로고침합니다.

## 감사합니다!

쿠버네티스는 커뮤니티 참여와 함께 생존하며, 우리는 사이트 및 문서화에 대한 여러분의 컨트리뷰션에 대해 정말 감사하게 생각합니다!

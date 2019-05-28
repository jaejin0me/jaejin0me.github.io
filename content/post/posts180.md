---
title: "블로그 리뉴얼 사항"
date: 2019-05-28T21:59:55+09:00
tags: []
categories: []
author: "Jaejin Jang"
mathjax: true
---
겉으로 보기에 바뀐것은 없어 보이지만 내부적으로 몇가지를 수정했습니다.

## 1. 빌드 및 deploy 자동화

현재까지 블로그에 새로운 글을 올리기 위한 과정을 다음과 같았습니다.

 - 1. 새로운 포스트를 추가한다.  
 - 2. 빌드 한다.  
 - 3. 블로그 github에 푸쉬한다.  

이 중에서 2~3번 과정을 자동화 시켰습니다.
새로운 포스트를 추가하고 빌드용 repository에 push하기만 하면 빌드가 되고 블로그용 repository에 push가 되게 수정되었습니다.  
이 과정에서 [Travis CI](https://travis-ci.org) 라는 사이트를 이용했습니다.
제일 고생한게 trivis ci에서 어떻게 빌드할지에 대한 스크립트인 .travis.yml 에서 hugo 설치였습니다.   
구글에서 검색한 스크립트를 써도 잘안되더라고요. go get github~ 뭐 이렇게 시작하는 스크립트들이 많아서 따라해봤는데 저는 잘안됐습니다.  
그래서 그냥 wget으로 dep받아서 설치하게 작성했습니다. 단순하지만 가장 확실한 방법!

```
install:
  - wget -O /tmp/hugo.deb https://github.com/gohugoio/hugo/releases/download/v0.55.6/hugo_0.55.6_Linux-64bit.deb
  - sudo dpkg -i /tmp/hugo.deb
```

## 2. github를 이용한 이슈관리
리뉴얼 하면서 git사용법도 공부하고 github에서 제공하는 이슈관리도 적용했습니다.
혼자하는 것이기 때문에 모든 작업을 이슈를 만들어서 처리하지 않겠지만, 큰 변화가 있을만한 작업을 할때는 이슈를 만들어 다른 브랜치에서 작업하고 PR을 통해 머지하는 플로우를 따르려고 합니다.

## 3. 포스트이름 관리방법 변경
기존에는 카테고리별로 포스트이름을 나눠서 추가하고 있었는데, 전혀 그럴필요가 없어 posts숫자.md 형태로 수정했습니다. 

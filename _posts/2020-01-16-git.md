---
layout: post
title: SourceTree 활용 git 사용법 강의 정리
comments: true
---

## .gitignore

* [Gitignore](https://www.gitignore.io) -> `.gitignore` 패턴 생성 사이트<br>
* 파일명, 패턴 옵션 등 무시할 방법 설정 가능
* 이미 커밋되어 버전관리가 되는 파일 무시 방법
> 이 경우 해당 파일을 삭제한 후 커밋하고해야 합니다.<br>
> 무시할 파일의 경우 저장소에 들어 있지 않아도 빌드 과정에서 다시 만들어지는 파일이므로 삭제해도 무방합니다.

- - -

## checkout시 주의할 점
* 언제나 변동이 일어나는 곳으로 `Checkout` 후 작업!!
> ex)master로 checkout 후 newBranch와 merge를 하면 `master`만 변경

* 삭제는 다른 곳에서 해도 가능<br>
* 기존 branch 삭제를 해도 `pointer`만 없어짐, `history`는 남음

--- 

## Fast-forward

1. 같은 곳에서 branch를 나눔
2. 새 branch에서 commit 을 발생시킴
3. master로 checkout후 merge를 하면 master의 head만 변경
4. `Feature branch`는 fast-forward를 사용하지 **않음**

- - -

## git-flow

### 서포팅 브랜치

#### feature 브랜치

* 브랜치 생성: develop으로 부터
* 머지: develop으로
* prefix: feature/
* 특정 기능 하나에 대하여 develop으로부터 생성하여 개발
* 개발이 완료되면 다시 develop 브랜치로 merge
* 개발 기간이 `너무` 길면 주기적으로 develop 브랜치에 merge함

#### release 브랜치

* 브랜치 생성: develop으로 부터
* 머지: develop과 master로
* prefix: release/
* 배포를 위한 준비를 할 수 있는 브랜치
* 각종 메타 데이터 (버전 명 등)을 변경하거나 작은 버그를 수정
* 요즘은 사용하는데에 `의문`??

#### hotfix 브랜치

* 브랜치 생성: master로 부터
* 머지: develop과 master로
* prefix: hotfix/
* 배포된 버전에 긴급한 변경 사항이 있을 때 활용
* master로부터 생성
* 긴급한 이슈가 해결되면 다시 master와 develop에 머지
    * develop 브랜치에서는 다음 배포를 위한 기능 개발을 계속 진행 가능
* hotfix -> develop으로 머지하기 때문에, hotfix 브랜치의 변경 사항이 develop에 반영

- - -

## 원격 저장소

tracking을 사용하면 원격 브랜치를 따라감

### Fetch
* origin/master의 내용을 받아옴
* 로컬과 동기화 하려면 원격 브랜치를 로컬 브랜치로 머지해야함

### Pull
* fetch + merge를 자동으로 해줌

### Push
* 로컬 저장소의 변경 사항을 원격 저장소로 올려줌
* 로컬의 origin/master와 원격 저장소의 origin/master가 다르면?
    * pull 받은 다음 push함

- - -

## Pull Request

* 주요 목적 : Merge
* base에서 받고 compare에서 주기
* Merge를 하면서 `Code review` 가능!!
* Setting-branches에서 반드시 Pull Request 후, Develop branch에 merge 한다던지 하는 규칙 생성 가능
    * NHN github enterprise에서는 강제까지는 아님
    * Dooray - Pull Request 제목에 `#{프로젝트 업무 번호}` 입력하면 업무에 자동으로 댓글 달림!!
    > ex) 'BaseCamp-7기-루키행복TF/7 add기능 추가'로 Pull Request 생성

- - -

## 그 외

### revert
* git revert `{hash 값}` -m 1 ==> merge 통째로 날림

### reflog
* 지금까지 했던 로그들 다 남아있음

### 복구하기
1. git checkout `{삭제한 곳 hash}`
2. 브랜치 만들기

---
++ [Fork](https://www.git-fork.com) -> 버전 관리 프로그램(source tree보다 좋다고 함)

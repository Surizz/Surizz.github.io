---
layout: post
title: Bash 활용 기초 강의 정리 - 1
comments: true
---

# 목차
* 커서이동
* 기본명령
* 변수
* 파일 permission
* 제어문
* 입출력 리다이렉트
* 필터
* 파일 다루기
* Alias - 별명짓기
* 함수
* Script File
* Job Control
* 설정파일

# 소개
* Unix SHell
    * Kernel을 둘러싸고 있는 껍데기
    * 사용자가 제어할 수 있는 명령행 해석기
    * MS 윈도우에서는 DOS창

# 커서 이동
**문장의 맨 처음 / 끝**
* ctrl + a / ctrl + e

**단어 단위 이동**
* ctrl + 방향키
    * 터미널마다 다름

**화면 정리**
* ctrl + l
* 커서가 맨 위로 가짐
**스크롤 중단 및 재개**
* ctrl + s / ctrl + q

# 기본 명령

## 접속

```bash
ssh
rlogin -l <아이디> <호스트명>
```

## 종료
* logout / exit

## 조회
* last

## pwd
* 현재 디렉토리를 확인하는 명령

## id
* uname
    * 현재 접속해있는 서버의 이름/OS 정보 확인
* 비슷한 명령
    * hostname
    
## echo
    * 문자열을 출력하는 명령
    * `-n` 붙이면 개행 안함
    * `;`은 문장과 문장 구분할 때 사용
``` bash
echo "hello world"
echo -n "hello world"; echo "java coffee"
```
## ls
    * 디렉토리와 파일의 목록을 출력하는 명령
    * 윈도우즈 cmd의 dir
``` bash
ls -1 
ls -alF   숨긴 파일 까지
ls -t 시간 역순으로 출력
ls / 
ls /home1/irteam
```
## Globbing & Wildcard
### Glob pattern
* `*` : 임의의 길이를 가지는 문자열
* `?` : 임의의 한 글자
``` bash
ls *
ls .*
ls [a-f]*.txt
ls *.tx?
```
# 파일 시스템
## File
* OS에서 데이터를 디스크와 같은 저장소에 저장해 둘 때의 단위 형태
* 넓은 의미에서는 일반 파일, 디렉토리와 각종 입출력 장치를 모두 파일로 간주

## Directory
* 파일을 담아두는 공간
* 디렉토리 안에 디렉토리를 담을 수 있기 때문에 트리 구조로 형상화

## 디렉토리 이동
```bash
cd /var/log
```
## 사용자 홈 디렉토리
```bash
cd ~/.vim
```
## 특정 사용자의 홈 디렉토리
```bash
ls ~nemon
```

# History
## 조회
```bash
history
```

## 12번 명령 재실행
```bash
!12
```

## 커맨드 prefix로 재실행
```bash
!find
```
* find로 실행했던 커맨드 재 실행

## 바로 직전 명령 재실행
```bash
!!
```
## 바로 직전 명령의 마지막 argument
`$`는 보통 마지막이라는 의미
```bash
!$
```
## 직전 명령 편집
`^`는 보통 직전이라는 의미
```bash
echo "hello world"
^hello^java
```

# 파일 다루기
## touch
```bash
touch file1
```
## 디렉토리 만들기/삭제
```bash
mkdir 디렉토리
rmdir 디렉토리
```
**삭제는 비어있는 디렉토리만 가능**
## cp
```bash
cp test1.txt test2.txt
cp -r mydir1 mydir2
```
* 마지막 argument가 디렉토리면?
    * 여러개 복사해서 디렉토리에 저장
## mv
```bash
mv test1.txt test2.txt
mv mydir1 mydir2
```
* 대상 파일이 이미 존재하는 디렉토리면?
    * 에러 발생

## rm
```bash
rm -f test2.txt
rm -rf mydir2
```
* `-f`옵션: 강제로 지우기
    * 기본적으로 interactive(물어봄)하기 때문
    * 이걸 보지 않기 위해 -f 옵션을 줌
* `-rf` : 재귀적 삭제 

## file
```bash
file /bin/gzip
file /etc/rpm/macros.jpackage
```
* 파일의 종류를 판단하는 명령
* file *파일*
* 뒤에 `*`이 붙으면 실행 가능한 파일이라는 뜻
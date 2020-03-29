---
layout: post
title: Bash 활용 기초 강의 정리 - 2
comments: true
---
# 변수

## 사용자 변수

**보통 소문자로 명명**

``` bash
x=1
x="Hello world"
x=hello world (x)
```

* 주의사항
    * `=` 주위에 공백 없음

## 값을 꺼내쓰기

``` bash
echo x
echo $x
echo ${x}
echo "$x"
```

* echo x - 문자열 x
* echo $x - 값을 꺼냄
* echo ${x} - 값을 꺼냄, 추천
* echo "$x" - 오동작 이슈 존재

## Full Quotation
* 내부를 해석하지 않음
* single quote as literal

``` bash
x="java"; echo '$x is noun'
```

**결과**

``` bash
$x is noun
```

## Partial Quotation

* 내부를 변수로 생각

``` bash
x="java"; echo "$x is noun"
```

**결과**

``` bash
java is noun
```

# Evaluation

## 명령 실행 결과를 문자열 변수로 대입

**backquote`'` 또는 `$()` 로 둘러싸기**

``` bash
date_str=`date + "%Y%m%d"`
date_str=$(date + "%Y%m%d")
```

## 명령 문자열을 실행하기

**eval *명령문자열***

``` bash
cmd="ls -alF"
eval $cmd
```

## 사칙연산

``` bash
a=13
b=$((a % 3))
i=$((2 ** 60))
echo $RANDOM
```

# Type

**권장하지 않음**

## read-only

``` bash
declare -r var1=1
var1=2
```

## function

// 입력

# 환경변수

## 환경(environment)

* 프로세스를 둘러싼 주변 정보
* `env`로 볼 수 있음

## exporting

* sub-shell에서 상속받아 사용할 수 있도록 변수를 공개
* 사용자 변수를 환경 변수로 전환
* `export`나 `declare -x`로 지정

``` bash
var 1=2
bash
echo $var1
exit
```

**상속 X**

``` bash
export var1=2
bash
echo $var1
exit
```

**상속 O**
하지만 상위 shell에 영향을 미치진 않음

## 주요 환경 변수

* HOME
* PATH
* LANG: 인코딩
* LC\_ALL: 인코딩
* TERM: 터미널
* SHELL: 로그인 쉘
* HOSTNAME: 호스트 이름
* LOGNAME / USER / USERNAME
* ID / UID
* EDITOR / VISUAL: 기본 편집기
* PS1 / PS2: 프롬프트 지정

## 개발자에게 유용한 환경변수

* SHLVL: 쉘의 레벨, `depth` 보여줌
* PPID: parent process id
* SECONDS: 쉘이 시작한지 몇초나 됐는지
* FUNCNAME: 함수의 이름
* TMOUT: 타임아웃
    * 쉘에 타임아웃이 걸려있으면 입력 몇초간 안할 시 로그아웃.

# File Permission

**형식**

| type | owner | group | others |
| ---- | ----- | ----- | ------ |
| \- / d / \.\.\. | rwx | rwx | rwx |

``` bash
[name@local ~]$ ls -al
total 732
drwxr-xr-x 2 irteam irteam   4096 Jan 17 11:48 .
drwxr-xr-x 6 root   root     4096 Jan  3 15:52 ..
-rw------- 1 irteam irteam     32 Jan 21 10:24 .bash_history
-rwxr-xr-x 1 irteam irteam     18 Jan 12  2017 .bash_logout
-rwxr-xr-x 1 irteam irteam    176 Jan 12  2017 .bash_profile
-rwxr-xr-x 1 irteam irteam    124 Jan 12  2017 .bashrc
-rw-r--r-- 1 root   root        0 Jan  3 15:52 .k5login
-rwxr-xr-x 1 irteam irteam    368 Jan 16 11:21 set_webapps.sh
-rwxr-xr-x 1 irteam irteam    376 Sep 14  2018 set_webapps.sh.old
-rw-rw-r-- 1 irteam irteam 703394 Jan 17 11:24 test.txt
-rw------- 1 irteam irteam   4384 Jan 17 11:48 .viminfo
-rw-rw-r-- 1 irteam irteam     18 Jan 17 11:22 vs
```

``` bash
vi test1.sh
ls -al !$
chmod 0644 !$
ls -al !$

chmod u+x test1.sh
```

## umask

* 파일에 부여되는 permission의 초기값에서 `제외할` 권한 지정

``` bash
umask 0027
touch test12
```

## 소유자 변경하기

* chown *소유자 아이디 파일*
* root만 가능

# 제어문

## Boolean expression

`&&`, `||`, `!`, `-a`, `-o`

``` bash
((0 && 1)) && echo true || echo false
((0 || 1 )) && echo true || echo false
```

## Comparison operation

* integer
    * `-eq`, `-ne`, `gt`, `ge`, `lt`, `le`
* string
    * `<`, `<=`, `>`, `>=`, `==`, `=`, `!=`, `-z`, `-n`

## File Test

* `-e`: exist?
* `-s`: size not zero?
* `-f` / `-d` / `-p` / `-L`: file, directory, pipe, symlink?
* `-r` / `-w`/ ...: permision?
* `nt` / `-ot`: newer than, older than?

## if

* if [ *조건* ]; then *실행문*
* fi

**Bash**

``` bash
x=3
if [ "$x" -eq 3 ]; then echo "x is 3"
fi
```

**결과**

``` bash
x is 3
```

## while

* while [ *조건* ]; do *실행문*
* done

**Bash**

``` bash
i=3
while [ $i -lt 10 ]; do echo $i; i=$((i+1)); done;
```

**결과**

``` bash
3
4
5
6
7
8
9
```

## 무한루프

Ctrl+c로 나가기 가능

``` bash
while:;do date sleep 5; done;
```

## for

for *변수명* in *리스트*; do *실행문*; done;

**Bash**

``` bash
for dir in *; do
[ -d "$dir" ] && (echo "$dir"; cd "$dir"; ls | wc -l)
done
```

**결과**

``` bash
d1
1
d2
1
```

## switch-case

**bash**

``` bash
a=3
case "$a" in
[0-9])
echo ingeger;;
[[:upper:]])
echo upper;;
*) echo unknown;;
esac
```

## 주의사항

* `-eq` 연산자는 정수 연산자
* `==`, `=` 연산자는 문자열 연산자
* 변수가 정의되지 않았거나 공백을 포함할 수 있으므로 quotation 필요

``` bash
x="he llo"
```

# 입출력 리다이렉트

* 표준 입력(standard input) - 0번
    * 키보드 입력이 프로세스로 들어가는 통로
    * `<`: input redirect
* 표준 출력(standard output) - 1번
    * 프로세스가 출력한 내용이 화면으로 나오는 통로
    * `>`:output redirect
* 표준 에러(standard error) - 2번
    * 프로세스에서 발생한 에러가 화면으로 나오는 통로
    * `2>`: error redirect

**bash**
``` bash
sort < in txt
ls /asdfasg > out.txt
ls /asdfasg > out.txt 2> err.txt
```

## append
``` bash
ls /asdfasg > out.txt 2>&1
ls /asdfasg > 2>&1 | sort
```

## 필터

* 프로세스의 출력을 다른 프로세스의 입력으로 전달
* 파이프`|` 기호를 이용
* 각 내용마다 fork 후 exec 변환, pipe로 연결

**Bash**
정렬 후 중복제거 후 재정렬
```bash
ls | head
cat test.txt | sort | uniq -c | sort -n
```

**결과**
```bash
1 c
2 a
3 d
5 b
```

# 파일 내용 보기
## 전체 보기
```bash
cat test1.txt
more test1.txt
```

## 일부 보기
* 앞쪽 일부만 보기
    * head -10 test1.txt: 앞 10줄 보기

* 뒷쪽 일부만 보기
    * tail -5 test1.txt : 뒤 5줄 보기
    * tail -f /var/log/lastlog
        * 파일이 계속 변하고 있는 걸 출력
        * 변화를 볼 때 사용

## 문자열 찾기
**grep *문자열 파일***
```bash
grep "export" /etc/profile
grep -r "export" /etc
```
## 잘라내서 쓰기
한 라인을 잘라내서 씀
**cut -d *구분자* -f *필드번호 파일***
```bash
grep irteamro /etc/passwd | cut -d: -f1,3-4,7
```

# Alias
**별명**
```bash
alias grep="egrep"
alias diffs="diff -ignore-all-space"
alias l="ssh hcon.nhnent.com"
```
# 함수
function과 subroutine을 구분하지 않음 - 반환값 존재 여부
* function 으로 시작 가능(안써도 됨)
* function 이름 뒤 `()` 가능(안써도 됨) 
* `$0` - 자신, `$1`...`$9` - arguments
* `echo $?`: 결과값
```bash
func1() { echo "hello, $1"; }
func1 "world"

function func2 { return $(($1 + $2)); }
func2 3 4
echo $?
```

* 변수는 기본적으로 전역
* 함수 내부에서만 사용하려면 local로 지정해야 함

**bash**
```bash
function test1() {
    local int val=3
    str_val="value of global variable"
 }
 test1
 echo $int_val
 echo $str_val
```
**결과**
```bash
[name@local ~]$  echo $int_val

[name@local ~]$  echo $str_val
value of global variable
```
# Script File
## shabang
* sharp + bang 이라는 뜻
* #!
    * 어떤 인터프리터 프로그램을 사용할 것인가를 지정
* #!/bin/bash
* #!/usr/bin/env python
    * 권장하는 방식
* #!/bin/awk -f
    * 옵션을 붙여야 할 경우, env을 사용할 수 없음.

## 파일 작성

```bash
///test.sh
#!/bin/bash
echo "hello world"
```
```bash
//shell prompt
bash test.sh

chmod a+x test.sh
./test.sh
```
## arguments & status
**Function과 완전 같음**

# Job Control
## Foreground job
* 기본적으로는 프로그램은 foreground에서 실행됨
* 한 명령의 실행이 끝날 때까지 다음 명령은 기다려함

## Background job
* `&`을 붙여서 실행하면 background에서 실행됨
* 다음 명령을 바로 실행하거나 프롬프트가 뜸

## Example
```bash
sleep 500 & -- background 실행
jobs
fg %1
Ctrl-z
bg
wait
```
* sleep 500 & ==> background 실행
* jobs ==> 현재 실행중 목록
* fg %1 ==> foreground로 불러옴
* Ctrl-z ==> 다시 백그라운드로
* bg
* wait ==> 기다림

# 사용자 설정
* Bourne shell
    * ~/.profile

* BASH
    * login shell: `.bashrc`
        * 현재 shell에 적용하려면 source ./bashrc 해야함
    * non-login shell: `.bash_profile`
    * export해서 관리하는게 편함

# 기타 설정
## 파일 경로 다루기
* `dirname`: 디렉토리 이름 구하기
* `basename`: 파일 이름 구하기

```bash
dirname /usr/local/share/doc/foo.txt
basename /usr/local/shhare/doc/
```
## 화면 클리어
**clear**

## 실행 가능한 프로그램 확인
**which**
```bash
which lsof
```
## 로케일 관련 명령

**locale**
```bash
locale
locale -a
```
**iconv**
* 파일 인코딩 변경
* -c 옵션 필수
* -l 사용 시 목록 확인
```bash
iconv -c -f CP949 -t UTF-8 ${완성형파일} > ${UTF8파일}
iconv -l
```

## 정렬과 카우팅

* **정렬**: sort *파일*
* **인접한 행의 중복 제거**: uniq -c
* **라인 카운팅**: wc -l
```bash
sort | uniq -c | sort -n
```
## 파일 탐색
**find**
find *디렉토리 목록* -name *이름패턴* -type *파일유형* ...

```bash
find /usr/lib64
find /usr/lib64 -name "lib*.a" ls
find /usr/lib64 -type -d -name "py*"
find /usr/lib64 \(-name "python*" -o -name "pgsql*" \) -ls
```

## xargs
* 행 단위로 처리하기
* xargs *명령*
* xargs -I % *명령*"%"

```bash
find /etc/init | xargs wc -l
find /etc/init | xargs -I % wc -l "%"
```

# 다른 사용자로 변경
## su
**다른 사용자로 변경하기**
```bash
su irteam
su -l irteamsu
```
## sudo
* 인터넷 서비스를 올릴 때 1024 이하의 포트 정리할 때
    * 바인딩 하기가 힘듬, 관리자로 실행
```bash
rlogin -l irteamsu 호스트명
sudo apps/httpd/bin/apachectl start
```

# 패스워드 변경하기
* passwd
    * 접속중인 패스워드 변경
* kpasswd
    * 커버로스 패스워드 변경
* 주기적(3개월)으로 변경

# 원격 제어

* rcp, scp: 파일 복사
* ftp, sftp: 파일 전송

# 파일 압축
**zip 포멧**
```bash
zip -r 파일 이름
unzip -l 파일 이름
```

# 시스템 관련 명령
* pstree: 목록 조회
* pidof *프로세스명*: 프로세서명으로 pid 알아내기
* top: 자원 사용현황 조회
* du -s: 디렉토리의 크기 조회
* df -k: 디스크 전체 사용현황 조회

# 네트웍 관련 명령
* ifconfig: ip주소
* ping: ping 보내기
* nslookup ==> getent hosts: ip 구하기

# 메뉴얼 페이지
* man wc
* man 2 open
* man 3 opendir

---
참고: NHN 기술교육 - bash tutorial
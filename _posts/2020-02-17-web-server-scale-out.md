---
layout: post
title: 웹서버 스케일 아웃
comments: true
---
# 웹 서버를 두 대로 늘리면?
## 생각되는 문제점
* 부하 분산
* 공유 자원에서 동기화 문제
* DB에서의 문제
* 웹서버 세션 문제
* 서버가 죽었을 때

# 서버 한 대에서 시작
## 생각되는 문제점
* 가용성 부족(하나가 죽으면 끝)
* 웹과 디비를 같이 써서 생기는 문제: 보안 문제!!
    * 사용자가 접근할 수 없게 중간에 방화벽을 둠

# 두 대의 서버
## 웹 서버 1대, 디비 서버 한대
* SPOF 문제

# 세 대의 서버
## 로드 밸런싱
* 두 서버에 트래픽을 분배
## 고가용성
* 장애 시에 대기 서버를 활용

## 로드밸런싱과 고가용성
* 가장 흔한 것이 L4
* L7 헬스체크
    * 헬스체크 URL을 통해 확인(200 OK)
    * 임의로 설정 가능 하기 때문에 사용자가 보지 못하게 수정 가능
    * 반면 L4는 사용자가 접근 불가능 한 것을 확인 가능하기 때문에, 별로임
* DNS RR을 이용
    * 캐싱이 문제(브라우저, 서버사이드)
 * 세션 공유
     * 로그인 정보 등을 공유해야함
     * 쿠키를 이용 할 수도
* 로컬 디스크에 공유 정보를 저장할 수 없음
* 컨텐츠 공유 방법
    * DB
    * NAS/NFS
    * Redis

## DSR(Direct Server Return) 모드
* L4가 양방향 프락시라면
    * 모든 웹서버가 받는 트래픽을 L4가 다 받아야 함
* L4의 부담을 덜어주기 위해 직접 반환함
    * ex)응답이 1GB일 때
* 적당한 예: 요청 패킷이 적은 케이스
    * 일반적인 웹 요청
    * 파일 다운로드
* 적합하지 않은 예: 요청 패킷이 많은 케이스
    * 파일 업로드와 같은 웹 요청
    * 업로드 할 때는 L4를 거치지 않도록 예외 처리
    * SMTP
* 대용량 파일 업로드: 2단계로 거쳐서 동작
    * 클라이언트에게 web-1의 IP를 알려준 뒤 직접 POST 쓰게 함
    1. GET http://L4-IP/who
    2. web-1 public IP
    3. POST http://web-1/upload

---
참고: NHN 기술교육 - 웹 서버 스케일 아웃
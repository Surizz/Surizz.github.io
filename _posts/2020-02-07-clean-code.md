---
layout: post
title: 읽기 좋은 코드
comments: true
---
# 시작하기 전에

* <읽기 좋은 코드가 좋은 코드다>의 요약 정리: [http://www.hanbit.co.kr/book/look.html?isbn=978-89-7914-914-2](http://www.hanbit.co.kr/book/look.html?isbn=978-89-7914-914-2)
* 절대적인 기준은 없음
* 일관성이 가장 중요

# 코드는 이해하기 쉬워야 한다

## 간결하게

```
for (Node* node = list->head; node != NULL; node = node->next)
    Print(node->data);
```

```
Node* node = list->head;
if (node == NULL) return;

while(node->next != NULL) {
    Print(node->data);
    node = node->next;
}
if (node != NULL) Print(node->data);
```

## 간결 != 적은 줄

``` 
return exponent>=0 ? mantissa * (1 << exponent) : mantissa / (1 <<-exponent);
```

``` 
if (exponent >= 0) {
    return mantissa * (1 << exponent);
} else {
    return mantissa / (1 << -exponent);
}
```
줄이 짧다고 해서 좋은 코드는 아니다!!

## 핵심 아이디어

> 코드는 "다른 사람"이 그것을 "이해"하는 데 들이는 시간을 최소화하는 방식으로 작성해야한다.

* 내가 읽기 좋은 것은 의미가 없음 -> 동료가 읽기 좋아야 한다.
* "이해"
    * 코드를 자유롭게 수정하고
    * 버그를 짚어내고
    * 수정한 내용이 여러분이 작성한 다른 부분의 코드와 어떻게 상호작용하는지 알 수 있어야
        * 다른 협업자의 작업물에 추가한 경우에 이상해 보이지 않
* "다른 사람"
    * "지금의 나"와 "6개월 뒤의 나"는 다른 사람 ^^

* 보이스카웃 원칙
    * 보이스카우트는 캠핑장소에 갔을 때 그 장소를 `청소`하고 가야 한다
    * 다른 사람의 코드에 `악취`가 난다면 자기가 치우자

# 이름에 정보 담기

> 핵심: 이름에 정보를 담아라
 
* 오버커뮤니케이션을 `지향`하자!
    * 상호간의 커뮤니케이션이 중요
* 적절한 단어를 고르자

## 특정한 단어 고르기

### `GetPage()`

``` python
def GetPage(url):
    ...
```

* `get`은 지나치게 보편적
    * 너무 일반적이다.
    * 무엇을 가져오는지?
* 로컬 캐쉬에서? 인터넷에서?!
    * `FetchPage()`, `DownloadPage()`

### `Size()`

``` cpp
class BinaryTree{
    int Size();
    ...
}
```

* 트리의 높이? `Height()`
* 노드 개수? `NumNodes()`
* 트리의 메모리? `MemoryBytes()`

### `Stop()`

``` cpp
class Thread {
    void Stop();
}
```

* 더 정확한 이름을 고를 수 있음
* 되돌릴 수 없는 최종 동작이라면 `Kill()`
* `Resume()`등으로 다시 돌이킬 수 있다면 `Pause()`
* Thread의 경우에는 Lifecycle에 맞는 명명을 하자.

### 보다 다양한 단어를 활용

| 단어 | 유의어 |
| --- | --- |
| send | deliver, dispatch, announce, distribute, route |
| find | search, extract, locate, recover |
| start | launch, create, begin, open |
| make | create, set up, build, generate, compose, add, new |

* 미묘한 뉘앙스를 캐치하자
* 같은 역할은 하면 일관성있게 같은 걸 쓰자
* 잘못된 네이밍 규칙으로 되어 있어도 `협업`일때는 일관성을 맞추는게 좋다.
    * 깨진 유리창의 법칙 - 클린 코드도 기하급수적으로 더러워진다
    * 그럴지라도 일관성을 맞추면 깨진 유리창이 아니게 된다.
    * ??

## `tmp`, `retval`같은 보편적 이름 피하기

### `retval`

* 리턴값이라고 무조건 `retval`을 쓰지 않기
* 그 변수가 의미하는 바를 이름으로

### `tmp`

* 적당한 예

``` cpp
if (right < left) {
  tmp = right;
  right = left;
  left = tmp;
}
```

* 적당하지 않은 예: `tmp` --> `userInfo`

``` java
String tmp = user.name();
tmp += " " + user.phoneNumber();
tmp += " " + user.email();
...
template.set("user_info", tmp);
```

* 이름의 일부에 `tmp`를 사용
    * 나쁘지 않음, 의미가 있기 때문

``` cpp
tmp_file = tempFile.NamedTemporaryFiles();
...
SaveData(tmp_file, ...);
```

* `tmp`가 파일? 파일 이름? 파일 데이터?
    * 의미를 알 수 없음, 좋지 않은 예

``` cpp
SaveData(tmp, ...);
```

> 조언: tmp라는 이름은 대상이 짧게 임시적으로만 존재하고, 임시적 존재 자체가 변수의 가장 중요한 용도일 때 한해서 사용해야한다.

### `i`, `j`, `k`

* 반복자라는 의미를 충분히 전달. 그래서 다른 목적으로 사용하면 헷갈릴 수 있음
* 다음과 같이 중첩해서 사용할 때 헷갈리지 않도록 유의. 조금 더 명확하게
* 알고리즘 문제 풀 때는 괜찮지만, 협업할 때에는 주의해서 쓰자(누구나 이해할 수 있게)

``` cpp
for (int i = 0; i < clubs.size(); i++)
  for (int j = 0; j < clubs[i].members.size(); j++)
      for (int k = 0; k < users.size(); k++)
          if (clubs[i].members[k] == users[j])
              ...
```

* `i` --> `club_i`, `j` --> `members_j`, `k` --> `users_k`

> 조언: tmp, it, retval 같은 보편적인 이름을 사용하려면, 꼭 그렇게 해야하는 이유가 있어야 한다.

### 기타

* Manager, Info, Object

## 추상적인 이름보다 구체적인 이름을 선호하라

### `DISALLOW_EVIL_CONSTRUCTOR`

* C++에서 복사 생성자, 할당 연산자를 생략하면 자동으로 기본값을 제공. 이 코드가 추후 문제를 일으킬 수 있어서 그걸 막는 매크로(구글 코드)
* `EVIL`이라는 낱말이 너무 추상적
* 결국 `DISALLOW_COPY_AND_ASSIGN`으로 변경

### `--run_locally`
* 무언가를 작명할 때에는 상황이 아니라 목적을 써야 함
    * 상황을 쓰고 있어서 좋지 않은 명명
* 명령행 플래그
    * 디버깅 정보를 출력하고 동작 속도가 느려짐
    * 로컬에서 주로 디버깅하는 목적으로 사용
* 문제
    * 이름으로 어떤 플래그를 뜻하는지 알 수 없음
    * 때에 따라 리모트에서도 필요한 옵션
    * 진짜 그냥 로컬에서 띄울 뿐인데 본의 아니게 느리게 띄울 수도
* 제안
    * `--extra_logging`

## 추가적인 정보를 이름에 추가

* `string id; //Example: "AF84EF845CD8"`
    * --> `string hexId;`
    * id가 어떠한 형식을 가지는지 명시하면 더 좋을**수도** 있음
* 단위: sec? ms?: `Start(int delay_secs)`
    * 딜레이 단위가 어떤지 알아야 함
* `plaintext_password`, `unescaped_comment`, `html_utf8`
* 구체적으로 어떻게 작동하는지 쓰고, 무엇을 필요로 하는지 쓰자

## 변수 길이
* 논란이 있을 수도 있는 주제
    * python, javascript 등 같은 경우(함수형 언어들)은 짧게 쓰는걸 선호
* 충분히 설명
    * IDE를 활용하면 길어도 크게 문제 안됨
    * `BEManager` --> `BackEndManager`
* 스코프가 좁을 때는 짧아도 OK
    * 한눈에 보이므로 문제되지 않음
    * 다만 스코프가 길면 위치를 까먹을 가능성 높으므로 의미를 부여하자 
``` cpp
if (debug) {
  map m;
  LookUpNamesNumbers(&m);
  Print(m);
}
```

# 오해할 수 없는 이름들

> 핵심: 본인이 지은 이름을 '다른 사람들이 다른 의미로 해석할 수 있을까?'라는 질문을 던져보며 철저하게 확인해야한다.
 
* 왼쪽, 오른쪽이라는 것도 무엇을 기준으로 하는지 모호함
* `나`를 기준으로 코드를 작성하기 때문
* 내 동료가 봤을 때 어떻게 판단할 지 항상 생각하자
## `Filter()`

``` java
results = Database.all_objects.filter("year <= 2011")
```

* 제외? 해당?
* 제안
    * 고르는 것이라면 `select()`
    * 제거하는 것이라면 `exclude()`

## `min`, `max`

* 경계를 포함하는 한계를 다룰 때

``` python
CART_TOO_BIG_LIMIT = 10
  if shopping_cart.num_items() >= CART_TOO_BIG_LIMIT:
      Error("Too many items in cart!")
```

## `first`, `last`

* 경계를 포함하는 범위를 다룰 때

## `begin`, `end`

* 경계를 포함하고/배제하는 범위를 다룰 때

## 불리언 변수

* `is`, `has`, `can`, `should`등의 접두어 활용
* 변수는 긍정 문구로
* enableSsl이 더 좋아보임

``` java
bool disableSsl = false;
```

## 사용자의 기대에 부응하기

### `getXXX()`

* 가볍게 값을 읽을 거라는 기대
* `getMean()` vs `computeMean()`

### `list::size()`

* C++ 표준 라이브러리
    * O(n)
* 지금은 스펙에 "size()는 O(1)이어야 한다"고 정의



# 주석에 담아야 하는 대상

> 핵심: 주석의 목적은 코드를 읽는 사람이 코드를 작성한 사람만큼 코드를 잘 이해하게 돕는데 있다

## 설명하지 말아야 하는 것

### 뻔한 것

``` cpp
// 클래스 Account를 위한 정의
class Account {
    public:
        // 생성자
        Account();

        // profit에 새로운 값을 입력
        void SetProfit(double profit);
}
```

> 핵심: 코드에서 빠르게 유추할 수 있는 내용으로 주석을 달지 말라

``` python
name = '*'.join(line.split('*')[:2]) # 두 번째 '*' 뒤에 오는 내용을 모두 제거한다.
```

### 나쁜 이름에 주석을 달지 말라 - 대신 이름을 고쳐라

#### 예 1

``` cpp
// 반환되는 항목의 수나 전체 바이트 수의 길이
// Request가 정하는 대로 Reply에 일정한 한계를 적용한다.
void ClearReply(Request request, Reply reply);
```

``` cpp
// 'reply'가 count/byte/등과 같이 'request'가 정하는 한계조건을 만족하도록 한다.
void EnforceLimitsFromRequest(Request request, Reply reply);
```

#### 예 2

```
// 해당 키를 위한 핸들을 놓아 준다. 이 함수는 실제 레지스트리를 수정하지는 않는다.
void DeleteRegistry(RegistryKey *key);
```

```
void ReleaseRegistryHandle(RegistryKey *key);
```

### 생각을 기록하라

#### 중요한 통찰을 기록

```
// 놀랍게도, 이 데이터에서 이진트리는 해시테이블보다 40% 정도 빠르다.
// 해시를 계산하는 비용이 좌/우를 비교하는 비용을 능가한다.
```

#### 코드의 결함을 설명

```
//TODO: 더 빠른 알고리즘을 설명하라
```

| 표시 | 보통의 의미 |
| --- | ------ |
| TODO: | 아직 하지 않은 일 |
| FIXME: | 오동작을 일으킨다고 알려진 코드 |
| HACK: | 아름답지 않은 해결책 |
| XXX: | 위험! 여기 큰 문제가 있음 |

내일 할 것만 작성!!

#### 상수에 대한 설명

```
NUM_THREADS = 8; // 이 상수 값이 2 * num_processors보다 크거나 같으면 안된다.

const int MAX_RSS_SUBSCRIPTIONS = 1000; // 합리적인 한계를 설정하라 - 이렇게 많이 읽을 수 있는 사람은 없음

image_quality = 0.72; // 사용자들은 0.72가 크기/해상도 대비 최선이라고 생각한다.
```

* 주저리주저리 쓰는게 맞는지는 모르겠음


## 코드를 읽는 사람의 입장이 될 것

### 나올 것 같은 질문 예측하기

### 사람들이 쉽게 빠질 것 같은 함정을 경고하라

* 평범한 사람이 예상하지 못할 특이한 동작을 기록
* 보내기만 한 것인지, 실제로 받은 것 까지 한 것인지
```java
// 리턴값이 `true`라고 메일이 전달된 것은 아님!
// SMTP 서버에 전달 성공을 뜻함
boolean SendEmail(String to, String subject, String body);
```

### '큰 그림'에 대한 주석

* 파일이나 클래스 수준에서 '큰 그림'을 설명하고 각 조각이 어떻게 맞춰지는지 설명

### 요약 주석

``` python
def GenerateUserReport():
    # 이 사용자를 위한 lock을 얻는다.
    ...
    # 데이터베이스에서 사용자의 정보를 읽는다.
    ...
    # 정보를 파일에 저장한다.
    ...
    # 사용자를 위한 lock을 되돌려 놓는다.
    ...
```

* 이런 것보다 각각을 메소드로 분리해서 주석을 없애는 것이 좋은 코드


## 글 쓰는 두려움을 떨쳐내라

1. 마음에 떠오르는 생각을 무조건 적어본다.(다듬어 지지 않았어도)
2. 주석을 읽고 무엇이 개선되어야 하는지 확인한다.
3. 개선한다.

# 읽기 쉽게 제어 흐름 만들기

## 조건문에서 인수의 순서

### 예 1

```
if (length >= 10)
```
 
```
if (10<=length)
```

### 예 2

```
while(bytes_received < bytes_expected)
```

```
while(bytes_expected > bytes_received)
```

### 읽기 편한 규칙

| 왼쪽 | 오른쪽 |
| --- | --- |
| 값이 더 유동적인 질문을 받는 표현 | 더 고정적인 값으로 비교 대상으로 사용하는 표현 |

## if/else 블록의 순서

* 같이 있을 때 적용
* 부정이 아닌 긍정을 다룰 것 `if(debug)`
* 두 블록 중 간단한 블록을 먼저
* 더 흥미롭고, 확실한 것을 먼저

## 삼항연산자 ?

> 핵심: 줄 수를 최소화하는 일보다 다른 사람이 코드를 읽고 이해하는데 걸리는 시간을 최소화하는 일이 더 중요하다

```
time_str += (hour >= 12) ? "pm" : "am";
```

## do/while 루프를 피하라

```
do {
    continue;
} while(false);
```
* C++ 창시자의 말씀
* 가독성이 좋지 않다.


## 함수 중간에 반환하기

* 중간에 반환하는 것을 막는 이유는 클린업 코드 때문
* 예외는 빠르게 처리하기 위해
* 하지만 현대 언어는 더 정교한 방법을 제공
    * 파괴자, try/catch/finally, with(파이선), using(C#)

## 코드 중첩 최소화

### 중첩된 코드

* N번 중첩된 코드라면, 변수 N 개의 상태를 기억하며 읽어야 함

``` cpp
if (user_result == SUCCESS) {
    if (permission_result != SUCCESS) {
        reply.WriteErrors("error reading permissions");
        reply.Done();
        return;
    }
    reply.WriteErrors("");
} else {
    reply.WriteErrors(user_result);
}
reply.Done();
```
메소드 분리를 하면 더 쉬워진다

* [실습] 중첩 제거해보기

### 중첩을 제거한 코드

``` cpp
if (user_result != SUCCESS) {
    reply.WriteErrors(user_result);
    reply.Done();
    return;
}

if (permission_result != SUCCESS) {
    reply.WriteErrors("error reading permissions");
    reply.Done();
    return;
}

reply.WriteErrors("");
reply.Done();
```

# 거대한 표현을 잘게 쪼개기

## 설명 변수

```
if line.split(':')[0].strip() == "root":
```

```
username = line.split(':')[0].strip()
if username == "root"
    ...
```

## 요약 변수

``` java
if (request.user.id == document.owner_id) {
    // 사용자가 이 문서를 수정할 수 있다.
}
...
if (request.user.id != document.owner_id) {
    // 문서는 읽기 전용이다.
}
```

``` java
final boolean user_owns_document = (request.user.id == document.owner_id);

if (user_owns_document) {
    // 사용자가 이 문서를 수정할 수 있다.
}
...
if (!user_owns_document) {
    // 문서는 읽기 전용이다.
}
```

## 드모르간의 법칙 사용하기

```
if (!(file_exists && !is_protected)) Error("...");
```

```
if (!file_exists || is_protected)) Error("...");
```

## 쇼트서킷 논리(short-circuit logic) 오용 말기

```
assert((!(bucket = FindBucket(key))) || !bucket->IsOccupied());
```

```
bucket = FindBucket(key);
if (bucket != NULL) assert(!bucket->IsOccupied());
```

> 핵심: 영리하게 작성한 코드에 유의하라. 나중에 다른 사람이 읽으면 그런 코드가 종종 혼란을 초래한다.

### 이디엄 같은 경우

* C

```
if (object && object->method())
```

* 자바스크립트, 파이선, 루비

```
x = a || b || c
```

# 변수와 가독성

* 변수의 수가 많을수록 기억하고 다루기 더 어려워진다.
* 변수의 범위가 넓어질수록 기억하고 다루는 시간이 더 길어진다.
    * 메소드 분리
* 변수값이 자주 바뀔수록 현재값을 기억하고 다루기가 더 어려워진다.
    * 불변객체 이용

## 변수 제거하기

### 불필요한 임시 변수

``` python
now = datetime.datetime.now()
root_message.last_view_time = now
```

## 변수의 범위를 좁혀라

> 핵심: 변수가 적용되는 범위를 최대한 좁게 만들어라

### 문제 코드

``` cpp
class LargeClass {
    string str_;

    void Method1() {
        str_ = ...;
        Method2();
    }

    void Method2() {
        // str_ 변수 사용하기
    }

    // str_을 사용하지 않는 다른 메소드...
};
```
인스턴스 변수는 위험하다. (임계영역 접근)
-> 최대한 지역변수로 쓰자


### 개선

* 지역 변수로 강등
``` cpp
class LargeClass {
    void Method1() {
        string str = ...;
        Method2(str);
    }

    void Method2(string str) {
        // str 변수 사용하기
    }
};
```
* 커다란 클래스를 여러 작은 클래스로
    * 작은 클래스끼리 서로의 멤버를 참조하면 나눈 효과가 없음



### C++에서 if문의 범위

``` cpp
PayInfo * info = db.ReadPayInfo();
if (info) {
    ...
}
```

``` cpp
if (PayInfo* info = db.ReadPayInfo()) {
    ...
}
```

### 정의를 사용하기 직전에

* 예전 C에서는 선언부를 맨 앞에 써야만 했으나 지금은 아님

## 값을 한 번만 할당하는 변수를 선호하라

* C++ `const`, 자바 `final`

> 핵심: 변수값이 달라지는 곳이 많을수록 현재값을 추측하기 더 어려워진다.


# 상관없는 하위 문제 추출하기

* 일반적인 목적의 코드를 프로젝트의 특정 코드에서 분리

## 순수한 유틸리티, 일반적인 목적의 코드

* 문자열 변경, 해시테이블 사용, 파일 읽기/쓰기
* Map pretty 출력

# 한 번에 하나씩

> 핵심: 한 번에 하나의 작업만 수행하게 코드를 구성해야 한다.

* 흩어진 작업을 관련 코드 단위로 묶고, 필요하면 각각을 메소드로 분리

# 코드 분량 줄이기

* 제품에 꼭 필요하지 않은 기능을 제거하고, 과도한 작업을 피한다.
* 요구사항을 다시 생각해서, 가장 단순한 형태의 문제를 찾아본다.
* 주기적으로 라이브러리 전체 API를 훓어봄으로써 표준 라이브러리에 친숙해진다.

## 예, LRU 캐시 만들기

* 디스크에서 객체를 읽는데 느림
* 캐시가 필요
* LRU면 충분할 듯

### 어떤 질문을...?

### 사용 패턴

```
read Object A
read Object A
read Object B
read Object B
read Object B
read Object C
read Object D
read Object D
```

### 이 상황에 맞는 간단한 구현을

``` java
DiskObject lastUsed; // 클래스 멤버

DiskObject lookUp(String key) {
    if (lastUsed == null || !lastUsed.key().equals(key)) {
        lastUsed = loadDiskObject(key);
    }

    return lastUsed;
}
```

## 라이브러리와 주변 도구에 대한 충분한 이해

* Java 7, 8, 9, 10에서 추가된 API에 대한 이해
* Spring 버전 업 정보 확인

### 예, 파이썬의 기본 자료구조

`[2, 1, 2]`와 같은 List에서 중복을 제거하고 싶다면?

``` python
def unique(elements):
    temp = {}
    for element in elements:
        temp[element] = None
    return temp.keys()

unique_elements = unique([2, 1, 2])
```

``` python
unique_elements = list(set([2, 1, 2]))
```

### 예, json-simple

[https://code.google.com/p/json-simple](https://code.google.com/p/json-simple)
`JSONObject`를 `HashMap`으로 변경하는 작업을 종종하는데 쓸데없는 짓

### 예, jQuery의 `next()`

``` html
<div id="tip-1" class="tip">Tip: 업무 등록 핫키는 Ctrl-w 입니다.
<div id="tip-2" class="tip">Tip: 빠른 검색은 Ctrl-k입니다.
```

``` javascript
...
$('#tip-' + (shown_tip_num + 1)).show();
...
```

``` javascript
cur_tip = $('.tip:visible').hide();
next_tip = cur_tip.next('.tip');
```

### 예, Map의 `getOrDefault()`

``` java
Long port = map.get("port");
if (port == null) {
    port = 8080; 
}
```

``` java
long port = map.getOrDefault("port", 8080);
```

### 예, 특정 월의 첫 번째 월요일 구하기

* 1일의 요일을 구해서 약간의 덧셈을 하여 월요일 찾기

``` java
LocalDate.of(year, month, 1).with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY))
```

### 예, 코딩 대신 유닉스 도구 활용하기

* 웹 서버에서 4XX, 5XX 오류가 많이 발생하고 있는 상황
* 어떤 URL이 가장 많은 문제를 일으키는가?
```
203.12.32.1 example.com [24/Aug/2010 01:08:11] "GET /index.html HTTP/1.1" 200
```

``` sh
cat access.log | awk '{print $6 " " $8 }' | egrep "[45]..$" | sort | uniq -c | sort -nr
```

```
95 /favicon.ico 404
13 /help?topic=8 500
11 /login 403
...
```
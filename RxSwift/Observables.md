
## Observable

RxSwift에서 observer는 Observable을 **구독** 한다.
Observable이 **방출**(emit)하는 하나 또는 연속된 item에 옵저버는 반응(react)한다.
이러한 패턴은 **동시성**(concurrent) 연산을 가능하게 한다.
그 이유는 Observable이 방출하기를 기다릴 필요 없이 Observable을 감시하는 observer를 두어
방출 알림을 받으면 되기 때문이다.

![[Pasted image 20241007135352.png]]

### Observable의 생명주기
* `next` 이벤트를 계속해서 방출 시킬 수 있다. `next` 이벤트는 어떠한 `Element`  인스턴스를 가진다.
* `error` 이벤트를 방출하여 완전 종료될 수 있다. `error` 이벤트는 `Swift.error` 인스턴스를 가진다.
* `completed` 이벤트를 방출하여 완전 종료될 수 있다. `completed` 이벤트는 인스턴스를 가지지 않고 단순히 이벤트를 종료시킨다.

### Observable 만들기

### just
```Swift
let one = 1
let two = 2
let three = 3

let obsevable: Observable<Int> = Observable<Int>.just(one)
```
`just`는 Observable의 타입 메소드. 오직 하나의 요소를 포함하는 Observable sequence를 생성한다.

#### of
```Swift

let observable = Observable.of(one, two, three)
```
`of` 연산자는 주어진 값들의 타입추론을 통해 Observable sequence를 생성한다.
타입은 `Observable<Int>` 즉 observable로 이루어진 array를 만들 수 있다.
`Observable<[Int]>` 타입을 만들고 싶다면
```Swift
let observable = Observable.of([one, two, three])
```

#### from
```Swift
let observable = Observable.from([one, two, three])
```
타입은 `Observable<Int`
`from` 연산자는 array각각 요소들을 하나씩 방출한다.
`from` 연산자는 오직 array만을 받는다.


### Observable 구독

>[!note] 중요
>  Observable은 sequence 정의일 뿐이다. Observable은 subscriber되기 전에는 아무런 이벤트도
>  보내지 않는다. 그저 정의일 뿐이다.


## Traits

Observable을 감싸고 있는 구조체다.
`.asObservable()`을 사용해 다시 Observable로 돌아갈 수 있음
Trait은 좁은 범위의 Observable을 선택적으로 사용할 수 있다.

### Single
* `.success(value)` 또는 `.error` 이벤트를 방출한다.
*  `.success(value)` = `.next` + `.completed`
*  성공 또는 실패로 확인되는 1회성 프로세스 ex) 데이터 다운로드, 디스크에서 데이터 로딩

### Completable
- `.completed` 또는 `.error` 만을 방출하며, 이 외 어떠한 값도 방출하지 않는다.
- 사용: 연산이 제대로 완료되었는지만 확인하고 싶을 때 (예. 파일 쓰기)

### Maybe
- 0개 또는 1개의 element를 포함하는 Observable sequence
- Maybe는 Single과 유사하지만 아무런 값을 방출하지 않고 complete되더라도 오류가 발생하지 않는다.
- `success(value)`, `completed`, `error` 이벤트 3가지를 방출 할 수 있습니다.





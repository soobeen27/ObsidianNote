
`Observable`은 기본적으로 값을 내보내기만 하고 값을 받을 수 없다.
 새로운 값을 전달하는 기능을 원한다면 `Observable` 대신 `Subject` 사용해야 한다.
 즉 Observable이자 Observer인 것이 필요하다면 Subject를 사용해야한다.

## Subject
* Subject = Observable + Oberserver
* Subject는 `.next` 이벤트를 받고, subscriber에게 방출한다.

**예시와 함께 알아보자**

```Swift
// 1
let subject = PublishSubject<String>()

// 2
subject.onNext("Is anyone listening?")

// 3
let subscriptionOne = subject
	.subscribe(onNext: { string in
		print(string)
	})

// 4
subject.onNext("1") // print: 1

```

1. PublishSubject를 만들었다.
2. onNext를 해도 아무것도 프린트되지 않는다.
3. subscribe를 해도 아무것도 프린트되지 않는다.
	* PublishSubject는 구독이후의 값만을 방출한다.
	* 그러므로 "Is anyone listening?"은 방출하지 못한다.
4. 구독 이후에 이벤트를 전달하면 방출한다.


## Subject의 종류
* **PublishSubject**: 빈 상태로 시작하여 새로운 값만을 subscriber에게 방출한다.
* **BehaviorSubject**: 하나의 초기값을 가진 상태로 시작하여, 새로운 subscriber에게 초기값 또는 최신값을 방출한다.
* **ReplaySubject**: 버퍼를 두고 초기화하며, 버퍼 사이즈 만큼의 값들을 유지하면서 새로운 subscriber에게 방출한다.
* **Variable**: `BehaviorSubject`를 래핑하고, 현재의 값을 상태로 보존한다. 가장 최신/초기 값만을 새로운 subscriber에게 방출한다.



### PublishSubject
* PublishSubject는 구독된 순간 새로운 이벤트 수신을 알리고 싶을 때 용이하다.
* 이런 활동은 구독을 멈추거나, `.complete` , `.error` 이벤트를 통해 Subject가 완전 종료될 때까지 지속된다.

![[Pasted image 20241007153937.png]]

### BehaviorSubject
* BehaviorSubject는 마지막 .next 이벤트를 새로운 구독자에게 반복한다는 점만 빼면 PublishSubject와 유사하다.
* BehaviorSubject는 항상 최신의 값을 방출하기 때문에 초기값 없이는 만들 수 없다. 반드시 초기값이 있어야 한다.
* BehaviorSubject는 뷰를 가장 최신의 데이터로 미리 채우기 용이하다.

![[Pasted image 20241007160135.png]]


## Relay

- onComplete와 onError에 의해 이벤트 스트림이 종료되지 않도록 wrapping된 Subject
- Next 이벤트만 받고 Error와 Complete를 받지않아 dispose하기 전까지 이벤트 계속 받음
- UI 이벤트를 처리하는데에 사용
- 이벤트를 전달할때 onNext가 아닌 accept사용
- RxCocoa에서 제공되는 기능
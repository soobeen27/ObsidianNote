# subscribe

- 파라미터로 onNext, onError, onComplete, onDisposed를 받고 각 이벤트에 대한 처리가 가능하다.
- 기본값이 nil로 지정되어 있기 때문에 각 이벤트들은 상황에 따라 필요한 것만 사용이 가능하다.
- **에러처리가 가능**하다는 것이 가장 중요하고, **쓰레드**지정이 가능하다.

# bind
```Swift
/**
    Subscribes an element handler to an observable sequence.
    In case error occurs in debug mode, `fatalError` will be raised.
    In case error occurs in release mode, `error` will be logged.

    - parameter onNext: Action to invoke for each element in the observable sequence.
    - returns: Subscription object used to unsubscribe from the observable sequence.
    */
    public func bind(onNext: @escaping (Element) -> Void) -> Disposable {
        self.subscribe(onNext: onNext,
                       onError: { error in
                        rxFatalErrorInDebug("Binding error: \\(error)")
                       })
    }
```

> 내부적으로 subscribe를 사용하고 있지만 onNext만 처리하므로 에러처리가 불가능하다.

> [!note] onNext만 처리가능한 subscribe이다.

---
# drive
```Swift
/** Subscribes an element handler, a completion handler and disposed handler to an observable sequence. 

This method can be only called from `MainThread`. 

Error callback is not exposed because `Driver` can't error out. 

	- parameter onNext: Action to invoke for each element in the observable sequence. 
	- parameter onCompleted: Action to invoke upon graceful termination of the observable sequence. gracefully completed, errored, or if the generation is canceled by disposing subscription) 
	- parameter onDisposed: Action to invoke upon any type of termination of sequence (if the sequence has gracefully completed, errored, or if the generation is canceled by disposing subscription) 
	- returns: Subscription object used to unsubscribe from the observable sequence. 
	*/ 
	public func drive( 
		onNext: ((Element) -> Void)? = nil,
		onCompleted: (() -> Void)? = nil, 
		onDisposed: (() -> Void)? = nil ) -> Disposable { 
			MainScheduler
			.ensureRunningOnMainThread(errorMessage: errorMessage) 
			return self.asObservable()
			.subscribe(onNext: onNext, 
				onCompleted: onCompleted, 
				onDisposed: onDisposed) 
	}
```

> 파라미터로 onNext, onComplete, onDisposed를 받고 각 이벤트에 대한 처리가 가능
> **에러처리가 불가능**, **메인 스레드에서만 동작** -> 주로 UI관련 작업에서 사용

`Driver<Element>`에서만 사용가능하고 `ObservableType`에서는 사용하지 못한다.
`ObservarbleType`에서 사용하려면 `asDrvier()` 메서드를 사용해야한다.

---
## Driver

> RxCocoa에 구현되어 있는 UI에 특화된 Observable
> MainScheduler에서 사용한다. 
> **error**를 방출하지 않으며 Observable을 Wrapping한다.

#### 특징 
- 종료되지 않는다.
- MainScheduler에서 돌아간다
- 구독 또한 MainScheduler에서 하는것을 권장
- Driver의 구독 메서드가 **drive**이다.

---

# 한눈에 보기

|     | subscribe                       | bind                                                        | drive                                    |
| --- | ------------------------------- | ----------------------------------------------------------- | ---------------------------------------- |
| 특징  | 에러 처리 가능              쓰레드 지정 가능 | 에러 처리 불가                           onNext만 구현한 subscribe 형태 | 에러 처리 불가                    메인 쓰레드에서만 동작 |
| 사용  | 에러 처리하는 경우                      | 에러 처리 필요없는 경우                    주로 UX subscribe용으로 사용      | UI 작업                                    |

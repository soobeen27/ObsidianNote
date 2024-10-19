
# Transforming Observables

> 방출된 옵저버블을 변환하는 연산자

## map
* Observable에서 방출된 각각의 아이템에 함수를 적용시켜 변환하는 연산자

![[Pasted image 20241014103409.png]]

```Swift
let observable = Observable.of(1, 2, 3)
observable
    .map { $0 * 2 }
    .subscribe(onNext: { print($0) }) 
// 출력: 2, 4, 6
```

---
## flatMap
- Observable에서 방출된 각각의 아이템들을 Observable들로 변환 후 
- 하나의 Observable로 평면화합니다.

![[Pasted image 20241014103800.png]]

즉  1, 2, 3의 아이템을 가진 observable에 `.flatMap { $0 * 2 } ` 을 하게되면
2, 4, 6을 아이템으로 가진 observable이 세개가 생긴뒤 각각 아이템을 방출한 뒤 한개의 
observable로 방출된다.

>[!question] 그럼 map이랑 다른점이 뭘까?
> flatMap은 
> - 한번에 여러 스트림에 사용 할 수 있다.
> - 여러 스트림에서 방출된 아이템에 대해 누락 없이 구독이 가능하다.

예시를 통해 좀 더 알아보자
```Swift
let observable1 = Observable.of("A", "B", "C")
let observable2 = Observable.of("a", "b", "c")
```

이렇게 두개의  Observable이 있고 Aa, Ab, Ac, Ba.. 를 가진 Observable을 얻고 싶을 때

```Swift
observable1.flatMap { uppercase: String -> Observable<String> in
	return observable2.map { uppercase + $0 }
}
```
1. `observable1`의 각 아이템 `uppercase`들을 이용해 `flatMap`
2. `observable2.map`을 통해 `observable2`의 각 아이템에 `uppercase`를 합친것을 아이템으로 갖는 `observable`이 반환된다.
3. `uppercase`는 세개이기 때문에 세개의 `observable`이 반환된다.
4. `flatMap`을 썼기 때문에 세개의 `observable`들이 하나의 `observable`로 평면화 된다.

>[!note] 
> flatMap은 하나의 Observable을 가공하는 클로저 내부에서 또 다른 Observable을 사용할 때 편하다.

만일 `flatMap`을 사용하지 않고 `map`만을 사용한다면 중첩의 늪에 빠지게 될 가능성이 크다.
`Observable<Observable<String>>` 과 같은 타입을 볼 수도..

---

# Filtering Observables

> 원본 Observable에서 선택한 아이템들만 방출하는 연산자

## filter

- 테스트에 통과한 아이템들만 방출하는 연산자

![[Pasted image 20241014111941.png]]

```Swift
let observable = Observable.of(1, 2, 3, 4, 5)
observable
    .filter { $0 % 2 == 0 }  // 짝수만 통과
    .subscribe(onNext: { print($0) }) 
// 출력: 2, 4
```

---

## take

- 방출된 아이템들중 n개의 아이템만 방출하는 연산자
![[Pasted image 20241014112518.png]]

```Swift
let observable = Observable.of(1, 2, 3, 4, 5)
observable
    .take(3)
    .subscribe(onNext: { print($0) })
// 출력: 1, 2, 3
```

---

## skip

- 첫 n개를 넘긴뒤 방출하는 연산자
![[Pasted image 20241014112957.png]]

```Swift
let observable = Observable.of(1, 2, 3, 4, 5)
observable
    .skip(2)
    .subscribe(onNext: { print($0) })
// 출력: 3, 4, 5
```

---

# Combining Observables

> 여러개의 Observable들을 하나로 만드는 연산자

## merge

- 여러개의 Observable들을 결합하여 방출하는 연산자
![[Pasted image 20241014113520.png]]

```Swift
let observable1 = Observable.of(1, 2, 3)
let observable2 = Observable.of(4, 5, 6)
Observable.merge(observable1, observable2)
    .subscribe(onNext: { print($0) }) 
// 출력: 1, 2, 3, 4, 5, 6
```

>[!note] concat과 다른점
> `merge`는 아이템들이 간섭될 수(interleave) 있고 `concat`은 뒤에 붙여져서 합병된다.

---
### Concat
- merge와 같지만 아이템이 간섭되지 않고(interleave) 병합된다.
![[Pasted image 20241014114137.png]]

쉽게 그냥 직렬로 하나 이상의 Observable을 합친다고 생각하면 된다.

```Swift
let observable1 = Observable.of(1, 2, 3)
let observable2 = Observable.of(4, 5, 6)
let observable3 = Observable.of(7, 8, 9)
        
Observable.concat([observable1, observable2, observable3])
.subscribe(onNext: { print($0) })
// 1 2 3 4 5 6 7 8 9
```


# combineLatest

> 둘 중 하나의 Observable에서 아이템이 방출되었을 때, 각 Observable에서 최신의 아이템들을
> 결합하여 특정 함수를 거쳐 결과를 방출한다.

![[Pasted image 20241017212314.png]]

```Swift
let observable1 = BehaviorSubject(value: "A")
let observable2 = BehaviorSubject(value: "1")

Observable.combineLatest(observable1, observable2) { "\($0)\($1)" }
    .subscribe(onNext: { print($0) })

observable1.onNext("B")
observable2.onNext("2")
// 출력: A1, B1, B2
```
---
# zip

>여러개의 Observable들에서 방출된 아이템들을 함수를 거쳐 결합하여 방출한다.

![[Pasted image 20241017212945.png]]

```Swift
let observable1 = Observable.of("A", "B", "C")
let observable2 = Observable.of(1, 2, 3)

Observable.zip(observable1, observable2) { "\($0)\($1)" }
    .subscribe(onNext: { print($0) })
// 출력: A1, B2, C3
```

> [!note] combineLatest와 차이점
>  combineLatest와 zip은 유사하지만 
>  zip은 오직 source가 되는 Observable들에서 아직 zip되지 않은 item들이 방출되었을 때만 zip하여 방출한다.
>  그에 비해 combineLatest는 아무 source Observable에서 아이템을 방출할 때마다 가장 최근의 아이템들을 함수를 거져 방출한다.

---

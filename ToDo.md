## FeedView
- 비디오 압축
	- https://img.ly/blog/working-with-large-video-and-image-files-on-ios-with-swift/#resizingavideo
	- avfoundation viedeo resize
- 파이어베이스 업로드


### 암장 선택 -> 검색 페이지 모달?

### 섹터 선택 -> 모달? 피커뷰?

### 난이도 선택 -> 모달? 피커뷰?


## Firestore
데이터 유형지원 -> 1mb까지 그냥 uiimage -> data 해서 저장하는것 고려

![[스크린샷 2024-08-28 오후 3.38.11.png]]



```swift

```


## Trouble
로그인할때 유저 컬렉션의 문서명을 토큰으로 할 지 유저네임으로 할지

토큰 
	장점:  로그인이 빠름
	단점: ios내에서 유저 이름만으로는 유저 정보에 접근하기 힘듬

유저네임
	장점: ios내에서 유저를 찾을 때 참조가 쉽다
	단점: 로그인시 모든 문서를 참조해서 반복문을 통해 찾아야함

### 해결
	user collection은 


계정을 삭제할 때
cloud function으로는 하위 콜렉션 까지만 삭제하고 + 안에 레퍼런스 파일들도 삭제 ㄱ
storage 파트는 그냥 문서하나 파서 deletedAccount에다가 넣어놓자

### 포스트에 썸네일 가져오기

authoruid
TWvz2vI9ikUtbuboWAjXoMEMH0Q2
postuid
"25A6183F-EFDA-48EB-BCE8-03BD5BC8E169"
media
/media/9666FE8B-646A-4F80-95B3-61593B0A91DC
url
"https://firebasestorage.googleapis.com:443/v0/b/weclimb-db3ca.appspot.com/o/users%2FTWvz2vI9ikUtbuboWAjXoMEMH0Q2%2F192599CC-7D06-4207-918B-24401ABD4AF8.mp4?alt=media&token=ed4109a8-503d-4b75-9686-b07ed853f39d"
storage
192599CC-7D06-4207-918B-24401ABD4AF8.mp4

쿠팡을 추천합니다!
식품용 기름종이
https://link.coupang.com/a/bWB5uV
![[스크린샷 2024-08-28 오후 3.38.11.png]]
# Firestore 구조

유저(콜렉션)
	토큰(문서)
		피드(콜렉션)
			각피드(문서)
				썸네일이미지
				이미지,비디오 어레이 (url)
				캡션 (데이터)
				좋아요 (데이터)	
		아우스토큰
		프로필 사진
		유저명
		신장
		암리치
		지역
		팔로우리스트(어레이 -> 유저 레퍼런스)
		팔로워리스트(어레이 -> 유저 레퍼런스)

암장(콜렉션)
	암장명(문서)
		프로필 사진
		주소
		섹션
		난이도
	문제(콜렉션)
		이미지, 비디오 어레이(url)
	답지(컬렉션)
		비디오 어레이(url)
	


## 로그인
- 계정있는지 체크 메소드
- 회원가입 메소드
- 로그인 메소드

## 유저
- 유저이름으로 유저 정보 가져오는 메소드
- 정보 수정 메소드
- 중복체크 메소드
- 유저삭제 -> 유저 글삭제, 댓글삭제

## 포스트
	프로필사진(유저)
	유저명(유저)
	피드
		미디어 어레이
		캡션
		좋아요

- 포스트 업로드 메소드
- 유저네임으로 포스트 정보 가져오는 메소드

## 댓글
	댓글 쓰기
	댓글 가져오기
		postUID를 통해가져올까?

## 피드

- ~~팔로우한 유저, 암장 기준 일주일 이내에 올린 포스트 가져오기~~
- 모든 피드중에 최신피드

```swift
           func playVideo() {
        let videoURLString = "https://firebasestorage.googleapis.com/v0/b/weclimb-db3ca.appspot.com/o/users%2FF5267DB2-BD25-410B-84EF-2CBC016791B2.mp4?alt=media&token=0697bb9f-c38b-4f7f-809c-26fa0470bff1"

                // Ensure the URL is valid
                guard let videoURL = URL(string: videoURLString) else {
                    print("Invalid video URL")
                    return
                }

                let player = AVPlayer(url: videoURL)
                let playerViewController = AVPlayerViewController()
                playerViewController.player = player

                present(playerViewController, animated: true) {
                    player.play()
                }
    }
        ```


```swift
    func compressVideoTo720p(url: URL, completion: @escaping (URL?) -> Void) {
        let config = FYVideoCompressor.CompressionConfig(videoBitrate:1000_000,
                                                         videomaxKeyFrameInterval: 10,
                                                         fps: 24,
                                                         audioSampleRate: 44100,
                                                         audioBitrate: 128_000,
                                                         fileType: .mp4,
                                                         scale: CGSize(width: 1080, height: -1))
        FYVideoCompressor().compressVideo(url, config: config) { result in
            switch result {
            case .success(let compressedVideoURL):
                completion(compressedVideoURL)
            case .failure(let error):
                print(error)
            }
        }
    }
```


# 19092024 처음부터

## 유저(루트)

* 경로: users/user(uid)
* 필드:
	* userName: 사용자 이름
	* profileImage: 프로필 이미지 url
	* posts: [DocumentReference] : 사용자가 올린 게시물 레퍼런시 배열
	* comments: [DocumentReference] : 사용자가 남긴 댓글 레퍼런시 배열
	* registerantionDate: Date
	* userRole: String
	* armReach: String?
	* height: String?
	* followers: [DocumentReference]
	* followings: [DocumentReference]

## 포스트(루트)

* 경로: posts/post(postUID)
* 필드:
	* authorUID: 작성자
	* caption: 글
	* like
	* gym?
	* creationDate: 게시물 작성 시간
	* media: 미디어 배열
		* 객체로 저장
```json
{
  "authorUID": "user123",
  "content": "Here is a new post!",
  "timestamp": "2024-09-19T12:34:56Z",
  "media": [
    {
      "url": "gs://bucket/images/image1.jpg",
      "type": "image",
      "sector": "A",
      "grade": "1"
    },
    {
      "url": "gs://bucket/videos/video1.mp4",
      "type": "video",
      "sector": "B",
      "grade": "2"
    }
    // 최대 10개까지 추가
  ]
}
```


## comments

* 경로: posts/post/comments(col)/comment(uid)
* 필드: 
	* authorUID
	* content
	* time: 작성 시간
	* like
	* postRef: 댓글이 달린 게시물에 대한 레퍼런스


## Firebase storage 구조

* 경로:
	* users/userUID/fileName

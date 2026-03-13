# API 명세

## 인증

### POST /auth/signup
- 설명: 회원가입
- 인증: 불필요
- request
  - email
  - username
  - password
- response
  - user_id
  - email
  - username
- error
  - 400: 잘못된 요청
  - 409: 이메일 또는 username 중복

### POST /auth/login
- 설명: 로그인
- 인증: 불필요
- Rate Limit: IP 기준 5회/1분
- request
  - email
  - password
- response
  - access_token
  - token_type
- error
  - 401: 로그인 실패

### GET /me
- 설명: 내 정보 조회
- 인증: 필요
- response
  - id
  - email
  - username
  - bio
  - profile_image_url

---

## 사용자

### GET /users/{username}
- 설명: 사용자 프로필 조회
- 인증: 불필요
- response
  - id
  - username
  - profile_image_url
  - follower_count
  - following_count
  - post_count

### POST /users/{user_id}/follow
- 설명: 사용자 팔로우
- 인증: 필요
- error
  - 400: 자기 자신 팔로우
  - 404: 대상 사용자 없음
  - 409: 이미 팔로우 중

### DELETE /users/{user_id}/follow
- 설명: 사용자 언팔로우
- 인증: 필요
- error
  - 404: 팔로우 관계 없음

---

## 게시물

### POST /posts
- 설명: 게시물 생성
- Rate Limit: 사용자 기준 10회/1분
- 인증: 필요
- request
  - image_url
  - caption
- response
  - post_id
  - created_at
- error
  - 400: image_url 누락

### GET /posts/{post_id}
- 설명: 게시물 단건 조회
- 인증: 불필요
- response
  - id
  - author
  - image_url
  - caption
  - like_count
  - comment_count
  - created_at
- error
  - 404: 게시물 없음

### DELETE /posts/{post_id}
- 설명: 게시물 삭제
- 인증: 필요
- error
  - 403: 작성자 아님
  - 404: 게시물 없음

---

## 피드

### GET /feed
- 설명: 팔로우한 사용자의 게시물 피드 조회(cursor: 다음 조회를 어디서 이어갈 지, limit: 한 번에 몇 개를 가져올 지)
- 인증: 필요
- query
  - cursor (optional)
  - limit (default 20)
- response
  - next_cursor
  - items[]
- error
  - 401: 인증 없음

---

## 좋아요

### POST /posts/{post_id}/like
- 설명: 게시물 좋아요
- 인증: 필요
- error
  - 404: 게시물 없음
  - 409: 이미 좋아요함

### DELETE /posts/{post_id}/like
- 설명: 게시물 좋아요 취소
- 인증: 필요
- error
  - 404: 좋아요 이력 없음

---

## 댓글

### POST /posts/{post_id}/comments
- 설명: 댓글 작성
- 인증: 필요
- request
  - content
- error
  - 400: 빈 댓글
  - 404: 게시물 없음

### GET /posts/{post_id}/comments
- 설명: 댓글 목록 조회
- 인증: 불필요
- query
  - cursor (optional)
  - limit (default 20)
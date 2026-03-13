# ERD 초안

## users
- id: bigint, PK
- email: varchar, UNIQUE, NOT NULL
- username: varchar, UNIQUE, NOT NULL
- password_hash: varchar, NOT NULL
- profile_image_url: varchar, NULL
- created_at: timestamp, NOT NULL

## posts
- id: bigint, PK
- user_id: bigint, FK -> users.id, NOT NULL
- image_url: varchar, NOT NULL
- caption: text, NULL
- created_at: timestamp, NOT NULL
- updated_at: timestamp, NOT NULL

## follows
- id: bigint, PK
- follower_id: bigint, FK -> users.id, NOT NULL
- following_id: bigint, FK -> users.id, NOT NULL
- created_at: timestamp, NOT NULL
- UNIQUE(follower_id, following_id)

## likes
- id: bigint, PK
- user_id: bigint, FK -> users.id, NOT NULL
- post_id: bigint, FK -> posts.id, NOT NULL
- created_at: timestamp, NOT NULL
- UNIQUE(user_id, post_id)

## comments
- id: bigint, PK
- user_id: bigint, FK -> users.id, NOT NULL
- post_id: bigint, FK -> posts.id, NOT NULL
- content: text, NOT NULL
- created_at: timestamp, NOT NULL
- updated_at: timestamp, NOT NULL

## 관계 정리
- users 1 : N posts
- users N : N users (follows)
- users 1 : N likes
- posts 1 : N likes
- posts 1 : N comments
- users 1 : N comments

## 설계 메모
- 팔로우, 좋아요는 관계 테이블로 분리
- 좋아요 중복 방지를 위해 UNIQUE(user_id, post_id)
- 피드 조회는 follows + posts 조합으로 처리
- caption은 선택값
- 게시물은 MVP 기준 이미지 1장만 허용
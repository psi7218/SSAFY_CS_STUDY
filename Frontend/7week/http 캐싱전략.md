# 📦 HTTP 캐시 전략 정리

> HTTP 통신에서 **Cache가 사용되는 이유**를 시각 자료로 쉽게 이해해보자.

---

## 🧨 캐시가 없을 경우 (Always Network)

![no-cache-1](https://velog.velcdn.com/images/psi7218/post/89af20f1-0870-4113-b4ad-661d50b9800a/image.png)  
![no-cache-2](https://velog.velcdn.com/images/psi7218/post/17e172d2-e334-4cf7-830d-c715a26c8e4b/image.png)

- 데이터가 바뀌지 않아도 **매번 다운로드**
- 네트워크는 **느리고 비싸며**
- **브라우저 로딩 지연**, 사용자 경험 저하

---

## ✅ 캐시 적용 (Fresh Cache: `Cache-Control`)

![cache-1](https://velog.velcdn.com/images/psi7218/post/22004019-aa96-4132-b56f-3149cf1c4c97/image.png)  
![cache-2](https://velog.velcdn.com/images/psi7218/post/e6a51459-f5f8-46b7-a09d-b9b8da82439e/image.png)  
![cache-3](https://velog.velcdn.com/images/psi7218/post/818c736c-e7b0-4f16-82d8-62fa325422c3/image.png)

- `Cache-Control: max-age=60` 등으로 유효시간 설정
- 브라우저는 응답을 **로컬에 저장**
- 해당 시간 내 재요청 시, **네트워크 사용 없이 빠른 응답**

---

## ⏳ 캐시 시간 초과 → 다시 다운로드?

![expired](https://velog.velcdn.com/images/psi7218/post/6a57882b-19ef-495c-92ee-1b90b8580641/image.png)

- 유효시간이 지나면 캐시는 무효
- 다시 네트워크 통해 다운로드
- 하지만 _**데이터가 안 바뀌었으면?**_

---

## 🧠 검증 기반 캐시 (조건부 요청 + 헤더 기반)

![verify-1](https://velog.velcdn.com/images/psi7218/post/35e671bc-dbda-4011-9b91-ed313a923e36/image.png)  
![verify-2](https://velog.velcdn.com/images/psi7218/post/d78a0016-d22e-4aea-98ad-c479003f4d6d/image.png)  
![verify-3](https://velog.velcdn.com/images/psi7218/post/54f3d009-0ac1-40c8-9af0-a905c7e9c36f/image.png)

- `Last-Modified` → `If-Modified-Since`
- `ETag` → `If-None-Match`
- 서버가 변경되지 않았다고 판단하면 **304 Not Modified** 응답
- **본문 없이 헤더만 전송 → 캐시 재활용**

---

## 🧾 검증 헤더의 동작 흐름

![verify-4](https://velog.velcdn.com/images/psi7218/post/066a050b-086b-4124-b62a-32a591247b3a/image.png)  
![verify-5](https://velog.velcdn.com/images/psi7218/post/7c37d99b-0330-400e-b881-c644a2404fa8/image.png)

- 조건부 요청을 통해 트래픽을 줄임
- 캐시를 믿되, 검증은 서버에 맡기는 전략

---

## ⚠️ Last-Modified의 단점

![lastmod-limit](https://velog.velcdn.com/images/psi7218/post/a67fe0d6-b912-490f-acde-c5ed41eebc77/image.png)

- 1초 미만 단위 조정 불가
- 날짜 기반이라 정확도가 떨어짐
- 데이터 내용은 동일하지만 수정 시간이 달라지면 무효화됨

---

## 🧬 ETag의 활용

![etag-1](https://velog.velcdn.com/images/psi7218/post/a3e37921-681e-4fe6-9fc9-04ddaa92de81/image.png)  
![etag-2](https://velog.velcdn.com/images/psi7218/post/5af99217-13ed-4825-8901-652727084645/image.png)  
![etag-3](https://velog.velcdn.com/images/psi7218/post/37291146-49cf-4858-bbed-db0f3d5803c9/image.png)  
![etag-4](https://velog.velcdn.com/images/psi7218/post/c2f756b5-81e5-4ebe-bb9b-ac766c77c8a2/image.png)

- 리소스에 **고유 식별자(해시)**를 부여
- 내용이 바뀌면 해시도 바뀌고, 캐시 무효화가 정확하게 일어남

---

## 🎯 캐시 전략 비교 요약

| 전략                     | 방식                        | 장점                | 단점                       |
| ------------------------ | --------------------------- | ------------------- | -------------------------- |
| `Cache-Control`          | 만료 시간까지 브라우저 판단 | 빠름, 간단          | 만료 후 무조건 재요청      |
| `ETag` / `Last-Modified` | 서버가 변경 여부 판단       | 정확, 트래픽 최소화 | 비교 요청 필요 (약간 느림) |

---

## ✅ 마무리

- HTTP 캐시는 **속도와 서버 부하**를 동시에 최적화하는 도구
- 전략적으로 잘 활용하면 사용자 경험과 비용 효율을 함께 개선할 수 있음
- 실무에서는 상황에 맞게 **Cache-Control + ETag 조합**을 많이 사용함

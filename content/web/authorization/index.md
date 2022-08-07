---
emoji      : 🌐
title      : 인증과 인가(2) - 인가란?
date       : '2022-02-22 23:00:00'
author     : 윤명국
tags       : web
categories : web
---

## 인가 (Authorization)

### 1. 인가란?

요청을 보낸 유저가 그 서비스를 사용할 수 있는 유저인지를 확인하는 절차이다.

### 2. 인가 절차

1. 서버에 로그인을 성공한 유저는 인증을 받았다는 표시로 그 서버가 발행한 토큰을 발급받는다.
보통 이 토큰을 access token 이라고 한다. 이 token에는 user id와 같이 아주 중요한 정보가 아니면서도(예를 들어 주민등록번호 같은) 유저를 확실하게 구분할 수 있는 정보가 있어야 한다.
2. 인증을 받은 유저는 request를 보낼 때 request header에 보통 'Authorization' 라는 이름으로 token 정보를 담아서 보낸다.
3. 서버는 이 request에 담긴 token 정보를 복호화하여 우리 서버에서 발급한 token이 맞는지 그리고 token에 담긴 user id를 확인할 수 있다.
4. token에서 얻은 user id를 사용해서 DB에서 해당 유저의 권한(permission)을 확인한다.
5. 해당 유저가 그 요청을 사용할 수 있는 권한을 가지고 있으면 해당 요청을 처리한다.
6. 유저가 권한을 가지고 있지 않은 유저라면 Unauthorized Response(401) 에러 또는 적절한 다른 에러 코드를 보낸다.

### 3. JSON Web Token (JWT)

#### 3-1. JWT란?

1. access token을 생성하는 방법에는 여러가지가 있는데, 그 중 가장 널리 사용되는 기술 중 하나가 바로 JSON Web Token(JWT)이다.
2. JSON Web Token (JWT) 은 웹표준 (RFC 7519) 으로서 두 개체에서 JSON 객체를 사용하여 가볍고 자가수용적인 (self-contained) 방식으로 정보를 안전성 있게 전달해주고, 사용자에 대한 속성을 저장하는 Claim 기반의 Web Token이다.
3. 자가 수용적 이라는 말은 JWT 는 필요한 모든 정보를 자체적으로 지니고 있다는 것을 뜻한다.
4. 토큰에 대한 기본정보(header), 전달 할 정보(payload), 그리고 토큰이 검증됐다는것을 증명해주는 Signature 를 포함하고있다.
5. JWT는 양방향 해쉬 알고리즘이 적용되어 있는데, 이를 이용하여 프론트엔드에서는 API요청 시 암호화 된 토큰 정보를 보내고 백엔드에서는 이 토큰을 복호화해서 유저의 권한을 확인한다.

#### 3-2 JWT 구조

![jwt_image](./jwt.jpeg)

##### Header

- Header 는 두가지의 정보를 지니고 있다.
- typ: 토큰의 타입을 지정. JWT는 JWT이다.
- alg: Signature 해싱 알고리즘을 지정합니다. 해싱 알고리즘으로는 보통 HMAC-SHA256 혹은 RSA 가 사용되며, 이 알고리즘은 토큰을 검증 할 때 사용되는 signature 부분에서 사용된다.

##### Payload

- Paylaod에는 토큰을 통해 전달할 내용이 담겨져있다.
- 여기에 담긴 하나의 key-value 쌍을 'Claim'이라고 부르고, Registered, Public, Private의 3가지 유형으로 나뉜다.
- 보통 만료일시, 발급일시, 발급자, 권한정보 등이 포함된 공개 클래임
- 그리고 클라이언트와 서버간의 협의 하에 사용하는 비공개 클래임이 있다.
- Header와 Payload 부분은 암호화하지 않고 base64로 인코딩만 하기 때문에 웹에 접속한 호스트라면 누구나 볼 수 있다. 따라서 민감함 정보는 넣지 않고, user id와 값은 PK값을 전달한다.

##### Signature

- signature(서명)은 인코딩된 header 와 payload 를 합쳐 header의 signature 알고리즘 정보(alg)를 가져와 암호화하여 생성한다
- 만약 헤더정보의 알고리즘이 HS256 일 경우 아래 예시와 같은 signature가 생성된다.
- 프론트엔드에서 JWT를 백엔드 API 서버로 전송하면 서버에서는 전송받은 JWT의 Signature 부분을 복호화 하여 이 서버에서 생성한 JWT가 맞는지를 확인한다.(내가 만든 건 복호화 가능하기 때문)
- 마치 계약서의 위변조를 막기 위해 서로 사인하는 것과 같다고 보면된다.

```toc

```

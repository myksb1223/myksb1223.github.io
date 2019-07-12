---
layout: post
title:  "JWT 이해하기"
date:   2019-07-12 18:41:00
author: Seungbeom Kim
categories: develop_diary
tags:	JWT JSONWebToken
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)에 JWT를 적용시키기 위해서 공부를 해보았다.

### JWT란 무엇인가?

JWT(JSON Web Token)은 JSON 객체를 사용하여 당사자들 사이에서 안전하게 정보를 전달하기 위해서 작고, 독립적인 방법을 정의해 놓은 공개 표준이다. 정보는 디지털 방식으로 서명되기 때문에 확인 및 신뢰될 수 있다. JWT는 **HMAC** 알고리즘 또는 **RSA**나 **ECDSA**를 사용한 공개키/비밀키 쌍으로 서명될 수있다.

비록 JWT는 당사자들 사이의 비밀 유지를 제공하기 위해서 암호화 될 수 있지만, 서명된 토큰이 중요하다. 서명된 토큰은 해당 토큰에 포함된 요청들의 진실성을 확인할 수 있는 반면에 암호화된 토큰은 다른 당사자로부터 해당 요청을 숨긴다. 토큰이 공개키/비밀키 쌍을 사용하여 서명될 때, 그 서명은 비밀 키를 가진 그 당사자가 서명한 사람인지 또한 증명한다.

### JSON Web Token은 언제 사용해야 하는가?

JSON Web Token이 유용한 경우 :

- **Authorization** : JWT를 사용하는 가장 일반적인 시나리오이다. 일단 사용자가 로그인하면, 사용자에게 그 토큰에 허가된 리소스, 서비스, 경로에 대한 접근을 허용하는 각각의 차후 요청은 JWT를 포함할 것이다.
JWT의 작은 오버헤드와 그 기능이 매우 쉽게 다른 도메인간에 사용될 수 있기 때문에, 요즘에는 Single Sign On이 JWT를 넓게 사용하는 형태이다.

- **Infomation Exchange** : JSON Web Token 당사자들 간에 안전하게 정보 전달을 하기 위한 좋은 방법이다. JWT 서명될 수 있기 때문에 - 예를 들어, 공개키/비밀키 쌍의 사용 -, 보낸이를 확신할 수 있다. 또한, 서명은 해더와 페이로드를 사용해서 계산되기 때문에, 내용이 함부로 변경되었는지 아닌지 확인할 수 있다.

### JSON Web Token 구조는 무엇인가?

JWT의 작은 형태에서, JSON Web Token은 `.`을 기준으로 세 부분으로 구성된다.

- Header
- Payload
- Signature

그러므로, JWT 일반적으로 아래와 같은 형태를 가진다.

```
xxxxx.yyyyy.zzzzz
```

#### Header

해더는 일반적으로 두 부분으로 구성된다. : 토큰의 타입(JWT), 서명에 사용되는 알고리즘(HMAC SHA256, RSA ...).

예를 들면 :

```
{
  "alg": "HS256",
  "typ": "JWT"
}
```

그러면, 이 JSON은 JWT의 첫 부분을 구성하는 **Base64Url**로 인코딩된다.

#### Payload

토큰의 두 번째 부분은 요청을 포함하고 있는 페이로드이다. 요청들은 엔티티(일반적으로는 사용자)와 추가적 데이터에 대한 서술이다. 3가지 종류의 요청이 있다. : *registered, public, private*

- **Registered claims** : 유용하고 상호 정보 교환이 가능한 요청의 집합을 제공하기 위한 의무적이진 않지만 요구되는 미리 정의된 요청의 집합이다.
**iss**(issuer), **exp**(expiration time), **sub**(subject), **aud**(audience), [others](https://tools.ietf.org/html/rfc7519#section-4.1) 등이 있다.
    ```
    JWT는 소형을 의미하기 때문에 이 claim의 이름은 오직 3글자의 길이만 가진다.
    ```
- **Public claims** : JWT를 사용하는 사람들에 의해 자유롭게 정의될 수 있다. 하지만 충돌을 피하기 위해서 그들은 [IANA JSON Web Token Registory](https://www.iana.org/assignments/jwt/jwt.xhtml)나 충돌을 피할 수 있는 이름을 포함한 URI로 정의되어야 한다.
- **Private claims** : *registered*와 *public* claims에 해당하지 않으면서 서로가 사용하겠다고 동의한 당사자간의 정보 공유를 위해 생성된 맞춰진 claim이다.

```
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

그러면 페이로드는 JSON Web Token의 두 번째 부분을 구성하기 위해 **Base64Url**로 인코딩 된다.

```
서명된 토큰에서 이 정보는 비록 변조에 대해서는 보호되었다고 하지만, 누구에게나 읽힐 수 있다. 토큰이 암호화 되기전에는 JWT의 페이로드나 해더 요소에는 비밀 정보를 넣지 않아야 한다.
```

#### Signature

서명 부분을 생성하기 위해서는 인코딩된 해더와 페이로드 그리고 비밀키, 해더에 정의된 알고리즘을 가져와서 서명해야한다.

예를 들면, HMAC SHA256 알고리즘을 사용한다면, 이 서명은 아래와 같은 방법으로 생성될 것이다.

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

이 서명은 오는 길에 메시지가 변경된 것이 없는지 확인하는데 사용된다. 그리고 비밀키와 함께 서명된 토큰의 경우에는 JWT를 보낸이가 누구인지 또한 할 수 있다.

#### Putting all together

결과물은 XML 기반의 표준(SAML)과 비교할 때 더 작게 만들면서, HTML과 HTTP환경에서 쉽게 전달될 수 있는 `.`으로 구분된 3개의 Base64-URL 문자열이다.

아래는 인코딩된 해더, 인코딩된 페이로드, 비밀키로 서명된 부분을 가진 JWT를 보여준다.

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.
TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```

JWT를 생성하거나 이 개념을 실천하고 싶다면, JWT 생성, 확인, 디코딩을 위해서 [jwt.io Debugger](https://jwt.io/)를 사용할 수 있다.

### JSON Web Token은 어떻게 동작하는가?

인증에서 사용자가 자신의 자격을 사용하여 성공적으로 로그인 할 때, JSON Web Token은 반환된다. 토큰이 자격이기 때문에 보안 이슈를 막기위해서는 엄청난 주의가 요구되어야 한다. 일반적으로, 요구된 것 보다 더 오랫동안 토큰을 가지고 있지 않아야 한다.

사용자가 보호된 경로나 리소스에 접근할 때마다 일반적으로 **Bearer** 구조를 사용한 **Authorization** 해더에 사용자는 JWT를 보내야 한다.
해더의 내용은 아래와 같은 형태여야 한다.

```
Authorization: Bearer <token>
```

어떤 경우에는 stateless 인증 메커니즘일 수 있다. 서버의 보호된 경로는 `Authorization` 해더에서 유효한 JWT인지 확인할 것이고 그것이 존재한다면 사용자는 보호된 리소스의 접근이 허가될 것이다. 만약 JWT가 필수 데이터를 포함한다면, 항상 그렇지는 않지만, 몇몇 처리를 위한 데이터베이스 쿼리의 필요가 감소될 것이다.

만약 토큰이 `Authorization` 해더로 전달되었다면, 쿠키를 사용하지 않기 때문에 Cross-Origin Resource Sharing(CORS) 문제가 되지 않는다.

아래의 그림은 어떻게 JWT가 획득되고, 어떻게 API와 리소스에 접근하는데 사용되는지 보여준다.

<img src="{{ site.baseurl }}/assets/develop_diary/JWT_1.png" title="JWT process" class="post-image">

1. 앱 혹은 클라이언트는 인증 서버에 인증을 요청한다. 다른 인증 흐름을 통해 수행된다.
2. 인증이 승인되면, 인증 서버는 앱에 접근 토큰을 넘겨준다.
3. 앱은 보호된 리소스(API)에 접근 할 때 해당 토큰을 사용한다.

토큰과 정보를 변경시킬 수는 없다지만, 서명된 토큰과 함께 해당 토큰 내에 포함된 모든 정보는 다른 사용자나 다른 당사자들에게 노출된다. 그렇기 때문에 토큰에는 비밀 정보를 넣으면 안된다.

### 왜 JSON Web Token을 사용해야 하는가?

**Simple Web Toknes(SWT)** 와 **Security Assertion Markup Language Tokens(SAML)** 와 비교했을 때 **JSON Web Token(JWT)**의 장점에 대해서 이야기해 보면...

JSON은 XML보다 덜 장황하기 때문에 인코딩 되었을 때 그 사이즈가 더 작기에 JWT가 SAML보다 더 작아진다. 이는 JWT가 HTML과 HTTP환경에서 전달되 때 더 좋은 선택이 된다.

보안에 관해서는, SWT는 오직 HMAC 알고리즘을 사용한 공유된 비밀키에 의해서만 대칭적으로 서명될 수 있다. 하지만 JWT와 SAML 토큰은 서명을 위한 X.509 증명서 형태에서 공개키/비밀키 쌍을 사용할 수 있다. 모호한 보안 허점에 대한 소개 없이 XML 디지털 서명으로
XML을 서명하는 것은 JSON 서명의 간단함과 비교할 때 매우 어렵다.

JSON 파서는 직접적으로 객체들과 매핑되기 때문에 대부분의 프로그래밍 언어들에서 흔하다. 정반대로, XML은 본연의 문서-객체 매핑이 없다. 이는 SAML assertions보다 JWT로 작업하는 것이 더 쉽다는 것을 말해준다.

사용법에 관해서는, JWT는 Internet 스케일에서 사용된다. 이는 다양한 플랫폼, 특히 모바일에서의 JSON Web Token의 client-side 처리의 쉬움을 강조한다.

SAML 생성에 관해 궁금하면 [여기](http://samltool.io/)에 가면 알 수 있다.

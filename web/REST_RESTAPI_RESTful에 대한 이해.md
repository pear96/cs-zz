# REST / REST API / RESTful에 대한 이해



## 간단정리

* REST ?!

  >  “웹에 존재하는 모든 자원(이미지, 동영상, DB 자원)에 고유한 URI를 부여해 활용**”**
  >
  > => 자원을 정의하고 자원에 대한 주소를 지정하는 방법론

* REST API ?! 

  > REST 기반으로 서비스 API를 구현한 것

* RESTful ?!

  > REST원리를 따르는 시스템

# REST란?!

![image-20220326170553401](REST_RESTAPI_RESTful에 대한 이해.assets/image-20220326170553401.png)

## REST의 정의

* **“Representational State Transfer”**의 약자, 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미한다.

* 즉, **자원(resource)**의 **표현(representation)**에 의한 상태 전달을 의미

* REST는 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있는 **아키텍처 스타일**

* **HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.**

###  cf) URI vs URL란?!

* URI 는 네트워크 상 자원을 가리키는 일종의 고유 식별자(ID) 이다. 즉 인터넷에 있는 자원을 나타내는 유일한 주소



## 더 알아보기

#### URL /URI

- URI(Uniform resource Identifier) 자원의 위치뿐만 아니라 자원에 대한 고유 식별자로서 URL을 의미를 포함.
- URL(Uniform Resource Locator) 자원이 실제로 존재하는 위치
- **URI가 URL의 상위 개념.**
  (URL이 URI안에 포함. URI 의 하위 개념으로는 URL 말고 URN도 있음.)

#### URL 와URI 구분

- [https://example.com](https://example.com/) 의 경우

  >  [https://example.com](https://example.com/) 이라는 서버를 나타내기 때문에 URL이면서 URI

- https://example.com/skin 의 경우

  >  example 서버의 skin이라는 인터넷상의 자원의 위치를 의미하기에 URL 이면서 URI

- https://example.com/one/two/abc.html 의 경우

  > example 서버의 one/two 디렉토리 아래의 abc.html을 가리키므로 URL이면서 URI

- https://example.com/123 의 경우 

  > 조금 다르다!  여기서 URL은 https://example.com까지이고, 내가 원하는 정보에 도달하기위해 123이라는 식별자가 필요하다.
  > 즉, URI 이지만 URL은 아닌 것이다.

- https://example.com/one?id=123 의 경우

  > 이것도 마찬가지! URL은 https://example.com/one 까지이고 내가 원하는 정보에 도달하기 위해서는 ?id=123이라는 식별자가 필요한 것이다.
  > 이것 또한 URI이지만 URL은 아닌것.



## REST 구성 요소



1. **자원(Resource), URI**

   * **모든 자원은 고유한 ID를 가지고** ID는 서버에 존재하고 클라이언트는 각 자원의 상태를 조작하기 위해 요청을 보낸다. 
   * HTTP에서 이러한 자원을 구별하는 ID는 "Students/1" 같은 HTTP URI 이다.

   

2. **행위(Verb), Method**

   * 클라이언트는 **URI를 이용해 자원을 지정**하고 **자원을 조작하기 위해 Method를 사용**한다.
   *  HTTP 프로토콜에서는 **GET, POST, PUT, DELETE같은 Method를 제공**한다.

   

3. **표현(Representation)**
   * 클라이언트가 서버로 요청을 보냈을 때 **서버가 응답으로 보내주는 자원의 상태를 Representation**이라고 한다. 
   * REST에서 하나의 자원은 **JSON, XML, TEXT, RSS 등 여러형태의 Representation**으로 나타낼 수 있다.
   * 현재는 JSON으로 주고 받는 것이 대부분이다.



## REST의 특징

1. **클라이언트 / 서버 구조(Client-Server)**
   * **자원이 있는 Server, 자원을 요청하는 Client의 구조**를 가진다.

     
   
2. **무상태(Stateless)**

   * HTTP는 Stateless 프로토콜 이므로 REST 역시 **무상태성**을 가진다. **즉 서버에서 어떤 작업을 하기위해 상태정보를 기억할 필요가 없고 들어온 요청에 대해 처리만 해주면 되기 때문에 구현이 쉽고 단순해진다.**

   

3. **캐시 처리 가능(Cachealble)**
   * 웹 표준 HTTP 프로토콜을 그대로 사용하므로, **웹에서 사용하는 기존의 인프라를 그대로 활용** 가능하다.
   * 클라이언트는 응답을 캐싱할 수 있어야 한다. 이를 통해 클라이언트가 같은 자료를 중복 요청하는것을 막아 서버의 부담을 줄여줄 수 있다.

   
   
4. **계층화**
   
   * **API 서버는 순수 비즈니스 로직을 수행**하고 그 앞단에 **사용자 인증, 암호화, 로드밸런싱 등을 하는 계층을 추가**하여 **구조상의 유연성**을 줄 수 있다.



5. **인터페이스 일관성(Uniform Interface)**
   * **URI로 지정한 자원에 대한 조작을 통일**되고 **한정적인 인터페이스로 수행**한다. **HTTP 표준에만 따른다면** **모든 플랫폼에 사용이 가능**하다.



6. **자체 표현 구조**
   * **동사(Method) + 명사(URI)로 이루어져있으며** 어떤 메소드에 **무슨 행위를 하는지 알 수 있으며** REST API 자체가 매우 쉬워서 **API 메세지 자체만 보고도 API를 이해**할 수 있다.



## REST의 장단점

#### **장점**

1. **쉬운 사용**
   HTTP 프로토콜 인프라를 그대로 사용하므로 **별도의 인프라를 구축할 필요가 없다**.
2. **클라이언트-서버 역할의 명확한 분리**
   클라이언트는 REST API를 통해 서버와 정보를 주고받는다. **REST의 특징인 Stateless에 따라 서버는 클라이언트의 Context를 유지할 필요가 없다.**
3. **특정 데이터 표현을 사용가능**
   REST API는 **헤더 부분에** **URI 처리 메소드를 명시**하고 필요한 **실제 데이터를 'Body'에 표현할 수 있도록 분리**시켰다. **JSON, XML 등 원하는 Representation 언어로 사용 가능**하다.

#### **단점**

1. **메소드의 한계**
   REST는 HTTP 메소드를 이용하여 URI를 표현한다. 이러한 표현은 쉬운 사용이 가능하다는 장점이 있지만 반대로 **메소드 형태가 제한적인 단점**이 있다.
2. **표준이 없음**
   REST는 **설계 가이드 일 뿐이지 표준이 아니다.** 명확한 표준이 없다.



## "그래서 REST를 쓰는 이유가 뭔데?!

###  => **‘애플리케이션 분리 및 통합’  /  ‘다양한 클라이언트의 등장’**

- 최근의 서비스 / 애플리케이션의 개발 흐름은 멀티 플랫폼, 멀티 디바이스 시대로 넘어와 있습니다. 단순히 하나의 브라우저만 지원하면 되었던 이전과는 달리, **최근의 서버 프로그램은 여러 웹 브라우저는 물론이며, 아이폰, 안드로이드 애플리케이션과의 통신에 대응할 수 있어야 합니다.**
- 따라서 플랫폼에 맞추어 새로운 서버를 만드는 수고를 들이지 않기 위해 **범용적으로 사용성을 보장하는 서버 디자인이 필요하게 되었습니다.**



---

----



#  **REST API란?!** 

##  **API(Application Programming Interface)란** 

- API란 클라이언트가 리소스를 요청할 수 있도록 서버측에서 제공된 인터페이스(interface)를 말한다.
- 이러한 API로 데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉진하며, 서로 정보를 교환가능 하도록 한다.
- 서버를 개발한다는 것은 API에 대한 마스터 권한이 생기는 것이다.
- API와 함께 항상 메뉴얼도 제공되어야 한다. URI를 모르면 클라이언트(client)는 사용할 수 없다.



##  **REST API의 정의** 

- **REST 기반으로 서비스 API를 구현하는 것**
- 최근 OpenAPI(누구나 사용할 수 있도록 공개된 API : 구글 맵, 공공 데이터 등), 마이크로 서비스(하나의 큰 애플리케이션을 여러 개의 작은 애플리케이션으로 쪼개어 변경과 조합이 가능하도록 만든 아키텍처) 등을 제공하는 업체 대부분은 REST API를 제공한다.

 

##  **REST API의 특징** 

- 사내 시스템들도 REST 기반으로 시스템을 분산해 확장성과 재사용성을 높여 유지보수 및 운용을 편리하게 할 수 있다.
- REST는 HTTP 표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있다.
- 즉, REST API를 제작하면 델파이 클라이언트 뿐 아니라, 자바, C#, 웹 등을 이용해 클라이언트를 제작할 수 있다.



##  REST API 디자인 가이드

REST API 설계 시 가장 중요한 항목은 다음의 2가지로 요약할 수 있음.

### **첫 번째,** URI는 정보의 자원을 표현해야 한다.

### **두 번째,** 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.



##  **REST API 설계 기본 규칙** 

###  **참고 리소스 원형** 

- 도큐먼트 : 객체 인스턴스나 데이터베이스 레코드와 유사한 개념
- 컬렉션 : 서버에서 관리하는 디렉터리라는 리소스
- 스토어 : 클라이언트에서 관리하는 리소스 저장소 (?)

####  **1. URL는 정보를 자원을 표현해야 한다.** 

1. resource는 동사보다는 명사를, 대문자 보다는 소문자를 사용한다.
2. resource의 도큐먼트 이름으로는 단수 명사를 사용해야한다.
3. resource의 컬렉션 이름으로는 복수 명사를 사용해야한다.
4. resource의 스토어 이름으로는 복수 명사를 사용해야한다.
   - Ex) `GET /Member/1 `→ `GET /members/1`

 **2. 자원에 대한 행위는 HTTP Method(GET, PUT, POST, DELETE 등)로 표현한다.** 

1. URL에 HTTP Method가 들어가면 안된다.
   - Ex) `GET /members/delete/1` → `DELETE /members/1`
2. URL에 행위에 대한 동사 표현이 들어가면 안된다. (즉, CRUD 기능을 나타내는 것은 URL에 사용하지 않는다.)
   - Ex) `GET /members/show/1` → `GET /members/1`
   - Ex) `GET /members/insert/2` → `POST /members/2`
3. 경로 부분 중 변화하는 부분은 유일한 값으로 대체한다. (즉, :id는 하나의 특정 resource를 나타내는 고유값이다.)
   - Ex) student를 생성하는 route : `POST /students`
   - Ex) id=12인 student를 삭제하는 route : `DELETE /students/12`

 cf) 

### 자원을 표현하는 Colllection과 Document

------

* DOCUMENT는 단순히 문서로 이해해도 되고, 한 객체
* Colllection은 문서들의 집합, 객체들의 집합
* Colllection과 Document는 모두 리소스라고 표현할 수 있으며 URI에 표현됨.

```
    http:// restapi.example.com/sports/soccer
```

=> sports라는 컬렉션과 soccer라는 도큐먼트로 표현되고 있음.

```
    http:// restapi.example.com/sports/soccer/players/13
```

* sports, players 컬렉션과 soccer, 13(13번인 선수)를 의미하는 도큐먼트로 URI가 이루어짐

* 여기서 중요한 점은 **컬렉션은 복수**로 사용하고 있다는 점 => 좀 더 직관적인 REST API를 위해서는 컬렉션과 도큐먼트를 사용할 때 단수 복수도 지켜준다면 좀 더 이해하기 쉬운 URI를 설계할 수 있음.





##  **REST API 설계 규칙** 

#### **1. 슬래시 구분자(/ )는 계층 관계를 나타내는데 사용한다.** 

- Ex) `http://restapi.example.com/houses/apertments`

#### **2. URL 마지막 문자로 슬래시(/ )를 포함하지 않는다.** 

- URL에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며,
  URL가 다르다는 것은 리소스가 다르다는 것이고,역으로 리소스가 다르면 URL도 달라져야한다.
- REST API는 분명한 URL를 만들어 통신을 해야 하기 때문에 혼동을 주지 않도록 URL 경로의 마지막에는 슬래시(/ )를 사용하지 않는다.

#### **3. 하이픈(- )은 URL 가독성을 높이는데 사용한다.** 

- 불가피하게 긴 URL경로를 사용하게 된다면 하이픈을 사용해 가독성을 높여준다.

#### **4. 밑줄(_ )은 URL에 사용하지 않는다.** 

- 밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 하므로 가독성을 위해 밑줄은 사용하지 않는다.

#### **5. URL 경로에는 소문자가 적합하다.** 

- URL 경로에 대문자 사용은 피하도록 한다.
- PFC 3986(URL 문법 형식)은 URL 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문이다.

#### **6. 파일확장자는 URL에 포함하지 않는다.** 

- REST API에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URL안에 포함시키지 않는다.
- Accep header를 사용한다.
- Ex) `http://restapi.example.com/members/soccer/345/photo.jpg` **(X)**
- Ex) **`GET /members/soccer/345/photo HTTP/1.1` `Host: restapi.example.com` `Accept: image/jpg` (O)**

#### **7. 리소스 간에는 연관 관계가 있는경우** 

- /리소스명/리소스 ID/관계가 있는 다른 리소스명

- Ex) `GET : /users/{userid}/devices (일반적으로 소유 'has'의 관계를 표현할 때)`

  

-----

----

#  **RESTful이란?** 

- RESTful은 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
  - **‘REST API’를 제공하는 웹 서비스를 ‘RESTful’하다고 할 수 있다.**
- RESTful은 REST를 REST답게 쓰기 위한 방법으로, 누군가가 공식적으로 발표한 것이 아니다.
  - **즉, REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭된다.**

 

##  **RESTful의 목적** 

- 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것
- RESTful한 API를 구현하는 근본적인 목적이 성능 향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기이니, 성능이 중요한 상화에서는 굳이 RESTful한 API를 구현할 필요는 없다.

 

##  **RESTful 하지 못한 경우** 

- Ex1) **CRUD** 기능을 모두 POST로만 처리하는 API
- Ex2) route에 resource, id 외의 정보가 들어가는 경우(/students/updateName)



## RESTful API를 위한 HTTP Methods

## 1. GET

**자원을 받아오기만 할때 사용한다.**

* 어떠한 방식으로도 자원의 상태를 변경시키지 않으므로, safe method라고 불리기도 한다.
* GET API는 멱등성의 띈다. 

>  멱등성이란, 동일한 API를 여러번 호출시에도 동일한 결과를 얻을 수 있음을 의미한다.(POST, PUT을 통해 데이터가 변경되지 않는다면)

## 2. POST

**새로운 자원을 추가할 때 사용한다.**

* 이는 서버의 상태를 변경시키며, 때문에 비멱등성 성질을 가진다.
*  응답 코드로 `201(Created)`를 받아야 정상적으로 서버에 추가 되었음을 확인할 수 있다.

## 3. PUT

**존재하는 자원을 변경 할 때 사용한다.**

* 만약 자원이 존재하지 않는 경우, API는 새로운 자원을 생성하도록 할 수 있다. 이 경우 응답코드는 `201(Created)`를 받게 된다.
* ### 존재하는 자원을 변경시킨 경우 `200(OK)` 또는 `204(No Content)` 응답코드를 받게 된다. => 공부

#### POST VS PUT

* POST는 여러개의 자원에 수행되는 반면, PUT은 단일 자원에만 수행된다.

## 4. DELETE

**자원을 삭제할 때 사용한다.**

* DELETE 메소드는 멱등성 성질을 띈다.
*  DELETE 메소드를 요청하면 자원을 삭제하게 되는데, 반복적으로 DELETE를 호출한 경우 결과는 바뀌지 않는다. 하지만 이미 제거되었으므로`404(NOT FOUND)`를 반환받는다.

## 5. PATCH

**한 자원의 데이터를 부분적으로 변경할 때 사용한다.**

* PUT도 마찬가지로 자원을 변경할 수 있다. 하지만 조금더 명확하게는 **존재하는 자원에 대해 부분적으로 업데이트를 위해서는 PATCH를 사용한다.** PUT은 자원을 완전히 대체하는 경우 사용한다.

* PATCH의 경우 모든 브라우저, 서버, 앱 어플리케이션 프레임워크에서 사용할 수 있는 것은 아니다.





----



# 참고

1. https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html
2. https://yuricoding.tistory.com/79
3. https://velog.io/@somday/RESTful-API-%EC%9D%B4%EB%9E%80
4. https://velog.io/@ellyheetov/REST-API
5. https://www.joinc.co.kr/w/man/12/rest/about
6. https://meetup.toast.com/posts/92
7. https://www.joinc.co.kr/w/man/12/rest/about
8. https://medium.com/@js230023/url-%EA%B3%BC-uri%EC%9D%98-%EC%B0%A8%EC%9D%B4-154d70814d2a


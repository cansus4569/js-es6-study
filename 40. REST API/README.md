# REST API
**RE**presentational **S**tate **T**ransfer
* REST의 기본원칙을 성실히 지킨 서비스 디자인을 "RESTful" 이라고 표현한다.
* **REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처**
* **REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다.**

## REST API의 구성
* REST API는 자원(resource), 행위(verb), 표현(representations)의 3가지 요소로 구성된다.
* REST는 자체 표현 구조(self-de-scriptiveness)로 구성되어 REST API만으로 HTTP 요청의 내용을 이해할 수 있다.

구성요소|내용|표현 방법
-|-|-
자원(resource)|자원|URI(엔드포인트)
행위(verb)|자원에 대한 행위|HTTP 요청 메서드
표현(representations)|자원에 대한 행위의 구체적 내용|페이로드

## REST API 설계 원칙
* REST에서 가장 중요한 기본적인 원칙은 두 가지다.
* **URI는 리소스를 표현**하는데 집중하고 **행위에 대한 정의는 HTTP 요청 메서드**를 통해 하는 것이 RESTful API를 설계하는 중심 규칙이다.
### 1. URI는 리소스를 표현해야 한다.
* **URI는 리소스를 표현**하는데 중점을 두어야 한다.
* 리소스를 **식별할 수 있는 이름**은 동사보다는 **명사를 사용**한다.
    * 따라서 이름에 get 같은 행위에 대한 표현이 들어가서는 안된다.
```nginx
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```
### 2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.
* HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법이다.
* 주로 5가지 요청 메서드(GET, POST, PUT, PATCH, DELETE 등)를 사용하여 CRUD를 구현한다.

HTTP 요청 메서드|종류|목적|페이로드
-|-|-|:-:
GET|index/retrieve|모든/특정 리소스 취득|X
POST|create|리소스 생성|O
PUT|replace|리소스의 전체 교체|O
PATCH|modify|리소스의 일부 수정|O
DELETE|delete|모든/특정 리소스 삭제|X

* 리소스에 대한 행위는 HTTP 요청 메서드를 통해 표현하며 URI에 표현하지 않는다.
* 예시
    * 리소스를 취득하는 경우에는 GET 사용
    * 리소스를 삭제하는 경우에는 DELETE 사용

```nginx
# bad
GET /todos/delete/1

# good
DELETE /todos/1
```
## JSON Server를 이용한 REST API 실습
* HTTP 요청을 전송하고 응답을 받으려면 서버가 필요하다.
* JSON Server를 사용해 가상 REST API 서버를 구축하여 HTTP 요청을 전송하고 응답을 받는 실습을 진행
### 1. JSON Server 설치
* JSON Server는 json 파일을 사용하여 가상 REST API 서버를 구축할 수 있는 툴이다.
* 사용법은 매우 간단하다.
    * npm을 사용하여 JSON Server를 설치

```shell
$ mkdir json-server-exam && cd json-server-exam
$ npm init -y
$ npm install json-server --save-dev
```
```
npm(node package manager)은 자바스크립트 패키지 매니저다.
Node.js에서 사용할 수 있는 모듈들을 패키지화하여 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 CLI(Command Line Interface)를 제공한다.
자신이 작성한 패키지를 공개할 수도 있고 필요한 패키지를 검색하여 재사용할 수도 있다.
```
### 2. db.json 파일 생성
* 프로젝트 루트 폴더(/json-server-exam)에 다음과 같이 db.json 파일을 생성한다.
* [db.json](./json-server-exam/db.json) 파일은 리소스를 제공하는 데이터베이스 역할을 한다.
```json
{
    "todos": [
        {
            "id": 1,
            "content": "HTML",
            "completed": true
        },
        {
            "id": 2,
            "content": "CSS",
            "completed": false
        },
        {
            "id": 3,
            "content": "JavaScript",
            "completed": true
        }
    ]
}
```
### 3. JSON Server 실행
* 터미널에서 다음과 같이 명령어를 입력하여 JSON Server를 실행한다.
* JSON Server가 데이터베이스 역할을 하는 db.json 파일의 변경을 감지하게 하려면 watch 옵션을 추가한다.
* 기본 포트는 3000이다. 포트를 변경하려면 port 옵션을 추가한다.
```shell
## 기본 포트(3000) 사용 / watch 옵션 적용
$ json-server --watch db.json
## 포트 변경 / watch 옵션 적용
$ json-server --watch db.json --port 5000
```
* 위와 같이 매번 명령어를 입력하는 것이 번거로우니 package.json 파일의 scripts를 다음과 같이 수정하여 JSON Server를 실행하여 보자.
    * package.json 파일에서 불필요한 항목은 삭제함.

```json
{
  "name": "json-server-exam",
  "version": "1.0.0",
  "scripts": {
    "start": "json-server --watch db.json"
  },
  "devDependencies": {
    "json-server": "^0.17.0"
  }
}
```
* 터미널에서 `npm start` 명령어를 입력하여 JSON Server를 실행한다.

```shell
$ npm start

> json-server-exam@1.0.0 start
> json-server --watch db.json

  \{^_^}/ hi!

  Loading db.json
  Done

  Resources
  http://localhost:3000/todos

  Home
  http://localhost:3000

  Type s + enter at any time to create a snapshot of the database
  Watching...
```
### 4. GET 요청
* todos 리소스에서 모든 todo를 취득(index)한다.
* JSON Server의 루트 폴더(/json-server-exam)에 public 폴더를 생성하고 JSON Server를 중단한 후 재실행한다.
* public 폴더에 다음 [get_index.html](./json-server-exam/public/get_index.html)을 추가하고 브라우저에서 http://localhost:3000/get_index.html 로 접속한다.

![GET 요청(index)](https://user-images.githubusercontent.com/63139527/187630362-a2fc69f0-e086-4781-bd4f-103c3949890d.png "GET 요청(index)")

---
* todos 리소스에서 id를 사용하여 특정 todo를 취득(retrieve)한다.
* public 폴더에 다음 [get_retrieve.html](./json-server-exam/public/get_retrieve.html)을 추가하고 브라우저에서 http://localhost:3000/get_retrieve.html 로 접속한다.

![GET 요청(retrieve)](https://user-images.githubusercontent.com/63139527/187632143-b58cd168-07b8-4f04-9067-6a100a2ab222.png "GET 요청(retrieve)")

### 5. POST 요청
* todos 리소스에 새로운 todo를 생성한다.
* POST 요청 시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.
* public 폴더에 다음 [post.html](./json-server-exam/public/post.html)을 추가하고 브라우저에서 http://localhost:3000/post.html 로 접속한다.

![POST 요청](https://user-images.githubusercontent.com/63139527/187638369-84f78b7b-32a7-4699-9d1a-60934e02cc16.png "POST 요청")

### 6. PUT 요청
* PUT은 특정 리소스 전체를 교체할 때 사용한다.
* 다음 예제에서는 todos 리소스에서 id로 todo를 특정하여 id를 제외한 리소스 전체를 교체한다.
* PUT 요청 시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.
* public 폴더에 다음 [put.html](./json-server-exam/public/put.html)을 추가하고 브라우저에서 http://localhost:3000/put.html 로 접속한다.

![PUT 요청](https://user-images.githubusercontent.com/63139527/187639983-ea280627-0569-4624-9e1f-f9cd74ebec51.png "PUT 요청")

### 7. PATCH 요청
* PATCH는 특정 리소스의 일부를 수정할 때 사용한다.
* 다음 예제에서는 todos 리소스의 id로 todo를 특정하여 completed만 수정한다.
* PATCH 요청 시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.
* public 폴더에 다음 [patch.html](./json-server-exam/public/patch.html)을 추가하고 브라우저에서 http://localhost:3000/patch.html 로 접속한다.

![PATCH 요청](https://user-images.githubusercontent.com/63139527/187641234-f9a8f93a-a144-4442-8594-63b508ea9c26.png "PATCH 요청")

### 8. DELETE 요청
* todos 리소스에서 id를 사용하여 todo를 삭제한다.
* public 폴더에 다음 [delete.html](./json-server-exam/public/delete.html)을 추가하고 브라우저에서 http://localhost:3000/delete.html 로 접속한다.

![DELETE 요청](https://user-images.githubusercontent.com/63139527/187642512-0d74faa0-b649-4151-9871-39583b5e64f6.png "DELETE 요청")
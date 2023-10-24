# Database

### 목차

* Database란
* DBMS(Database Management System)
* RDBMS(Relational Database Management System)
* 데이터베이스 언어
  * DDL
  * DML
  * DCL
* SQL
* 데이터 모델
  * &#x20;관계형 데이터 모델
* 튜플

### 강의 정리

\| Database

* organized collection of data stored and accessed 구조화된 정보 or 데이터의 조직화된 모음

&#x20;\| DBMS(Database Managed System)

* 데이터베이스에서 데이터를 쉽게 찾을 수 있어야 하고, 효율적으로 조작(추가, 수정, 삭제)할 수 있어야 한다. 또한 접근 권한, 보안 문제를 다룰 수 있어야 하여 등장하였다.
*   DBMS는 데이터베이스 언어를 제공한다:

    1. DDL (Data Definition Language) → Schema
    2. DML (Data Manipulation Language) → Query & Command
    3. DCL (Data Control Language) → Grant, Revoke, Commit, Rollback

    대부분의 RDBMS에서 이들은 모두 SQL(Structured Query Language)로 표현된다.

\| DataModel



<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

데이터 모델은 크게 3가지로 구분한다:

1. Conceptual Data Model

개념 데이터 모델링은 모델링의 시작으로 고객과의 인터뷰를 통해 요구사항 수집 및 분석하여 데이터 모델의 전체적인 모양을 결정 짓는다.&#x20;

Entity와 RelationShip 위주로 모델링을 이룬다.

2. Logical Data Model

논리 데이터 모델은 개념 모델을 기반으로 논리적인 구성 요소를 빠짐없이 정의한다. 비즈니스에 필요한 모든 내용을 정의해야하며 정규화를 통해 중복된 내용을 없앤다.

3. Physical Data Model

논리 데이터 모델을 통해 비즈니스 요구사항을 반영했다면 구현을 위해 기술적인 부분을 고려한다. 엔티티를 테이블로 변환하고 Partition, Index , Organized Table 등의 형태로 디자인하고 각 DBMS 에 적합하게 물리화 한다. 데이터 모델의 변경 없이 해결할 수 없는 성능 문제나 쿼리의 작성이 불가능할 정도로 복잡하다면 논리 모델을 기준으로 반정규화를 수행한다.

\| DB 구성요소

### 튜플(Tuple)

튜플은 릴레이션을 구성하는 각각의 행을 말하며 속성의 모임으로 구성된다. 파일 구조에서 레코드와 같은 의미이다. 튜플의 수를 카디널리티(Cardinality) 또는 기수, 대응수라고 한다.

### 속성(Attribute)

속성은 데이터베이스를 구성하는 가장 작은 논리적 단위이며 개체의 특성을 기술한다. 파일 구조상의 데이터 항목 또는 데이터 필드에 해당한다. 속성의 수를 디그리(Degree) 또는 차수라고 한다.

### 궁금한 점?

1. 정규화란?&#x20;

정규화(Normalization)의 기본 목표는 테이블 간에 중복된 데이타를 허용하지 않는다는 것이다. 중복된 데이터를 허용하지 않음으로써 무결성(Integrity)를 유지할 수 있으며, DB의 저장 용량 역시 줄일 수 있다.

#### \[ 제1 정규화 ]

제1 정규화란 테이블의 컬럼이 원자값(Atomic Value, 하나의 값)을 갖도록 테이블을 분해하는 것이다. 예를 들어 아래와 같은 고객 취미 테이블이 존재한다고 하자.&#x20;

<figure><img src="https://blog.kakaocdn.net/dn/bNbQUm/btqT18yag04/pTXJX3wB23ouk8az7EgWQ1/img.png" alt=""><figcaption></figcaption></figure>

위의 테이블에서 추신수와 박세리는 여러 개의 취미를 가지고 있기 때문에 제1 정규형을 만족하지 못하고 있다. 그렇기 때문에 이를 제1 정규화하여 분해할 수 있다. 제1 정규화를 진행한 테이블은 아래와 같다.

<figure><img src="https://blog.kakaocdn.net/dn/bMlNZj/btqT17FWVot/jUKTAUyOdrH83pRraKw3K0/img.png" alt=""><figcaption></figcaption></figure>

&#x20;

#### \[ 제2 정규화 ]

제2 정규화란 제1 정규화를 진행한 테이블에 대해 완전 함수 종속을 만족하도록 테이블을 분해하는 것이다. 여기서 완전 함수 종속이라는 것은 기본키의 부분집합이 결정자가 되어선 안된다는 것을 의미한다.\
예를 들어 아래와 같은 수강 강좌 테이블을 살펴보자.&#x20;

&#x20;

<figure><img src="https://blog.kakaocdn.net/dn/ylbaZ/btqT8Jc4K3s/0VFTPoKKFkbxZghKWDwKo1/img.png" alt=""><figcaption></figcaption></figure>

이 테이블에서 기본키는 (학생번호, 강좌이름)으로 복합키이다. 그리고 (학생번호, 강좌이름)인 기본키는 성적을 결정하고 있다. (학생번호, 강좌이름) --> (성적)\
그런데 여기서 강의실이라는 컬럼은 기본키의 부분집합인 강좌이름에 의해 결정될 수 있다. (강좌이름) --> (강의실)\
즉, 기본키(학생번호, 강좌이름)의 부분키인 강좌이름이 결정자이기 때문에 위의 테이블의 경우 다음과 같이 기존의 테이블에서 강의실을 분해하여 별도의 테이블로 관리하여 제2 정규형을 만족시킬 수 있다.

<figure><img src="https://blog.kakaocdn.net/dn/bluCnc/btqT7VEOf04/Me8DfY7rtycgJPYlYQKEWK/img.png" alt=""><figcaption></figcaption></figure>

&#x20;

&#x20;

#### \[ 제3 정규화 ]

제3 정규화란 제2 정규화를 진행한 테이블에 대해 이행적 종속을 없애도록 테이블을 분해하는 것이다. 여기서 이행적 종속이라는 것은 A -> B, B -> C가 성립할 때 A -> C가 성립되는 것을 의미한다.\
예를 들어 아래와 같은 계절 학기 테이블을 살펴보자.&#x20;

<figure><img src="https://blog.kakaocdn.net/dn/enwN1N/btqUeiMyErd/sP8NKCe70NKsZncGuhO9uK/img.png" alt=""><figcaption></figcaption></figure>

&#x20;

&#x20;

기존의 테이블에서 학생 번호는 강좌 이름을 결정하고 있고, 강좌 이름은 수강료를 결정하고 있다. 그렇기 때문에 이를 (학생 번호, 강좌 이름) 테이블과 (강좌 이름, 수강료) 테이블로 분해해야 한다.&#x20;

이행적 종속을 제거하는 이유는 비교적 간단하다. 예를 들어 501번 학생이 수강하는 강좌가 스포츠경영학으로 변경되었다고 하자. 이행적 종속이 존재한다면 501번의 학생은 스포츠경영학이라는 수업을 20000원이라는 수강료로 듣게 된다. 물론 강좌 이름에 맞게 수강료를 다시 변경할 수 있지만, 이러한 번거로움을 해결하기 위해 제3 정규화를 하는 것이다.\
즉, 학생 번호를 통해 강좌 이름을 참조하고, 강좌 이름으로 수강료를 참조하도록 테이블을 분해해야 하며 그 결과는 다음의 그림과 같다.

<figure><img src="https://blog.kakaocdn.net/dn/ci1le3/btqUeXnPnpD/yKkURqr8cZl21f5erx42QK/img.png" alt=""><figcaption></figcaption></figure>

&#x20;

&#x20;

#### \[ BCNF 정규화 ]

BCNF 정규화란 제3 정규화를 진행한 테이블에 대해 모든 결정자가 후보키가 되도록 테이블을 분해하는 것이다. 예를 들어 다음과 같은 특강수강 테이블이 존재한다고 하자.

&#x20;

<figure><img src="https://blog.kakaocdn.net/dn/bBN6xu/btqT6IlqRF4/MvBoxYMxtgS1JT7t1AymnK/img.png" alt=""><figcaption></figcaption></figure>

&#x20;

특강수강 테이블에서 기본키는 (학생번호, 특강이름)이다. 그리고 기본키 (학생번호, 특강이름)는 교수를 결정하고 있다. 또한 여기서 교수는 특강이름을 결정하고 있다.\
그런데 문제는 교수가 특강이름을 결정하는 결정자이지만, 후보키가 아니라는 점이다. 그렇기 때문에 BCNF 정규화를 만족시키기 위해서 위의 테이블을 분해해야 하는데, 다음과 같이 특강신청 테이블과 특강교수 테이블로 분해할 수 있다.

&#x20;

<figure><img src="https://blog.kakaocdn.net/dn/3cbHr/btq3mNylPan/c6b2lBuH4OkdDNmrzGHWUk/img.png" alt=""><figcaption></figcaption></figure>

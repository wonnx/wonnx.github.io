---
title: '[database] 데이터베이스 정규화 (Nomalization)'
categories:
    - database
tag:
    - database
    - nomalization
---


## 데이터베이스 정규화  

**데이터베이스 정규화(Nomalization)**란 데이터의 중복을 줄이고 무결성을 향상 시키는 등 여러 목적을 달성하기 위해 데이터베이스를 재구성 하는 것을 뜻한다.

#### 정규화의 목적
- 불필요한 데이터를 제거하고 데이터의 중복을 최소화 하기 위해
- 데이터베이스 구조 확장 시 재디자인을 최소화하기 위해
- 테이블 구성을 논리적이고 직관적으로 만들기 위해
- 무결성 제약 조건을 지키고, 이상 현상(Anomaly)을 방지하기 위해

#### <span style="color: blue">**제 1 정규화(First Normal Form, 1NF)**</span>
> 제 1 정규형을 만족하기 위해서는 릴레이션에 속한 **<u>모든 도메인이 원자값</u>**만으로 되어 있어야 한다.

<center><img src="/assets/images/posts/2022-11-28-Database Nomalization/1NF.png" style="width:90%; height:90%"></center><br>
위의 Customer 릴레이션에서 Telephone Number 도메인이 전화번호를 여러 개 가지고 있어 모든 도메인이 원자값만으로 이루어져 있어야 한다는 조건을 만족하지 않았음을 알 수 있다. 따라서 위의 릴레이션을 제 1 정규형을 만족시키는 형태로 변환하기 위해 아래와 같이 정규화 할 수 있다.

<center><img src="/assets/images/posts/2022-11-28-Database Nomalization/1NF2.png" style="width:90%; height:90%"></center>

#### <span style="color: blue;">**제 2 정규화(Second Normal Form, 2NF)**</span>
> 제 2 정규형을 만족하기 위해서는 테이블의 모든 컬럼이 **<u>완전 함수적 종속</u>**을 만족해야 한다.

- X 값에 따라 Y 값이 결정되는 경우 X -> Y로 표현되는데, 이를 Y는 X에 대해 **함수적 종속**이라 한다. 이때 X를 **결정자**라 하고, Y는 **종속자**라 한다.
- 함수적 종속에서 X 값이 여러 요소일 경우, 즉 {X1, X2} -> Y일 경우, X1와 X2가 Y의 값을 결정한다면 이를 **완전 함수적 종속**이라 부르고, X1, X2 중 하나만 Y의 값을 결정한다면 이를 **부분 함수적 종속**이라고 한다.

<center><img src="/assets/images/posts/2022-11-28-Database Nomalization/2NF.png" style="width:90%; height:90%"></center><br>
위의 그림에서 Manufacturer과 Model 값에 따라 Model Full Name 값이 결정되기 때문에, {Manufacturer, Model} -> Model Full Name 이므로 완전 함수적 종속이라고 할 수 있다. 하지만 {Manufacturer, Model} -> Manufacturer Country 관계에서 Model은 Manufacturer Country 값을 결정하는데 아무런 영향을 미치지 않으므로 부분 함수 종속이다.  

따라서 부분 함수 종속을 제거한, 제 2 정규형을 만족하는 테이블은 다음과 같다.
<center><img src="/assets/images/posts/2022-11-28-Database Nomalization/2NF2.png" style="width:75%; height:75%"></center>

#### <span style="color: blue;">**제 3 정규화(Third Normal Form, 3NF)**</span>
제 2 정규화가 진행된 테이블에서 **<u>이행적 종속</u>**을 없애기 위해 테이블을 분리하는 단계
- 이행적 종속: A -> B, B -> C면 A -> C가 성립한다.

> 제 3 정규형을 만족하기 위해서는 릴레이션이 제 2 정규형을 만족해야 하며, **기본 키(primary key)가 아닌 속성(attribute)들은 기본 키에만 의존해야 한다.**

<center><img src="/assets/images/posts/2022-11-28-Database Nomalization/3NF.png" style="width:90%; height:90%"></center><br>
위의 그림에서 {Tournament, Year}가 후보키(Candidate Key)가 된다. 하지만 Winner Date of Birth는 Winner의 생년월일을 나타내므로 기본키가 아닌 속성인 Winner에 의존하고 있는 것을 알 수 있는데, 이는 3NF를 위반한 것이다. 이를 해결하기 위해서 테이블을 다음과 같이 분리할 수 있다.
<center><img src="/assets/images/posts/2022-11-28-Database Nomalization/3NF2.png" style="width:90%; height:90%"></center>

#### <span style="color: blue;">**BCNF (Boyce and Codd Normal Form)**</span>
BCNF는 제 3 정규형을 조금 더 강화시킨 개념이다. 결정자(Determinant)가 후보키로 취급되지 않는 경우 이상현상(Anomaly)이 발생할 수 있는데, 이러한 이상현상을 해결하기 위해서 BCNF에서는 **모든 결정자가 항상 후보키가 되도록 릴레이션을 분해한다.**
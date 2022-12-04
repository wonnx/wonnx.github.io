---
title: '[database] 데이터베이스 이상 현상 (Anomaly)'
categories:
    - database
tag:
    - database
    - nomalization
---


## 이상 현상 (Anomaly)
좋은 관계형 데이터베이스를 설계하는 목적 중 하나가 정보의 이상 현상이 생기지 않도록 고려해 설계하는 것이다. 이상 현상은 **갱신 이상(Modification Anomaly), 삽입 이상(Insertion Anomaly), 삭제 이상(Deletion Anomaly)**으로 구성된다.

1. 갱신 이상(Modificatioin Anomaly): 반복된 데이터 중 일부를 갱신할 시 데이터의 불일치가 발생하는 경우
2. 삽입 이상(Insertion Anomaly): 불필요한 정보를 함께 저장하지 않고서는 중요한 정보를 저장하는 것이 불가능한 경우
3. 삭제 이상(Deletion Anomaly): 필요한 정보를 함께 삭제하지 않고서는 어떤 정보를 삭제하는 것이 불가능한 경우

이러한 이상 현상은 데이터베이스 정규화를 통해 해결할 수 있다.  

#### 이상 현상 예시 
<center><img src="/assets/images/posts/2022-11-29-Database Anomaly/table.png"></center>

1. 갱신 이상: 만약 네 번째 행의 A. Bruchs의 Department가 CIS에서 Marketing으로 바뀌었다고 가정해보자. 테이블의 4, 5번째 행의 CIS를 둘 다 바꾸지 않고 하나만 바꾼다면, A. Brunchs의 부서는 CIS와 Marketing 두 개가 되어 어느 부서에 속해있는지 알 수 없다.
2. 삽입 이상: 새로운 부서 Engineering이 신설되었고 아직 Employee는 없다고 가정해보자. 하지만 이 부서에 대한 정보는 불필요한 정보(Employee_ID, Name)를 함께 입력하지 않는 한 테이블에 입력할 수 없다.
3. 삭제 이상: 만약 Accounting 부서에 속한 사람이 첫 번째 행 J. Longfellow 단 한 명이라고 가정해보자. J. Longfellow의 정보를 삭제하면 Accounting 부서에 대한 정보도 사라지게 된다.
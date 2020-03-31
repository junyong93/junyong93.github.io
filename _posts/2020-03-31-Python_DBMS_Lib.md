---
title: "Python DBMS 라이브러리 체험기"
categories: 
  - Miscellaneous
last_modified_at: 2020-03-31 12:00:00
toc: true #Table of Contents
comments: true
use_math: true # MathJax On
---

Python은 여타 프로그래밍 언어와 같이 왠만한 데이터를 읽어올수 있도록 설계되어있다.

예를 들면 직접 파일 입출력을 내장함수인 `open()` 과 `readline()` 등을 이용하여 불러올 수 있다.

특히 정형데이터는 [Pandas 라이브러리](https://pandas.pydata.org/)를 이용해서 `read_csv()`와 같은 함수가 자주 활용된다. 

하지만 이러한 방식들은 파일을 직접 불러와서 분석을 진행하는 경우이다.

이미 데이터를 활용하는 환경, 즉 DB가 구축된 곳에서 하나하나 파일을 저장한 후, 직접 읽어오는 방식은 비효율적이다.

이런 점에서 DB로부터 데이터를 끌어다 쓸 때는, 파이썬과 DBMS를 연결해서 데이터를 끌어올 수 있는 인터페이스가 존재한다.

파이썬에서 그러한 역할을 하는 라이브러리인 `cx_oracle` 그리고 `sqlAlchemy` 이다.

오늘은 이 두 가지 라이브러리를 직접 현업에서 활용해보았을 때 발생한 이슈를 가볍게 메모하고자 한다.

## Intro

먼저 국내 가장 많은 시장을 보유하고 있다고 알려진 Oracle DB에 연결시엔 `cx_oracle`을 사용한다.

그리고 기타 DB 연결시엔 `sqlAlchemy` 등등이 쓰인다.

그런데 특이하게도 Oracle DB 연결 시 **cx_oracle**만 사용해도 되지 않을까 싶었지만, 현업환경에서 **cx_oracle + sqlAlchemy**를 조합해서 쓰는 것을 보았다.

왜그랬던 것일까?

## 추측

테스트해본 결과 다음의 문제가 있었다.

- DB -> SQL -> Python (pandas DataFrame) 방향으로 데이터 전달 OK
- Python(pandas DataFrame) -> DB (table) 방향으로 데이터 전달 Error

``` {PYTHON}
# 라이브러리 로딩
import cx_Oracle
import pandas as pd

# DB 접속
#cx_Oracle.connect("DB_User/DB_PW@IP_NUM:PORT_NUM/ServiceID")
conn = cx_Oracle.connect('ID/PW@00.00.00.00:0000/SID', encoding='utf8')
cursor = conn .cursor()

# SQL read Dataframe
pandas_file = pd.read_sql_query("SQL_QUERY", cursor)

# 작업결과(pandas_file)을 DB 테이블(TABLE_NAME)으로 반환
pandas_file.to_sql('TABLE_NAME', conn) #error

## DatabaseError: Execution failed on sql 'SELECT name FROM sqlite_master WHERE type='table' AND name=?;': ORA-01036: 잘못된 변수명/번호
```

이것은 Pandas.DataFrame의 [to_sql 메소드](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_sql.html)에서 Oracle 커서를 바로 지원하지 않는 문제에서 기인한 것으로 여겨진다.

Pandas Document를 확인해보면, to_sql 메소드의 **con** 패러미터가 **sqlalchemy.engine.Engine** 또는 **sqlite3.Connection**만 지원하는 것을 알 수있다.

## 위 문제의 해결방법

가장 간단한 해결방법은 cx_oracle 대신 sqlAlchemy를 활용하는 것이다.

단, sqlAlchemy에서 OracleDB에 접속하기 위해선, cx_Oracle도 함께 로딩해서 사용해야 한다.

```{PYTHON}
# 라이브러리 로딩
import cx_Oracle
import pandas as pd
from sqlalchemy import create_engine

# DSN, TNS 정보 할당
dsn_tns = """
(DESCRIPTION=
    (ADDRESS_LIST=(ADDRESS=(PROTOCOL=TCP) (HOST=00.00.00.00)(PORT=0000)))
    (CONNECT_DATA=(SERVICE_NAME=SID)))
"""

# DB 접속, engine 생성
engine = create_engine('oracle+cx_oracle://ID:PW'+ '@%s' % dsn_tns)

# SQL read Dataframe
pandas_file = pd.read_sql_query("SQL_QUERY", engine )

# 작업결과(pandas_file)을 DB 테이블(TABLE_NAME)으로 반환
pandas_file.to_sql('TABLE_NAME', engine ) # OK
```

참고로 코드 중간에 들어가는 DSN과 TSN에 대한 간략한 설명은 다음과 같다.
>
> 파이썬 등 외부에서 DB 호출 시, DSN(Data Source Name, 데이터 원천 이름) 과 TNS(Transparent Network Substrate, Oracle에서 개발한 Network 기술~다른 DB와 연결하기 위한 ORACLE서버 정보를 가리킴)을 지정해주어야 한다. 

## 또 다른 해결방안?

수고스럽겠지만, cx_oracle 라이브러리를 이용해  ORACLE SQL 쿼리로 `CREATE TABLE` > `INSERT` 구문을 짜서 직접 데이터를 전송하는 방법도 남아있다.
- Python(pandas DataFrame) ->SQL(CREATE TABLE>INSERT) -> DB (table) 방향으로 데이터 전달

-----

끝

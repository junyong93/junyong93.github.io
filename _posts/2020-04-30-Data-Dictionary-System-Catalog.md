---
title: "Data Dictionary와 System Catalog"
categories: 
  - Miscellaneous
last_modified_at: 2020-04-30 12:00:00
toc: false #Table of Contents
comments: true
use_math: true # MathJax On
---

- DBMS를 배우다보면 나타나는 `Data Dictionary` 와 `System Catalog` 개념이 혼동이 되어, 그와 비스무리한 여러가지 개념들에 대해 알아보았다

1. Data Dictionary(데이터 사전) 또는 System Catalog(시스템 카탈로그)

  ㅇ [일반]
     - 시스템 또는 프로그램의 헤더부에 포함시켜, 
     - 내용 항목에 대한 식별자 및 그에 대한 정의,
     - 항목 엔티티의 구성요소, 항목이 저장되는 곳, 항목을 참조하는 곳 등을 포함하는 것

  ㅇ [데이터베이스] 
     - 데이터베이스 구조에 대한 메타데이터를 저장하는 곳


2. [관계형 데이터베이스]  카탈로그

  ㅇ 데이터베이스 스키마 및 릴레이션 정보에 대한 메타데이터를 이곳에 저장함
     - 릴레이션 이름
     - 각 릴레이션 속성의 이름
     - 무결성 제약조건
     - 데이터베이스 뷰 이름 및 그 정의 등

3. 표준메타
  ㅇ 데이터 사전 또는 카탈로그에 추가될 메타데이터의 값들의 대표값(컬럼명)
    - 즉 메타데이터의 표준.
	 
	
- 참고 1: http://www.ktword.co.kr/abbr_view.php?m_temp1=5200

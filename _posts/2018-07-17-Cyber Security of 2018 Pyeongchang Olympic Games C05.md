---

layout: post
title: "[컨퍼런스] 평창동계올림픽 정보보호체계"
description: "한국인터넷진흥원 신화수 수석연구원"
date: 2018-07-17
tags: [컨퍼런스, 정보보호의날, 정보보호, 평창동계올림픽]
comments: true
share: true

---

**목차**

---

* toc
{:toc}

## 평창동계올림픽 정보보호체계
#### 한국인터넷진흥원 신화수 수석연구원

<br>

## 평창동계올림픽 정보보호체계
### 1. 정보보호체계의 전략
1. 게임 특성의 고유한 특성을 고려하여 강력한 IT 보안 시스템 개발 및 운영
2. POCOG(PyeongChang Organizing Committee for the 018 Olypmic&Paralympic Winter Games)의 IT 핵심 자산 보호
3. 현지 법률 및 국제 표준에 따라 IT 보안 프레임워크 개발
4. 사이버 보안을 위해 POCOG, 정부 기관 및 파트너 사이의 긴밀한 협력을 촉진

- - -

### 2. 보호해야할 IT 자산
#### 네트워크 인프라

<img src="/assets/images/seminar/180711/0007.png" width="100%">

- 게임 네트워크
- 관리 네트워크
- 무선 네트워크
- 방송 네트워크
- 시설 네트워크

<br>

#### 올림픽 응용 서비스
- Olympic Management System(OMS)
   - 인증, 인력관리, 경쟁, 일정, 자원봉사 포털
- Olympic Diffusion System(ODS)
   - Info 2018/My info, 해설 정보 시스템, Data Feed 등
- Games Management System/WEB
   - GMS : 교통, 숙박 등, WEB : Pre-games, Games-Time, Test Event 등
- Administration System
   - 프로젝트 관리, Knowledge, Archive, 협업, 통합 금융, 내부 포털 등

<br>

#### 데이터 센터

<img src="/assets/images/seminar/180711/0008.png" width="100%">

<br>

#### IT 장비
- 노트북, PC 10,354대
- Wi-Fi 라우터 6,300대
- 태블릿 2,372대
- 모바일 디바이스 21,000대
- TV 7,130대
- 감시 카메라 810대
- 복사장치 2,665대

<br>

#### 공식 홈페이지 및 모바일 어플리케이션

- - -

### 3. 보안 위협과 거버넌스
#### 위험과 위협
- DDoS
- APT 공격
- 제로데이
- 랜섬웨어
- 피싱사기
- 응용프로그램 계층 공격
- 새로운 취약점

#### 사이버보안 거버넌스<br>
전국적인 사이버보안 거버넌스가 구축

<img src="/assets/images/seminar/180711/0009.png" width="100%">

- - -

### 3. 사이버보안 측정
#### 네트워크 보안<br>

**보안 및 신뢰성을 보장하기 위한 네트워크의 물리적 분리**

- 독립성
   - 5개의 주요 서비스를 위한 네트워크를 분리
- 네트워크 생존을 위한 이중화
   - 이중화 N/W 센터 : MNC&SNC
      - 강릉에 위치한 MNC(Main Network Centre)
      - 평창에 위치한 SNC(Secondary Network Centre)
   - 이중화 백본 & ISP N/W
- 접근 제어
- 장치에 의한 접근 제어
- 네트워크를 통한 방화벽 제어

- 인터넷 네트워크로부터 관리자 네트워크를 분리하는 VDI(Virdual Desktop Infrastructure)
   - 논리적으로 인터넷을 분할한다.
   - 인터넷 접근을 통해 잠재적인 시스템 위협을 예방

- 게임 네트워크의 관리자를 위한 인증 절차
- 로그 접근 및 게임 네트워크 관리자의 작업 내역
   - 전자서명, 생체 인증

<br>

#### 데이터 센터 보안<br>
게임 운영에 필수적인 70가지 소프트웨어에 대한 포괄적인 보호 조치

|다층 보호|
|---|
| - 1st : CDN(Content Delivery Network)에서 DDoS 공격 차단|
| - 2nd : 네트워크 서비스 공급자에 의한 DDoS 공격 차단|
| - 3rd : POCOG에 의한 DDoS 차단|
| - 4th : 침임 방지 시스템|
| - 5th : 방화벽|
| - 6th : 3개 구역으로 분리된 데이터 센터(DMZ, Trust, Secure)|
| - 7th : 웹 어플리케이션 방화벽|

<br>

#### 종단 보안
- 노트북, PC, BYOD, 키오스크, 태블릿, 스마트폰
- Anti-APT & Anti-Virus Solutions
   - 이메일과 웹 기반의 멀웨어 실시간 모니터링
   - 2단계의 이메일 검사, 1단계 바이러스 탐지, 2단계 첨부파일 스캔
- 소프트웨어 제한 정책(SRP, AppLocker)
   - 윈도우의 기본 보안 기능 활용
   - 원치 않는 S/W 또는 원치 않는 디렉토리 실행 금지
- 종단 보안 관리의 중앙화
   - 종단 보안 강화를 위한 PMS 채택
   - 종단 장치의 보안 상태 모니터링 및 IP 주소 관리
- MyPC-Safeguard 솔루션 및 사이버 보안 점검의 날
   - 중앙에서 제어되는 모든 PC에 대한 보안 검사
   - PC의 취약점을 정기적으로 줄이고 사이버 보안에 대한 인식을 향상
- 네트워크 접근 제어
   - 접근 제어 규칙 강화를 위한 MAC/IP/Username 기반 인증
   - POCOG N/W에 대한 무단 접근 금지
- 장치 인터페이스 제어
   - USB, 외부 저장 장치 등을 통한 악성코드 감염 예방
   - POCOG가 배포하는 보안 USB에 한해서 접근 가능

- - -

### 5. 사전 예방 전략
- 개인 정보 영향 평가
- 재해 복구 리허설
- 사이버 보안 자문위원회

<br>

#### Ref. 개회식 사고

- 개막식 사이버 공격으로 인해 텔레비전 및 인터넷 접속, 다른 서비스가 영향을 받았다.
- Olympic CERT, IOC 및 Partners와 협력하여 POCOG는 DRR 경험을 통해 중단된 서비스를 신속하게 복구하고 안정화했습니다.
- 우선 순위 선정 후 복구

- - -

### 6. Olympic CERT.

<img src="/assets/images/seminar/180711/0010.png" width="100%">

- - -

### 7. 올림픽 기간동안의 교훈

- 초기 단계에 모든것을 고쳐야 한다
- 사용의 편리성과 타협하지 말 것
- 이메일 보안은 태풍의 눈이다
- 공급망 관리의 보안
- 다른 조직과의 협력
- 최악의 경우를 항상 생각할 것

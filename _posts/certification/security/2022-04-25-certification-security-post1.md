---
layout: post
title: ETSI EN 303 645란?
description: IoT 보안
sitemap: true
hide_last_modified: true
categories:
  - certification
  - security
---

## __IoT기기 보안 이슈__
---
IT 산업에서 보안은 항상 중요한 화두로 강조되어 왔다.IoT 기기에도 보안이 필요할까?<br>
최근 IoT기기의 보안 이슈를 한 번 살펴보면
~~~
1. 올해 초 CCTV, 공유기 1만여대가 털려 해킹에 사용된 사건
2. 작년 아파트 월패드 해킹 사건
3. Baby Monitor 기기가 해킹되어 카메라로 집 안이 모니터링 된 사건
~~~
'초연결사회'에서 모든 네트워크가 연결되면서 이러한 개인정보들이 유출될 뿐만 아니라 심각하게는 생명에 위협까지도 이어질 수 있게 된다.

## __ETSI EN 303 645__
---
보안 위협에 따라 <span style='background-color: #f5f0ff'>ETSI EN 303 645</span>라는 새로운 IoT 보안 규정이 제정되었다. 기술적인 제어와 조직의 정책에 초점을 맞춘 IoT 제품 보안에 대한 지침을 제시하고 있다.
또한, <span style='background-color: #f5f0ff'>2024년 8월 1일</span> 부터 무선 장비에 대해 <span style='background-color: #f5f0ff'>RED(Radio Equipment Directive)</span> 무선기기지침이 강제화 됨에 따라 <span style='background-color: #f5f0ff'>2023년 HEN(Harmonized Standard)</span> 규격이 제정될 것으로 예정되어 있다.
유럽, 아시아 등 여러 국가들이 ETSI EN 303 645 테스팅에 주목하며 더욱 중요해지고 있다.

## __ETSI EN 303 645__ - <span style='color: #3c1ea1'>5번 조항</span>
---
5번 조항은 IoT 기기 테스트 요구사항을 담고 있다.

>__5 Cyber security provisions for consumer IoT__<br>
>소비자용 IoT를 위한 사이버 보안 조항

>__5.1 No universal default passwords__<br>
>보편적인 기본 비밀번호 금지

- 대부분의 IoT 기기는 기본 사용자 이름이나 암호(예: Username: "Admin", Password: "Admin/Password" 등)로 설정되어 판매되고 있다.
- OTP 절차를 사용하거나 무작위 암호 권장한다.

>__5.2 Implement a means to manage reports of vulnerabilities__<br>
>취약점 보고서를 관리하는 수단 구현

제조업체는 다음 정보를 정책에 포함해야 한다.
- 보안 이슈 보고를 위한 연락처 정보
- 최초 접수 확인 정보
- 보고된 문제가 해결될 때까지의 업데이트 등의 정보

>__5.3 Keep software updated__<br>
>소프트웨어 업데이트 유지

- 소프트웨어를 최신 상태로 유지하고, 유지보수되어야 한다.
- 안전하게 업데이트 가능해야 한다.

>__5.4 Securely store sensitive security parameters__<br>
>민감한 보안 파라미터 안전하게 저장

- 보안 저장 메커니즘이 사용되어야 한다.
- 하드 코딩된 주요 보안 파라미터는 사용 금지 되어야 한다.

>__5.5 Communicate securely__<br>
>안전한 통신

- Best practice에 맞게 암호화되어야 한다.
- 암호화는 리뷰 또는 평가된 뒤 사용되어야 한다.
- 보안 관련 변경은 인증 후에만 접근 가능하도록 해야 한다.

>__5.6 Minimize exposed attack surfaces__<br>
>노출된 공격 면적 최소화

- 사용하지 않는 모든 네트워크 및 인터페이스는 비활성화해야 한다.
- 코드는 서비스/기기가 작동하는 데 필요한 기능으로 최소화되어야 한다.

>__5.7 Ensure software integrity__<br>
>소프트웨어 무결성 보장

- 보안 부팅 메커니즘을 사용해서 소프트웨어가 확인되어야 한다.
- 무단 소프트웨어 변경이 가능하면 경고 및 차단이 필요하다.

>__5.8 Ensure that personal data is secure__<br>
>개인 데이터가 안전한지 확인

- 개인 데이터는 Best practice에 맞는 암호화가 필요하다.
- 기기의 외부 감지 기능은 사용자에 명확하고 투명한 방식으로 문서화 되어야 한다.

>__5.9 Make systems resilient to outages__<br>
>시스템 가동 중단에 대한 복원력 구현

- 네트워크나 전원 중단 시 IoT 기기 및 서비스 복원력이 필요하다.
- 네트워크 및 전원이 복구된 경우에 완전한 복구가 필요하다.

>__5.10 Examine system telemetry data__<br>
>시스템 원격 측정 데이터 검사

- 원격 측정 데이터를 수집하는 경우 보안 이상이 있는지 확인이 필요하다.
- 원격 데이터 수집 시 개인 데이터가 보호되어야 한다.

>__5.11 Make it easy for users to delete user data__<br>
>사용자가 쉽게 사용자 데이터를 삭제할 수 있도록 한다

- 기기에서 사용자 데이터를 간단한 방법으로 삭제할 수 있는 기능이 제공되어야 한다.
- 사용자에게 개인 데이터가 삭제되었다는 명확한 확인이 되어야 한다.

>__5.12 Make installation and maintenance of devices easy__<br>
>기기 설치 및 유지보수를 쉽게 한다

- 사용성에 대한 보안 Best practice를 따라야 한다.
- 기기를 안전하게 설정하는 방법 및 결과 확인 방법이 제공되어야 한다.

>__5.13 Validate input data__<br>
>입력 데이터 검증

- 사용자 인터페이스, API, 네트워크 간의 데이터 입력의 검증이 필요하다.
- 데이터 형식이나 데이터 범위 등이 확인 되어야 한다. 


정확하고 더 많은 정보는 ETSI EN 303 645 표준 원문을 보는 것을 추천한다.끝!

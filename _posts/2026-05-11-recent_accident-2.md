---
title: "Oracle E-Business Suite 제로데이 공격 사건"
date: 2026-05-11 11:00:00 +0900
categories: [블로그&기술문서, article, Zero-Day]
tags: [Oracle, CVE-2025-61882, RCE, CLOP, Enterprise Security]
---

# Oracle E-Business Suite 제로데이 공격 사건

## 사건 개요

2025년 하반기 발생한 Oracle E-Business Suite(EBS) 제로데이 공격 사건은  
최근 가장 위험했던 엔터프라이즈 대상 보안 사고 중 하나로 평가된다.

Oracle E-Business Suite는 전 세계 대기업과 공공기관에서 사용하는  
대표적인 ERP(Enterprise Resource Planning) 플랫폼이며,  
인사·재무·구매·급여·문서 관리 등 핵심 업무 시스템을 담당한다.

문제가 된 취약점은 CVE-2025-61882로 식별되었으며,  
인증 없이 원격 코드 실행(Remote Code Execution)이 가능한 치명적인 제로데이 취약점이었다.

특히 공격자는 계정이나 비밀번호 없이 인터넷에 노출된 Oracle EBS 서버에 접근할 수 있었고,  
이를 통해 내부 시스템 장악과 데이터 탈취를 시도했다.

보안 기업 CrowdStrike와 Google Threat Intelligence Group은  
이번 공격이 단순 테스트 수준이 아니라 실제 대규모 데이터 탈취 캠페인으로 이어졌다고 분석했다.

또한 CLOP(Cl0p) 계열 공격 조직이 해당 취약점을 악용한 정황이 확인되었으며,  
다수 기업들이 협박 메일(extortion email)을 받은 사실도 공개되었다.

이번 사건은 단순 취약점 사고가 아니라  
"인터넷에 노출된 핵심 업무 시스템"이 얼마나 위험한지 보여준 대표적 사례라고 볼 수 있다.

---

# 사고 원인

이번 사고의 핵심 원인은  
인터넷에 노출된 Oracle EBS 시스템과 인증 우회 가능한 제로데이 취약점의 결합이었다.

취약점 분석에 따르면 공격자는 다음과 같은 과정을 통해 침투를 시도했다.

```text
SSRF 취약점 악용
→ 내부 서비스 접근
→ 인증 우회(Authentication Bypass)
→ 악성 요청 실행
→ 원격 코드 실행(RCE)
→ 웹쉘(WebShell) 업로드
→ 데이터 탈취
```

특히 이번 취약점은 단일 버그가 아니라 여러 취약점 체인이 연결된 형태로 분석되었다.

보안 연구자들은 다음과 같은 요소가 결합되었다고 설명했다.

- SSRF(Server-Side Request Forgery)
- CRLF Injection
- 내부 서비스 노출
- XSLT 처리 취약점
- 인증 우회
- 원격 코드 실행

가장 심각했던 부분은  
공격 성공에 사용자 인증이 전혀 필요하지 않았다는 점이다.

즉, 인터넷에 노출된 Oracle EBS 서버라면  
외부 공격자가 직접 시스템 명령 실행 단계까지 도달할 가능성이 존재했다.

또한 일부 기업들은 Oracle EBS를 VPN이나 내부망 뒤에 두지 않고  
외부 인터넷에 직접 노출한 상태로 운영하고 있었고,  
이 때문에 공격 범위가 더 확대된 것으로 분석된다.

---

# 대응 방식

Oracle은 취약점 공개 이후 긴급 패치를 배포했으며,  
보안 권고문을 통해 즉시 업데이트 적용을 권장했다.

또한 Oracle은 다음과 같은 침해 지표(IOC)를 공개했다.

- 비정상 외부 연결 시도
- 수상한 JSP 파일 생성
- BI Publisher 관련 이상 동작
- 비인가 프로세스 실행 흔적

보안 기업들과 정부 기관 역시 대응에 나섰다.

- CrowdStrike는 대규모 악용 정황 발표
- CISA는 Known Exploited Vulnerabilities 목록 추가
- Google GTIG는 실제 데이터 탈취 캠페인 확인
- 여러 기업들이 긴급 패치 및 로그 분석 수행

일부 기업은 사고 이후 다음과 같은 추가 조치를 진행했다.

- 인터넷 노출 EBS 서버 차단
- VPN 기반 접근 구조 전환
- 웹쉘 탐지
- 계정 재설정
- 로그 포렌식
- 외부 연결 차단 정책 강화

그러나 문제는 단순 패치만으로 끝나지 않았다.

일부 보안 연구자들은  
패치 이후에도 특정 SSRF 동작이 완전히 차단되지 않았다고 지적했고,  
Reddit 및 보안 커뮤니티에서는  
“패치가 불완전한 것 같다”는 분석도 지속적으로 올라왔다.

---

# 한계와 시사점

이번 사건에서 가장 중요하게 드러난 문제는  
기업 핵심 업무 시스템이 여전히 인터넷에 직접 노출되어 있다는 점이다.

ERP 시스템은 일반 웹사이트와 달리  
재무, 인사, 계약, 내부 문서 등 매우 민감한 데이터를 다루기 때문에  
침해 발생 시 피해 규모가 매우 커질 수 있다.

또한 이번 사고는  
“제로데이 자체”보다도  
“패치 이전까지 얼마나 오래 노출되었는가”가 더 위험한 문제였다고 볼 수 있다.

CrowdStrike 분석에 따르면  
실제 공격은 2025년 8월부터 시작된 것으로 추정되며,  
공개 및 패치 이전까지 상당 기간 악용되었을 가능성이 제기되었다.

즉, 많은 기업들이 자신도 모르는 상태에서 이미 침해되었을 가능성이 존재했다.

또한 이번 사건은 다음과 같은 문제점을 보여준다.

- 인터넷 노출 ERP 운영 위험성
- 제로데이 대응 한계
- 인증 우회 취약점의 파급력
- 웹쉘 기반 장기 침투 가능성
- 패치 의존형 보안 구조의 한계

특히 공격자들은 단순 랜섬웨어 실행보다  
데이터 탈취 후 협박(Extortion) 방식을 적극적으로 사용했다.

이는 최근 공격 그룹들이  
“암호화”보다 “정보 유출”을 더 효과적인 수익 모델로 활용하고 있다는 점을 보여준다.

결국 Oracle EBS 사건은  
단순 취약점 사고가 아니라  
기업 핵심 인프라가 인터넷에 연결된 상태에서  
제로데이 공격이 발생할 경우 얼마나 빠르게 대규모 침해로 이어질 수 있는지를 보여준 대표적인 사례라고 볼 수 있다.

---

# 참고 자료

- [CrowdStrike 공식 분석](https://www.crowdstrike.com/en-us/blog/crowdstrike-identifies-campaign-targeting-oracle-e-business-suite-zero-day-CVE-2025-61882/)
- [TechCrunch - Oracle EBS 공격 기사](https://techcrunch.com/2025/10/06/clop-hackers-caught-exploiting-oracle-zero-day-bug-to-steal-executives-personal-data/)
- [The Hacker News 분석](https://thehackernews.com/2025/10/oracle-ebs-under-fire-as-cl0p-exploits.html)
- [CISA KEV 등록](https://www.cisa.gov/news-events/alerts)

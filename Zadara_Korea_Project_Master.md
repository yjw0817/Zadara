# Zadara Korea — 프로젝트 마스터 문서

> 💡 **새 Claude 세션 시작 시 이 파일을 업로드하면 맥락이 즉시 복원됩니다**
> 작성일: 2026년 2월 | 버전: v1.0

---

## 1. 프로젝트 배경 & 목표

### 핵심 목표
- Zadara Cloud 인프라를 기반으로 **한국형 웹 포털** 자체 개발
- 한국 공공시장 진출을 위한 **CSAP 인증** 획득
- 조달청 디지털서비스몰 등록을 통한 **공공납품** 실현

### 회사 구조
| 구분 | 내용 |
|------|------|
| 본사 | Zadara (이스라엘계 글로벌 클라우드) |
| 한국 지사 | 포털 구축 주도 |
| 대상 인프라 | https://compute-iaas-kr1-01.zadara.com (Korea KR1 Region) |

### 내 포지션
| 항목 | 내용 |
|------|------|
| 직함 | CTO / 기술이사 (Korea) |
| 연봉 | 7,000만원 (협의됨) |
| 역할 | 개발 설계·감독, 전략 기획, 보안 인증 총괄 |
| 비고 | 업무 범위 대비 연봉 상향 여지 있음. 직함·역할범위·성과인센티브 명문화 필요 |

---

## 2. Zadara 기술 분석 결과

> 면접 후 직접 시스템에 접속하여 분석 완료 — 19개 페이지, 60+ API 엔드포인트

### 플랫폼 구조

**Frontend**
- Vue.js 기반 SPA
- Chart.js / ECharts (모니터링 차트)
- Socket.IO 실시간 스트리밍 확인
- 이벤트 폴링: 5초 주기 + Socket.IO 병행

**Backend API**
- REST API: /api/v2/ 구조
- OpenStack Keystone 인증 (2단계)
- Opaque Token 방식 (JWT 아님) — Base64 인코딩 불투명 토큰
- X-Auth-Token 헤더로 API 호출

### 인증 흐름 (2단계)

```
1. POST /api/v2/identity/auth                     → Domain Token 발급
2. GET  /api/v2/identity/users/myself/projects    → 프로젝트 목록 조회
3. POST /api/v2/identity/auth                     → Project Token 발급 (2단계)
4. 이후 모든 API 호출: Header X-Auth-Token: {token}
5. 토큰 만료 시 → 자동 갱신 로직 필요
```

### 주요 API 엔드포인트

| 영역 | 엔드포인트 | 상태 |
|------|-----------|------|
| 인증 | POST /api/v2/identity/auth | ✅ 정상 |
| Compute | GET /api/v2/compute/vms | ✅ 정상 |
| Compute | GET /api/v2/compute/vm-snapshots | ✅ 정상 |
| Compute | GET /api/v2/compute/key-pairs | ✅ 정상 |
| Compute | GET /api/v2/autoscaling-groups/groups | ✅ 정상 |
| VPC Network | GET /api/v2/vpcs | ✅ 정상 |
| VPC Network | GET /api/v2/vpcs/networks | ✅ 정상 |
| VPC Network | GET /api/v2/vpcs/elastic-ips | ✅ 정상 |
| VPC Network | GET /api/v2/vpcs/nat-gateways | ✅ 정상 |
| VPC Network | GET /api/v2/vpcs/internet-gateways | ✅ 정상 |
| VPC Network | GET /api/v2/route53/zones | ✅ 정상 |
| LBaaS | GET /api/v2/lbaas/load-balancers | ✅ 정상 |
| LBaaS | GET /api/v2/lbaas/listeners | ✅ 정상 |
| Storage | GET /api/v2/volumes | ❌ 404 (VPSA 미연동) |
| Monitoring | GET /api/v2/events/query | ✅ 정상 |
| Monitoring | GET /api/v2/alarm/alarms | ✅ 정상 |
| Monitoring | GET /api/v2/cloudwatch-backend/metrics | ✅ 정상 |
| Identity | GET /api/v2/identity/users | ✅ 정상 |
| Identity | GET /api/v2/identity/projects | ✅ 정상 |
| Images | GET /api/v2/machine-images | ✅ 정상 |
| Config | GET /api/v2/engines | ✅ 정상 |
| Config | GET /api/v2/cluster/hardware | ❌ 403 (Admin 전용) |

> ⚠️ API Swagger Spec 공식 문서 확보 필요 — 확보 즉시 PoC 착수 가능

---

## 3. 전략 시나리오

### 핵심 분기점
> **"Zadara 본사가 한국 CSAP 인증 추진에 협조할 의향이 있는가?"**

### 시나리오 A — 본사 협조 가능 (권장)
Zadara 인프라 그대로 + 한국 포털 → CSAP 취득. 가장 빠르고 강력한 경로.
예상 기간: 1~1.5년 | 인원: 5~8명

| 단계 | 내용 | 기간 |
|------|------|------|
| 1 | 포털 개발 (한국어 UI, 원화 과금, 전체 기능) | 2~3개월 |
| 2 | 보안 기능 구현 (접근로그·암호화·세션관리) | 2~3개월 |
| 3 | CSAP 신청·심사 (KISA 서류 및 현장 심사) | 6개월~1년 |
| 4 | 조달청 등록 → 공공납품 완료 | - |

### 시나리오 B — 본사 협조 어려움
CSAP 인증된 국내 클라우드(KT/NHN) 인프라 활용. Zadara는 백엔드 API로만 연동.
예상 기간: 1.5~2년 | 인원: 8~12명

### 난이도 분석

| 구분 | 난이도 | 예상 기간 | 비고 |
|------|--------|-----------|------|
| 웹 포털 UI 전체 구현 | 🟢 쉬움 | 1~3개월 | 이미 데모 완성 가능 |
| 백엔드 / 인프라 구축 | 🟡 보통 | 2~4개월 | 개발팀 보유 시 가능 |
| ISMS-P 인증 | 🔴 어려움 | 6개월~1년 | 정책·문서·조직 체계 필요 |
| CSAP 인증 (공공 필수) | 🔴 매우 어려움 | 1~2년 | 공공납품 필수 |

### 참고: 전자정부 프레임워크
- **Zadara 포털에는 불필요** — SaaS 서비스이므로 기술 스택 자유 선택
- 전자정부 프레임워크는 공공기관이 직접 발주하는 SI 프로젝트에만 해당
- React, Vue.js, Python FastAPI 등 자유롭게 사용 가능

---

## 4. 만들어야 할 문서 목록

### 1단계 — 사업 기획 문서 (지금 당장)
- [x] 제안서 (Proposal) — 슬라이드 완성됨
- [x] 기술 분석 보고서 — HTML 완성됨
- [ ] 사업계획서 — 시장 규모, 수익 모델, 손익 분기점
- [ ] 파트너십 협약서 초안

### 2단계 — 개발 설계 문서 (개발 착수 전)
- [ ] 요구사항 정의서 (SRS)
- [ ] 시스템 아키텍처 설계서
- [ ] API 설계서 (Swagger 형식)
- [ ] DB 설계서 (ERD)
- [ ] UI/UX 와이어프레임

### 3단계 — 보안/운영 문서 (CSAP 인증 전 필수)
- [ ] 정보보호 정책서
- [ ] 개인정보 처리방침
- [ ] 접근통제 정책서
- [ ] 암호화 정책서
- [ ] 취약점 점검 보고서
- [ ] 재해복구(DR) 계획서
- [ ] 보안 사고 대응 절차서

### 4단계 — CSAP 신청 문서
- [ ] 서비스 설명서 (데이터 흐름도 포함)
- [ ] 보안 통제 항목 이행 증적
- [ ] 클라우드 인프라 구성도
- [ ] SLA (서비스 수준 협약서)

### 5단계 — 영업/계약 문서 (공공 납품 시)
- [ ] 서비스 소개서 (1~2페이지)
- [ ] 표준 이용약관
- [ ] 표준 계약서
- [ ] 조달청 등록 신청서

---

## 5. 제안 아키텍처

### 기술 스택

| 레이어 | 기술 |
|--------|------|
| Frontend | Vue.js 또는 React, Chart.js/ECharts |
| Backend | Python FastAPI |
| 인증 연동 | OpenStack Keystone, X-Auth-Token |
| 이벤트 처리 | Celery + Redis (폴링 방식) |
| DB | PostgreSQL (메인), Redis (캐시), InfluxDB (메트릭) |
| API Gateway | Nginx / Kong |
| DevOps | GitLab CI/CD, Docker, Kubernetes |
| 배포 환경 | 클라우드 (서버실 불필요, 월 3~10만원) |

---

## 6. 구현 로드맵

| Phase | 기간 | 주요 내용 | 마일스톤 |
|-------|------|-----------|----------|
| Phase 1: Foundation | 1개월 | 요구사항 분석, API 설계, UX 기획, DevOps 환경 | 아키텍처 확정 및 개발 환경 세팅 |
| Phase 2: Core Features | 2개월 | VM/스토리지/네트워크 관리, 계정/권한, 기본 모니터링 | 핵심 IaaS 기능 구현 및 알파 테스트 |
| Phase 3: Advanced | 2개월 | 이벤트 대시보드, 알림 시스템, 과금 리포트, 감사 로그 | 베타 서비스 오픈 |
| Phase 4: Optimization | 지속 | 운영 최적화, CSAP 준비, 브랜딩 고도화, 문서화 | 정식 서비스 런칭 및 공공 납품 |

---

## 7. 다음 단계 액션 아이템

1. **제안서 최종 확정** — 슬라이드 보완 완료 → Zadara 본사 미팅 자료 준비
2. **CSAP 협조 의향 확인** — 입사 후 가장 먼저 본사에 확인 (시나리오 A vs B 결정)
3. **근로계약서 검토** — 직함(CTO/기술이사), 역할 범위, 성과 인센티브 명문화
4. **사업계획서 작성** — 시장 규모, 수익 모델, 손익 분기점 (Claude와 함께)
5. **Swagger API 문서 확보** — 입사 후 내부 문서 확보 → PoC 즉시 착수
6. **요구사항 정의서(SRS) 작성** — 개발 착수 전 필수 (Claude와 함께)

---

## 8. Claude 활용 가이드

### 새 세션 시작 시 Claude에게 전달할 내용

```
나는 Zadara 한국 지사의 CTO(기술이사)로 입사 예정입니다.
Zadara Cloud(compute-iaas-kr1-01.zadara.com, Korea KR1)를 기반으로
한국형 클라우드 관리 포털을 직접 개발하고,
CSAP 인증을 통해 공공시장에 진출하는 것이 목표입니다.

이미 완료된 것:
- Zadara 시스템 전체 API 분석 (19개 페이지, 60+ 엔드포인트)
- 기술 분석 보고서 (HTML) 완성
- 제안서 슬라이드 완성
- 프로젝트 마스터 문서 (이 파일)

현재 진행할 작업: [여기에 오늘 할 일을 입력하세요]
```

### Claude가 할 수 있는 것 ✅
- 코드 작성 (프론트엔드 / 백엔드 / 인프라)
- 문서 초안 작성 (정책서, 설계서, 제안서 등)
- 아키텍처 설계 및 검토
- CSAP 준비 체크리스트 및 보안 문서 작성
- 발표 자료 및 슬라이드 내용 검토

### 본인이 직접 해야 하는 것 ⚠️
- KISA 현장 심사 대응 (물리적 존재 필요)
- 법인 계약 및 서명
- Zadara 본사 미팅 참석
- 조달청 등록 및 영업
- 팀원 채용 인터뷰

---

*Zadara Korea — 프로젝트 마스터 문서 v1.0 | 2026년 2월*

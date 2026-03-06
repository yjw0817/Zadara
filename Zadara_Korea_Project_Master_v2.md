# Zadara Korea — 프로젝트 마스터 문서 v2.0

> 💡 **새 Claude 세션 시작 시 이 파일을 업로드하면 맥락이 즉시 복원됩니다**
> 최종 업데이트: 2026년 3월 | 버전: v2.0

---

## 0. Claude에게 전달할 컨텍스트 (새 세션 시작 시 복사해서 사용)

```
나는 이비즈테크(Zadara 한국 총판/지사)에 CTO(기술이사)급으로 입사 예정인 윤정원입니다.
Zadara Cloud(compute-iaas-kr1-01.zadara.com, Korea KR1)를 기반으로
한국형 클라우드 관리 포털(CMP)을 직접 개발하고,
CSAP 인증을 통해 공공/금융 시장에 진출하는 것이 목표입니다.

현재 상태:
- 1차 면접 완료 (2026년 2월 27일)
- 포트폴리오 제출 완료
- PoC 계정 수령 예정 (2주 사용 가능)
- 2차 미팅 준비 중

이미 완료된 것:
- Zadara 시스템 전체 API 분석 (19개 페이지, 60+ 엔드포인트)
- 공식 API 문서 확인 (guides.zadarastorage.com / api-doc.zadarastorage.com)
- 기술 분석 보고서 (HTML) 완성
- 제안서 슬라이드 완성 및 수정
- 포트폴리오 완성
- 프로젝트 마스터 문서 v2.0 (이 파일)

오늘 진행할 작업: [여기에 입력하세요]
```

---

## 1. 회사 및 프로젝트 배경

### 회사 정보
| 항목 | 내용 |
|------|------|
| 회사명 | 이비즈테크 |
| 역할 | Zadara 한국 총판 / 지사장 운영 |
| 매출 | 약 100억원 수준 |
| 주요 실적 | LG전자, 삼성 등 대기업 Zadara 납품 성공 |
| 사무실 | 경기도 고양시 (일산) |
| 근무 조건 | 9시~6시, 월 1회 직원 예배 참석 문화 |
| 개발팀 현황 | **개발자 전무 — 이번이 첫 개발 인력 채용** |

### 핵심 목표
- Zadara Cloud 인프라를 기반으로 **한국형 CMP(클라우드 관리 포털)** 자체 개발
- 한국 공공/금융 시장 진출을 위한 **CSAP 인증** 획득
- 조달청 디지털서비스몰 등록을 통한 **공공납품** 실현
- 향후 NVIDIA GPU 사업 연계 개발도 계획 중

### 내 포지션
| 항목 | 내용 |
|------|------|
| 직함 | CTO / 기술이사 (Korea) — 한국 직급으로 "기술이사" |
| 연봉 | 7,000만원 (협의됨, 업무 범위 대비 상향 여지 있음) |
| 역할 | 개발 설계·감독, 전략 기획, 보안 인증 총괄 |
| 수습 | 3개월 (연봉 그대로 지급) |
| 비고 | 직함·역할범위·성과인센티브 명문화 필요 |

---

## 2. 면접 주요 내용 (2026년 2월 27일, 1차 면접)

### 회사가 원하는 것
- 단순 개발자가 아닌 **"이 사업을 주도적으로 리드할 수 있는 사람"**
- CMP 개발 + 인증 준비를 함께 이끌어 줄 리더
- 초기에는 1인 체제로 스타트, 이후 팀 빌딩

### 면접에서 언급된 기술 요구사항
- Zadara 포털 국산화 (한국형 CMP 개발)
- **보안 기능 확인서** 인증 취득 (250개 항목 — 암호화, 2차 인증, 통신 보안 등)
- 인증 후 공공기관/금융기관 납품
- API 기반 연동 (직접 DB 접근 불가, API를 통해서만 데이터 접근)

### 다음 단계 (면접 후 합의된 사항)
1. 포트폴리오 제출 (완료)
2. PoC 계정 수령 (2주 사용 가능) — 대기 중
3. 2차 미팅 일정 조율

---

## 3. Zadara 기술 분석 결과

### 공식 문서 (새로 확인됨 ✅)
| 문서 | URL |
|------|-----|
| 전체 가이드 홈 | https://guides.zadarastorage.com/ |
| zCompute Compute 가이드 | https://guides.zadarastorage.com/cs-compute-guide/latest/ |
| zCompute Networking 가이드 | https://guides.zadarastorage.com/cs-networking-guide/latest/ |
| zCompute Identity & Access | https://guides.zadarastorage.com/cs-iam-guide/latest/ |
| **Provisioning Portal API (Swagger)** | https://api-doc.zadarastorage.com/?urls.primaryName=Provisioning%20Portal |
| Command Center API | https://api-doc.zadarastorage.com/?urls.primaryName=Command%20Center |

> ✅ **포트폴리오 수정 필요:** "API Swagger Spec 확보 후 즉시 착수" → "공식 API 문서 확인 완료, 즉시 착수 가능"

### API 구조 (두 레이어)
| 레이어 | Base URL | 용도 |
|--------|----------|------|
| Provisioning Portal API | /api/v2/ | 테넌트/사용자용 — **포털 구축에 사용** |
| Command Center API | /api/ | 하드웨어/인프라 관리자용 — 본사 운영팀용 |

### 플랫폼 구조
**Frontend:** Vue.js 기반 SPA / Chart.js·ECharts / Socket.IO 실시간 스트리밍

**Backend API:** REST /api/v2/ / OpenStack Keystone 인증 (2단계) / Opaque Token (JWT 아님)

### 인증 흐름
```
1. POST /api/v2/identity/auth          → Domain Token 발급 (Opaque Token)
2. GET  /api/v2/identity/users/myself/projects  → 프로젝트 목록
3. POST /api/v2/identity/auth          → Project Token 발급 (2단계)
4. 이후 모든 API: Header X-Auth-Token: {token}
5. 토큰 만료 → 자동 갱신 로직 필요
```

### 주요 API 엔드포인트
| 영역 | 엔드포인트 | 상태 |
|------|-----------|------|
| 인증 | POST /api/v2/identity/auth | ✅ |
| Compute | GET /api/v2/compute/vms | ✅ |
| Compute | GET /api/v2/autoscaling-groups/groups | ✅ |
| VPC Network | GET /api/v2/vpcs | ✅ |
| VPC Network | GET /api/v2/vpcs/elastic-ips | ✅ |
| VPC Network | GET /api/v2/vpcs/nat-gateways | ✅ |
| LBaaS | GET /api/v2/lbaas/load-balancers | ✅ |
| Storage | GET /api/v2/volumes | ❌ 404 (VPSA 미연동) |
| Monitoring | GET /api/v2/events/query | ✅ |
| Monitoring | GET /api/v2/alarm/alarms | ✅ |
| Monitoring | GET /api/v2/cloudwatch-backend/metrics | ✅ |
| Identity | GET /api/v2/identity/users | ✅ |
| Identity | GET /api/v2/identity/projects | ✅ |
| Images | GET /api/v2/machine-images | ✅ |
| Config | GET /api/v2/engines | ✅ |
| Config | GET /api/v2/cluster/hardware | ❌ 403 (Admin 전용) |

---

## 4. 전략 시나리오

### ⭐ 핵심 분기점
> **"Zadara 본사가 한국 CSAP 인증 추진에 협조할 의향이 있는가?"**

### 시나리오 A — 본사 협조 가능 ✅ (권장)
- Zadara 인프라 그대로 + 한국 포털 → CSAP 취득
- 기간: 1~1.5년 | 인원: 5~8명

| 단계 | 내용 | 기간 |
|------|------|------|
| 1 | 포털 개발 (한국어 UI, 원화 과금) | 2~3개월 |
| 2 | 보안 기능 구현 (암호화·세션관리·2차인증) | 2~3개월 |
| 3 | CSAP 신청·심사 (KISA 서류·현장심사) | 6개월~1년 |
| 4 | 조달청 등록 → 공공납품 | - |

### 시나리오 B — 본사 협조 어려움 🔄
- KT Cloud G / NHN Cloud CSAP 인증존 활용
- Zadara는 백엔드 API로만 연동
- 기간: 1.5~2년 | 인원: 8~12명

### 난이도 분석
| 구분 | 난이도 | 예상 기간 |
|------|--------|-----------|
| 웹 포털 UI 전체 구현 | 🟢 쉬움 | 1~3개월 |
| 백엔드 / 인프라 구축 | 🟡 보통 | 2~4개월 |
| ISMS-P 인증 | 🔴 어려움 | 6개월~1년 |
| CSAP 인증 (공공 필수) | 🔴 매우 어려움 | 1~2년 |

### 인증 관련 중요 정리
- 면접에서 언급: **"보안 기능 확인서"** (250개 항목)
- 공공납품에 필요한 것: **CSAP (클라우드 보안 인증제)**
- **CC인증은 해당 없음** — SaaS 포털에는 CSAP가 적합, CC인증은 보안장비/OS용
- 2차 미팅에서 확인 필요: "추진하시는 인증이 CSAP인가요, 보안 기능 확인서인가요?"

### 참고: 전자정부 프레임워크
- Zadara 포털은 SaaS 서비스 → **전자정부 프레임워크 불필요**
- React, Vue.js, Python FastAPI 등 자유 기술 스택 사용 가능
- 전자정부 프레임워크는 공공기관이 직접 발주하는 SI 프로젝트에만 해당
- 서버실도 불필요 — 클라우드 배포 (월 3~10만원)

---

## 5. 포트폴리오 수정 사항

### 완료된 수정
- 슬라이드 11: 하단 문구 → "API Swagger Spec 확보 후 즉시 PoC 착수가능" ✅
- 슬라이드 12: "WebSocket 미지원" → "Socket.IO 실시간 스트리밍 확인" ✅
- 슬라이드 12: Celery+Redis 설명에 이유 추가 ✅

### 추가로 수정 필요한 것
- **슬라이드 11:** "API Swagger Spec 확보 후 즉시 착수" → "✅ 공식 API 문서 확인 완료 (api-doc.zadarastorage.com)"
- **슬라이드 13 (구현 가능성):** 하단 리스크 박스 → "✅ RESOLVED: 공식 Swagger 문서 확보 완료"
- **슬라이드 14 (핵심 상황):** "API 접근 문제 해결됨" 카드에 공식 문서 URL 추가
- **난이도 분석 슬라이드:** CC인증 항목 제거 → CSAP만 유지
- **프로필 요약:** 생년월일 제거 또는 나이만 표기

---

## 6. 제안 아키텍처

### 기술 스택
| 레이어 | 기술 |
|--------|------|
| Frontend | Vue.js 또는 React, Chart.js/ECharts |
| Backend | Python FastAPI |
| 인증 연동 | OpenStack Keystone, X-Auth-Token (Opaque Token) |
| 이벤트 처리 | Celery + Redis (Socket.IO 서버 직접 연동 대신 API 폴링으로 호환성 확보) |
| DB | PostgreSQL (메인), Redis (캐시), InfluxDB (메트릭) |
| API Gateway | Nginx / Kong |
| DevOps | GitLab CI/CD, Docker, Kubernetes |
| 배포 환경 | 클라우드 (서버실 불필요) |

---

## 7. 구현 로드맵

| Phase | 기간 | 주요 내용 |
|-------|------|-----------|
| Phase 1: Foundation | 1개월 | 요구사항 분석, API 설계, UX 기획, DevOps 환경 |
| Phase 2: Core Features | 2개월 | VM/스토리지/네트워크 관리, 계정/권한(JWT/RBAC), 기본 모니터링 |
| Phase 3: Advanced | 2개월 | 이벤트 대시보드, 알림 시스템, 과금 리포트, 감사 로그 |
| Phase 4: Optimization | 지속 | 운영 최적화, CSAP 준비, 브랜딩, 문서화 |

---

## 8. 만들어야 할 문서 목록

### 1단계 — 사업 기획 (지금 당장)
- [x] 제안서 슬라이드 — 완성
- [x] 기술 분석 보고서 (HTML) — 완성
- [x] 포트폴리오 — 완성 (수정 사항 일부 남음)
- [ ] 사업계획서 — 시장 규모, 수익 모델, 손익 분기점
- [ ] 파트너십 협약서 초안

### 2단계 — 개발 설계 (개발 착수 전)
- [ ] 요구사항 정의서 (SRS)
- [ ] 시스템 아키텍처 설계서
- [ ] API 설계서 (Swagger)
- [ ] DB 설계서 (ERD)
- [ ] UI/UX 와이어프레임

### 3단계 — 보안/운영 (CSAP 인증 전 필수)
- [ ] 정보보호 정책서
- [ ] 개인정보 처리방침
- [ ] 접근통제 정책서
- [ ] 암호화 정책서
- [ ] 취약점 점검 보고서
- [ ] 재해복구(DR) 계획서
- [ ] 보안 사고 대응 절차서

### 4단계 — CSAP 신청
- [ ] 서비스 설명서 (데이터 흐름도 포함)
- [ ] 보안 통제 항목 이행 증적
- [ ] 클라우드 인프라 구성도
- [ ] SLA (서비스 수준 협약서)

### 5단계 — 영업/계약 (공공 납품 시)
- [ ] 서비스 소개서
- [ ] 표준 이용약관
- [ ] 표준 계약서
- [ ] 조달청 등록 신청서

---

## 9. 2차 미팅 준비 사항

### 반드시 확인할 질문
1. **인증 종류 확인:** "추진하시는 인증이 CSAP인가요, 보안 기능 확인서인가요?"
2. **본사 협조 여부:** "Zadara 본사가 CSAP 인증 추진에 협조할 의향이 있나요?" (시나리오 A vs B 결정)
3. **역할 범위 명문화:** 직함(CTO/기술이사), 역할 범위, 성과 인센티브 조건

### 가져갈 것
- 포트폴리오 (수정본)
- 초기 개발 공수 견적 초안
- 공식 API 문서 확인 결과 (guides.zadarastorage.com)

### 어필 포인트
- 면접 후 공식 API 문서(guides.zadarastorage.com)까지 직접 확인
- Provisioning Portal /api/v2/ Swagger 문서 확보 완료
- 리스크가 이미 해소되었음을 증명

---

## 10. 상황 인식 및 마음가짐

### 걱정 vs 현실
| 걱정 | 현실 |
|------|------|
| "책임급을 감당할 수 있을까?" | 이미 이번 분석/기획/포트폴리오 작업이 CTO 역할 그 자체 |
| "개발팀 관리 경험이 없다" | 초기엔 1인 체제, 팀 관리는 나중에 |
| "클라우드가 낯설다" | Zadara 직접 분석 완료, 공식 문서까지 확인 |
| "인증을 모른다" | 회사도 컨설팅 업체 활용 예정, Claude와 함께 준비 가능 |

### Claude 활용 범위
**할 수 있는 것 ✅:** 코드 작성, 문서 초안, 아키텍처 설계, CSAP 준비 체크리스트, 슬라이드 검토

**본인이 직접 해야 하는 것 ⚠️:** KISA 현장 심사, 법인 계약·서명, 본사 미팅, 조달청 영업, 팀원 채용

---

*이비즈테크 Zadara Korea CMP 프로젝트 마스터 문서 v2.0 | 2026년 3월*

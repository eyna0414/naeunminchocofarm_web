# 나은민초코팜 웹
### 📌 프로젝트 개요

스마트팜을 효과적으로 운영하기 위해 만든 **IoT 기반의 농장 관리 시스템**

이 프로젝트는 그 중 React 기반으로, 관리자들이 웹 상에서 농장(Farm), 구역(Section), 센서(Sensor), 환경 센서값(습도·일조량 등)을 직접 **조회, 제어, 등록, 수정, 삭제**할 수 있도록 구현

---

### 🧭 프로젝트 목표

- 스마트팜의 핵심 요소(팜/구역/센서)의 **등록 및 상태 관리** UI 제공
- 실시간 환경 센서 데이터를 시각화하고, **자동 제어 기능**을 연결
- 관리자 전용 기능(서비스 신청 승인, 사용자 관리 등)을 통합한 웹 페이지 제공

---

### 🛠 기술 스택

| 분류 | 기술 |
| --- | --- |
| 프레임워크 | React (Vite 기반), JavaScript, Tailwind CSS |
| 상태 관리 | Redux Toolkit |
| API 통신 | Axios |
| 시각화 | Chart.js |
| 기타 | React Router, WebSocket, Dayjs |

---

### 🗂 주요 기능

### 1️⃣ 스마트팜 관리 (AdminFarmList)

- **Farm 목록 조회 / 등록 / 수정 / 삭제**
- UUID 자동 생성 후 연동
- 상태값(사용/미사용 등) 필터링
- **인라인 편집**으로 UX 개선 (텍스트, 날짜, select 등 동적 렌더링)

### 2️⃣ 구역 관리 (AdminFarmSection)

- 특정 Farm에 포함된 구역 리스트 조회
- 구역 이름, UUID 등록 및 수정
- **다중 선택 후 일괄 삭제 기능** 구현
- Section → Sensor 종속 구조 설계

### 3️⃣ 센서 관리 (AdminFarmSensor)

- Section에 소속된 센서 조회/등록/삭제
- 센서 UUID 자동 생성, 이름 및 타입 입력
- 센서 상태 확인 및 시각적 구분

### 4️⃣ 환경값 실시간 시각화

- **습도**, **일조량**, **토양수분**, **온도**, **이산화탄소** 등 센서값 표시
- Chart.js 기반 시간 변화 그래프 구현
- 실시간 WebSocket 데이터 연동을 통해 변화 감지 및 제어 연계

### 5️⃣ 자동/수동 제어 기능

- 센서값 기준으로 **급수 자동화**, **어닝 자동 개폐 기능** 연동
- 사용자가 직접 수동 전환 가능 (제어 카드 UI 구현)

### 6️⃣ 서비스 신청/회원 관리

- 관리자 페이지 내 서비스 신청 내역 관리 (승인/반려)
- 회원 목록 조회, 마이페이지 구성
- 사용자와 관리자 권한 분리 적용

---

### 💡 문제 해결 및 구조 개선 사례

### ✅ UUID 자동 등록 흐름 정립

- Farm/Section/Sensor 등록 시 UUID 테이블에 먼저 저장하고,
    
    해당 UUID ID를 참조하여 등록하는 구조로 MyBatis `<selectKey>` 사용
    
- 프론트에서도 이 흐름을 따라 등록 프로세스 분기

### ✅ 인라인 편집 UX 설계

- Farm 목록에서 항목별로 `수정` 버튼 클릭 시 각 항목을 input/date/select로 변경
- 수정 후 `저장` 버튼으로 변경되며 서버에 PUT 요청 수행

### ✅ 컴포넌트 모듈화

- Farm 기능 → 목록, 생성, 상세, Section, Sensor 컴포넌트로 분리
- 코드 재사용성과 유지보수성을 높임

### ✅ WebSocket 연동 구조 이해

- 센서값의 실시간 수신을 위해 ncf_socket_client.js 구성
- 환경 데이터 실시간 표시 및 급수 제어 기능과 연동

---

### 🧾 주요 디렉토리 구조
```
bash
복사편집
src/
├─ admin/components/farm/            ← 스마트팜(Farm) 기능
│  ├─ AdminFarmList.jsx              ← 목록 조회, 수정, 삭제
│  ├─ AdminFarmCreate.jsx            ← 팜 등록
│  ├─ AdminFarmDetail.jsx            ← 팜 상세
│  ├─ AdminFarmSection.jsx           ← 구역 관리
│  └─ AdminFarmSensor.jsx            ← 센서 관리
├─ humidity/components/              ← 습도 시각화
├─ sunshine/components/              ← 일조량 시각화
├─ smart_farm/components/            ← 센서 제어 카드 및 모니터링
├─ redux/                            ← 로그인 및 사용자 상태 관리
└─ routes/                           ← 라우터 및 권한 라우팅 설정

```

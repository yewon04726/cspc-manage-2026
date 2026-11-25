# CSPC 관리 사이트 (Frontend)

CSPC 학회의 **실습실 관리 시간표, 출결 기록, 컴퓨터(PC) 상태와 수리 요청**을 한곳에서 관리하는 웹 서비스의 프론트엔드입니다.

- 백엔드: [cspc-manage-server-2026](https://github.com/yewon04726/cspc-manage-server-2026)
- 배포: [TODO: Vercel 주소]

<img width="1457" height="805" alt="image" src="https://github.com/user-attachments/assets/3e327e76-606a-4b92-b52c-0d70cccc46a2" />
<img width="1461" height="802" alt="image" src="https://github.com/user-attachments/assets/062b366b-eddd-4024-b829-057d12b44401" />


---

## 왜 만들었나

학회원들이 시간을 나누어 실습실을 관리했지만 **시간표만 있고 관리가 이루어졌는지 확인할 방법이 없었습니다.**
또 주기적으로 컴퓨터 고장 여부를 점검해 전달해야 했는데, 컴퓨터 번호만 적힌 엑셀 파일로는 상태를 체계적으로 정리하기 어려웠습니다.

이 사이트는 두 문제를 해결합니다.

1. 주차별 관리 시간표와 실제 출결(관리 시작/끝) 기록을 함께 보여줍니다.
2. 실습실 PC 배치도에서 컴퓨터별 정보와 수리 요청을 기록합니다.

---

## 주요 화면

| 경로 | 화면 | 설명 |
| --- | --- | --- |
| `/` | 홈 | 주차별 관리 시간표, 지각 랭킹, 관리자 목록, 관리자 페이지·노션·실습실 바로가기 |
| `/admin` | 관리자 | 새 학기 설정, 학회원 관리, 출결 로그 조회 |
| `/d104` | D104 실습실 | PC 배치도, PC 클릭 시 정보 확인·수정과 요청 작성 |
| `/d105` | D105 실습실 | PC 배치도(전자교탁 포함), 요청 목록 |

### 홈
- 주차를 이동하며 **주간 관리 시간표** 확인
- 관리 담당 학회원의 **지각 랭킹**과 목록 표시

### 관리자
- **새 학기 설정**: 개강일·종강일을 입력하고, 월~일 1~6교시 기본 시간표를 표에서 바로 배정 (좌클릭 배정/해제, 우클릭으로 비우기)
- **학회원 관리**: 목록 확인, 추가, 삭제, 표시 색상 지정
- **출결 로그**: 학회원별로 관리 시작/끝 **예정 시각과 실제 기록**을 함께 비교

### 실습실 (D104, D105)
- PC 배치도에서 컴퓨터를 클릭하면 컴퓨터 번호, 제조사(제품명), 모델(머신타입), 시리얼 넘버 확인 및 수정
- 카테고리와 내용을 입력해 **PC 요청(수리 등)을 작성**하고 요청 목록으로 확인
- 서버에 등록되지 않은 좌석은 안내 표시

---

## 기술 스택

| 영역 | 구성 |
| --- | --- |
| UI | React 19, React Router 7 |
| 빌드 | Vite 7 |
| 스타일 | CSS Modules |
| 품질 | ESLint |
| 배포 | Vercel |
| 서버 | Node.js (Express), Supabase |

---

## 서버 연동

`/api/*` 요청은 개발 환경에서는 Vite proxy로, 배포 환경에서는 Vercel rewrites로 백엔드 서버에 전달됩니다.

| 화면 | 사용하는 API |
| --- | --- |
| 홈 | `GET /api/schedules/meta`, `GET /api/schedules/:week`, `GET /api/users`, `GET /api/users/ranking` |
| 관리자 | `/api/admin/setup-weeks`, `/api/admin/schedules/init`, `/api/admin/schedules/manager`, `/api/admin/users`, `/api/users/:id/logs` |
| 실습실 | `GET /api/lab?location=`, `GET /api/lab/:id`, `POST /api/lab/:id/reports`, `/api/lab/reports/:reportId` |

출석 체크인은 서버에서 **실습실 IP인지 확인**한 뒤에만 허용합니다.

---

## 프로젝트 구조

```text
src/
├─ App.jsx                 라우팅 (/, /admin, /d104, /d105)
├─ main.jsx                진입점
├─ pages/
│  ├─ home/                홈 (시간표, 랭킹)
│  ├─ admin/               관리자 (학기 설정, 학회원, 출결 로그)
│  ├─ D104.jsx             D104 실습실 PC 배치도
│  └─ D105.jsx             D105 실습실 PC 배치도
├─ components/layout/      TopBar, Panel
└─ styles/globals.css      전역 스타일
vercel.json                배포 시 API rewrite
vite.config.js             개발 시 API proxy
```

---

## 실행 방법

```bash
npm install
npm run dev       # 개발 서버
npm run build     # 빌드
npm run lint      # ESLint
npm run preview   # 빌드 결과 미리보기
```

### 환경 변수 (선택)

`.env`에 아래 값을 설정하면 API 서버 주소를 직접 지정할 수 있습니다. 값을 두지 않으면 `/api` 경로가 proxy(개발) 또는 rewrites(배포)를 통해 서버로 전달됩니다.

```env
VITE_API_BASE_URL=
```

---

## 내 역할

<!-- TODO: 팀 프로젝트라면 반드시 채우세요. -->

- 프로젝트 제안: 실습실 관리 문제를 정리해 학회원 2명에게 개선을 제안
- 담당: 백엔드 ([cspc-manage-server-2026](https://github.com/yewon04726/cspc-manage-server-2026))
- 프론트엔드와의 협업: [API 명세 협의, 화면 요구 전달 등 실제 한 일]

---

## 팀

<!-- TODO: 팀원과 역할 -->

원본 저장소: [CSPCLAB/cspc-manage-2026](https://github.com/CSPCLAB/cspc-manage-2026)

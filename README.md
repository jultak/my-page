# 클라우드컴퓨팅실습 개인과제 — 개인 소개 페이지 & 프론트엔드·백엔드 연동

## 프로젝트 소개
- 개인 소개 페이지(HTML/CSS/JS)와, Vite+React 프론트엔드 → FastAPI 백엔드 → Supabase DB로 이어지는 메모장 앱을 만들고 배포했습니다.
- 소개 페이지와 메모장 앱은 별도 페이지로 구성했고, 소개 페이지에서 메모장 앱으로 링크를 연결했습니다.

## 주요 구성

| 구분 | 내용 | 배포 플랫폼 |
|---|---|---|
| 개인 소개 페이지 | HTML/CSS/JS로 작성한 자기소개 페이지 | Vercel |
| 프론트엔드 | Vite + React로 만든 메모장 UI, 백엔드 API 호출 | Vercel |
| 백엔드 | FastAPI로 만든 메모 CRUD API | Render |
| 데이터베이스 | Supabase(PostgreSQL), Session Pooler로 연결 | Supabase |

## 배포 주소
- GitHub 저장소 (my-page): `여기에 URL`
- GitHub 저장소 (memo-frontend): `여기에 URL`
- GitHub 저장소 (memo-backend): `여기에 URL`
- Vercel 개인 소개 페이지: `여기에 URL`
- Vercel 메모장 앱: `여기에 URL`
- Render 백엔드 Swagger UI: `여기에 URL/docs`

## 핵심 개념 정리
- HTML(구조)·CSS(표현)·JS(동작)의 역할 분담
- Git·GitHub push의 기본 흐름 (add → commit → push)
- 프론트엔드-백엔드-DB 3계층 구조: 브라우저 요청이 Vercel(화면) → Render(API) → Supabase(저장)로 이어지는 흐름
- CORS(ALLOWED_ORIGINS)와 환경변수(VITE_API_URL, DATABASE_URL)의 역할
- 서버 재시작 시 메모리 저장 데이터가 사라지는 이유와, DB 연결로 해결되는 과정

## 막힌 점과 해결 과정
- Emmet으로 `!` + Tab은 되는데 그 이후 `ul>li*3` 같은 축약어는 Tab을 눌러도 확장이 안 됨 → 자동완성 팝업이 Tab을 가로채는 게 원인이었고, Esc로 팝업을 닫은 뒤 Tab을 누르니 정상 작동
- `<main class="cards"></main>`처럼 태그를 열자마자 바로 닫아버려서 그 안의 `<img>`·`<h2>`·`<p>`가 구조 밖으로 벗어나 스타일이 안 먹힘 → 닫는 태그를 마지막으로 옮겨서 해결
- 첫 GitHub push에서 사용자명/비밀번호를 입력해도 계속 비밀번호를 다시 물어봄 → 2021년부터 GitHub가 일반 비밀번호 인증을 막아서 생긴 문제였고, Personal Access Token을 발급받아 비밀번호 대신 입력해서 해결
- Live Server로 연 페이지에서 Cmd+Option+I를 눌러도 개발자 도구가 안 열림 → Safari는 개발자 도구가 기본으로 꺼져 있어서였고, Chrome으로 열어서 해결
- Elements 패널에서 `border-radius` 속성을 못 찾음 → `.cards`(컨테이너) 대신 `.card`(카드 하나)를 선택해야 하는데 컨테이너 쪽을 보고 있었음
- 가상환경을 새로 열 때마다 `fastapi: command not found` → `.venv`를 활성화하지 않고 명령어를 실행해서 발생, `source .venv/bin/activate` 후 정상 작동
- `fastapi dev main.py` 실행 시 `Path does not exist: main.py` → 터미널이 `memo-backend` 폴더가 아닌 상위 폴더에 있었음
- Swagger UI에서 `GET /memos`를 실행했는데 방금 추가한 메모가 안 보임 → 실제로는 "Try it out → Execute"를 안 누르고 문서에 적힌 예시 스키마만 보고 있었던 것
- Vercel에 `VITE_API_URL` 저장 시 "Remove the public framework prefix..." 경고가 뜨고 저장이 안 됨 → 변수 타입을 Secret으로 선택해서 생긴 문제, Secret은 한 번 저장하면 Config로 변경이 불가능해서 변수를 삭제하고 Config로 다시 생성해서 해결
- 로컬(`localhost`)에서 메모 추가가 실패함 → 로컬 백엔드 서버(uvicorn)를 꺼둔 상태였던 것이 원인
- 실제 배포 사이트(Vercel)에서 메모 추가가 처음엔 느리게 실패하는 듯 보임 → Render 무료 플랜의 콜드 스타트(첫 요청 시 30~60초 소요)였고, 잠시 후 정상 작동

## AI 활용 로그
- 실습 중 막히는 지점마다 Claude에게 에러 메시지나 스크린샷을 그대로 보여주고 원인을 물어봤습니다.
- 주로 물어본 내용: Emmet 미작동 원인, HTML 태그 구조 오류, Git push 인증 방식(토큰), 브라우저별 개발자 도구 차이, Vercel 환경변수 Secret/Config 차이, 가상환경 활성화 문제, Render 배포 로그 해석, Supabase 연결 문자열(Session Pooler) 설정, CORS·환경변수 흐름 정리
- AI가 제안한 해결책은 그대로 믿지 않고 직접 실행해서 확인했습니다 — 터미널 출력(`git log`, `pwd`, 서버 로그), 브라우저 개발자도구(Network 탭 상태 코드, Console 에러), Supabase Table Editor의 실제 데이터로 각각 검증했습니다.
- 워크북과 실제 화면이 다르게 보일 때(예: VS Code에 'Initialize Repository' 대신 'Clone Repository'만 보임)는 AI에게 스크린샷을 보여주고 워크북 문구와 실제 UI의 차이를 확인받았습니다.

## 자유 로그
- 오늘 배운 것, 아직 헷갈리는 것 등을 자유롭게 적어주세요.

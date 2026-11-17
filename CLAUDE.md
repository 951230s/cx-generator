# CX 문장 생성기

티머니GO·고속버스 티머니 고객 응대 문장을 팀 규칙에 맞춰 만드는 단일 HTML 페이지.
claude.ai 아티팩트로 배포되어 있고, 이 저장소의 파일이 원본이다.

- 배포 URL: https://claude.ai/artifact/7bwKKMH8JwLAwLZgprE5dd
- 원본 파일: `cx-generator/index.html` (외부 빌드 없음, 이 파일 하나가 전부)

## 파일 구조 (`cx-generator/index.html`)

| 위치 | 내용 |
|---|---|
| `<style>` | 디자인 토큰(`:root`, 다크 모드), 레이아웃 |
| `<form id="form">` | 모드·앱·스토어·유형 지정·어조·버전 개수 입력 |
| `<script type="text/plain" id="rules">` | **응대 규칙 본문 (1~11장)**. 대부분의 수정은 여기서 일어난다 |
| `PATHS` | 앱별 1:1문의·채팅상담 경로. 규칙 5장과 글자 그대로 일치해야 한다 |
| `BANNED`, `LIMITED` | 결과 카드의 자동 점검(금지어, 횟수 제한). 규칙 4장과 맞춘다 |
| `MODES`, `SAMPLE` | 모드별 라벨·지시문, 첫 화면 예시 카드 |
| `buildRules()` | 모드·앱·입력 내용에 따라 규칙 중 필요한 장만 잘라 프롬프트에 넣는다 |
| `buildPrompt()` | 최종 프롬프트 조립 |
| `checkVersion()` | 결과 자동 점검 |

## 수정할 때 지켜야 할 것

- 규칙의 장 제목은 `## N.` 형식을 유지한다. `buildRules()`가 이 번호로 장을 잘라낸다.
- 6장 유형 블록은 `**유형명**`으로 시작하고 빈 줄로 구분한다. `TAXI_BLOCK`, `BIKE_BLOCK`, `BLOCK_KEYS` 정규식이 이 제목에 기대고 있으므로 유형 이름을 바꾸면 정규식도 같이 고친다.
- 10장의 `### 자전거`, `### 택시` 절 제목도 고속버스 티머니에서 걸러낼 때 쓰인다.
- 경로를 바꾸면 규칙 5장과 `PATHS` 둘 다 고친다.
- 금지어를 추가하면 규칙 4장과 `BANNED` 둘 다 고친다.

## 배포

페이지는 `sample` 런타임 기능(`window.claude.use("sample")`)으로 Claude를 호출한다.
수정 후 Artifact 도구로 같은 URL에 다시 게시한다:

- `file_path`: `cx-generator/index.html`
- `url`: https://claude.ai/artifact/7bwKKMH8JwLAwLZgprE5dd
- `capabilities`는 생략한다 (기존 `{"sample":{}}` 선언이 유지됨)

claude.ai 쪽에서 직접 고친 버전이 있을 수 있으니, 게시 전에 Artifact `read`로 최신본을 받아 이 파일과 차이를 확인하고 합친다.

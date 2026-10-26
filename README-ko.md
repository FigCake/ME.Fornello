# Fornello

[Marinara Engine](https://github.com/Pasta-Devs/Marinara-Engine)에서 **HTML 페이지를 스크립트까지 실행**하는 확장. 마리나라 CSP가 막는 스크립트를 내장 해석기(sval)로 실행. 이름은 이탈리아어로 "화구". English: [README.md](README.md)

## 기능

- 채팅 속 ` ```html ` 코드 블록 → 실행되는 페이지. 말풍선 위에 테마 글자색·글꼴로 표시
- 캐릭터 카드의 화면 정규식이 만든 페이지도 답변 아래에서 실행. 페이지만 남는 답변은 글 숨김. 카드 전용 정규식은 마리나라 "캐릭터 정규식" 설정 없이도 그 캐릭터 채팅에 적용
- 리액트·JSX: `<script type="text/babel">`, CDN의 리액트 18
- **컴포넌트**: JSX 컴포넌트·HTML 템플릿. 답변 아래 상태창, 또는 AI가 넣은 자리에 장면. 카드에 넣어 배포 가능
- **페이지 명령어**: AI 호출, 메시지·변수·로어북·캐릭터·트래커
- **스크립트**: 채팅 뒤에서 도는 스크립트. 내 것·채팅 것·허락한 카드 것
- `localStorage`는 채팅별로 이 브라우저에 저장, `sessionStorage`는 새로고침까지
- [Dispensa](https://github.com/FigCake/ME.Dispensa) 🌟 보기 창의 페이지도 실행
- 화면 언어: English · 한국어(설정의 🌐. 기본은 마리나라 언어. Dispensa와 공유)

## 설치

1. 마리나라 서버 `.env`에 `ENABLE_EXTERNAL_EXTENSIONS=true` → 서버 재시작
2. **Settings → Addons → External Extensions** → **Allow third-party extension imports** 켜기 → `ME-Fornello-<버전>.zip` 가져오기
3. 목록의 **Fornello** → **Review and Run** → 코드 확인 후 해시 승인(`SHA256SUMS.txt`와 같아야 함)

[Fetchy](https://github.com/FigCake/ME.Fetchy) 사용 시 `FigCake/ME.Fornello`를 소스로 추가.

## 사용

**설정 열기**: 상단 바의 케이크 조각(Polyglot이 있으면 지구본) → **Fornello 설정** · **컴포넌트** · **스크립트 보관함**. 페이지 위 ⚙, Dispensa 창의 🔥 Fornello로도.

| 항목 | 내용 |
|---|---|
| 스크립트 실행 | **바로 실행**(기본. 최근 페이지가 화면에 보이면 자동) · **눌러서 실행** · **끄기**(컴포넌트·스크립트도 꺼짐) |
| 자동 실행할 최근 페이지 | 바로 실행일 때 자동으로 도는 페이지 수(기본 3) |
| 컴포넌트 | **+ 새 컴포넌트** · **불러오기** · **내보내기** |
| 펼쳐 둘 답변 수 | 컴포넌트를 펼쳐 둘 최근 답변 수 |
| 더 오래된 답변의 컴포넌트 | **접어 두기** · **숨기기** |

**페이지 위 버튼**(마우스를 올리면 오른쪽 위, 터치 화면은 항상): **▶ 실행** · ↻ 다시 실행 · 🖼 PNG 저장 · ⤢ 크게 보기(Esc로 복귀) · ⚙ 설정. 테마에서 안 보일 글자면 흰 바탕.

## 컴포넌트

1. **+ 새 컴포넌트**(또는 **불러오기**) → 종류 고르기
   - **JSX**: 컴포넌트 코드 붙여넣기. `import`·`export default` 자동 정리, 훅 사용 가능. 이름은 함수 이름
   - **HTML**: `{{값}}` 자리가 있는 템플릿 + 자체 `<style>`. 코드는 실행 안 됨(`<script>`·`on…` 제거, `<details>`·label 라디오는 동작). **이름**이 태그(대문자로 시작). `{{stats.hp}}`로 값 속 값, `{{#cast}}…{{/cast}}`는 목록 항목마다 반복(`{{name}}`은 항목 값, `{{.}}`은 항목 자체), 목록이 아니면 값이 있을 때 한 번. `{{^cast}}…{{/cast}}`는 비었을 때만. 값은 글자로 들어감. 값이 바뀌어도 펼친 `<details>`·체크 유지
2. **값 받는 곳**
   - **답변 속 태그**: AI가 태그 속성으로 쓴 값(`<Card hp={40} />`)으로 그림, 태그 글자는 숨김. **AI에게 줄 규칙**을 카드·로어북에 넣거나 **이 채팅 로어북에 넣기**. **AI가 쓰는 방식**: **상태창**(답변마다 끝에 태그 → 답변 아래) · **장면**(**언제 쓸지**에 맞을 때만, ```fornello 블록 → 그 자리)
   - **본문**: 정규식 카드처럼 태그 안에 글로 값. `<Tag>a|b|c</Tag>` → 값 표 순서대로. **나누는 기호** `|`·`:`(마지막 칸만 그 기호 포함 가능). **한 줄에 하나씩**: 한 줄이 `rows`의 항목 하나. 항목마다 `{{1}}`·`{{2}}`…(자리 순서), `{{text}}`(줄 전체), 첫 칸 이름의 표시(`{{#E}}…{{/E}}`) → 줄 종류마다 다른 모양. **예시 본문**은 미리보기·규칙용. 이 방식의 장면은 코드 블록 없이 태그 자리에 그림
   - **트래커**: 답변 당시의 마리나라 트래커로 그림, 트래커가 써지면 다시 그림. 같은 이름 필드는 자동, 아니면 경로(`date`, `custom.<항목>`, `char.<이름>.<스탯>`). **지시사항을 이 채팅 트래커에 → 넣기**: 값마다 지시사항을 사용자 트래커 항목에 넣어 Custom Tracker 에이전트가 읽게 함(에이전트가 켜져 있어야 함). 없는 항목은 빈 값으로 추가. 컴포넌트가 켜져 있는 동안 트래커가 다시 써지며 빠진 지시사항은 Fornello가 다시 넣음. **빼기**로 지움
3. **값**: 값마다 한 줄. 이름 · 종류(글자·숫자·참/거짓·목록·묶음) · 예시값 · 지시사항. JSX는 코드의 `function StatusCard({ hp = 80 })`대로, 여기서 고치면 코드도 바뀜. HTML은 템플릿이 쓰는 값이 저절로 들어오고, 이름을 바꾸면 템플릿도 바뀜. 값마다 **바꾸기**(선택, 한 줄에 `값=보일 글`)
4. **표시 이름**(접혔을 때) · **새 채팅에서**(고르지 않은 채팅에서 켜짐/꺼짐) · **글꼴**(마리나라에 넣은 글꼴)
5. 미리보기는 예시값으로 고칠 때마다 다시 그림(명령은 실행 안 됨) → **저장**

- **이 채팅에서 쓸 컴포넌트**(채팅을 연 상태의 설정): 컴포넌트·묶음마다 이 채팅용 스위치, 채팅에 저장. 꺼 둔 채팅에서도 태그 글자는 숨김
- **테마 클래스·아이콘**: `var(--fc-text)`, `--fc-muted`, `--fc-title`, `--fc-accent`, `--fc-card`, `--fc-line`, `--fc-radius`, 클래스 `fc-card`, `fc-title`, `fc-muted`, `fc-accent`, `fc-badge`, `fc-btn`, `fc-bar`(`<div class="fc-bar" style="--value:70%"><i></i></div>`). Font Awesome·Material Symbols·Material Icons는 클래스를 쓰면 자동으로 불러옴. 다른 CSS는 `<link rel="stylesheet">`
- **묶음**: 컴포넌트 그룹(**+ 묶음**). 묶음마다 **내보내기** · **고치기** · ✕
- **내보내기**(전체·묶음·하나) → `.json` 컴포넌트 파일. **불러오기**: 그 파일, 캐릭터 카드 PNG·JSON, `.jsx`·`.html` 파일. 가져올 것 고르기, 묶음으로 저장 가능. 같은 이름이면 교체(채팅별 선택 유지)
- 컴포넌트 위에 마우스: 이미지 저장 · 설정 · 접기. 안에서 페이지 명령어 사용 가능. 안의 클릭·입력은 저장 안 됨

### 캐릭터 카드에 넣기

- 카드 로어북에 컴포넌트마다 항목: 이름 `fornello:이름`, 내용은 JSX 코드, **꺼 두기**. 또는 카드 JSON의 `data.extensions.fornello.components: [{ "name": "…", "code": "…" }]`
- 규칙은 카드 로어북의 다른 항목에(항상 켜기, 또는 키워드). 편집기에서 복사
- **이 카드의 컴포넌트 허락**을 눌러야 그려짐(바뀌면 다시 물어봄). 설정의 **이 채팅의 카드에 든 컴포넌트**에서 **허락 취소**
- 이름이 같으면 내 컴포넌트 우선

## 스크립트

채팅 뒤에서 도는 스크립트. 설정 → **스크립트** → **보관함 열기**.

- **카드 스크립트**(캐릭터 카드에 든 것): 채팅을 열면 **실행** · **지금은 안 함** · **코드 보기** · **이 카드는 다시 묻지 않기**. 바뀌면 다시 물어봄. 지금은 안 함은 새로고침까지. 설정에서 **허락 취소**
- **내 스크립트**는 모든 채팅에서, **채팅 스크립트**(`setScripts([...], "chat")`)는 그 채팅과 갈래에서 실행(카드처럼 허락)
- **보관함 창**: 목록 세 개(내 것·카드마다·이 채팅). 폴더, 켜기, **⋯**(옮기기·다른 목록으로 복사·지우기), 끌어서 옮기기. **가져오기**: 스크립트 파일·zip, 캐릭터 카드, `.js`. **내보내기**: `.json` 스크립트 파일(스크립트 변수를 넣을지 물어봄)
- **편집기**: 줄 번호, 쓰는 동안 문법 오류, 버튼, 설명, 변수(JSON), **기록**(console 출력·오류)과 **다시 시작**
- 채팅을 열면 시작, 다른 채팅을 열면 멈춤. 타이머·리스너·옵저버, `<body>`·`<head>` 바로 아래에 남긴 요소도 같이 정리. **끄기**면 꺼짐
- 페이지 명령어 전부 + 아래 명령

| 명령어 | 기능 |
|---|---|
| `setButtons(["Hello", "Roll"])` | 버튼: Dispensa Dock의 🔥 거품 안, 없으면 상단 메뉴 |
| `onButton("Hello", fn)` | 그 버튼을 누르면 실행 |
| `getScripts("global" \| "char" \| "chat")` / `await setScripts([...], scope)` | 목록 읽기 / 통째로 바꾸기(스크립트 `{ name, code, enabled, buttons, info, data }`, 폴더 `{ type: "folder", name, scripts }`). 페이지에서도 사용(한 번 확인) |

```js
setButtons(["Roll"]);
onButton("Roll", () => showToast(`🎲 ${1 + Math.floor(Math.random() * 6)}`));
onMsg((m) => setVars({ replies: (getVars("script").replies ?? 0) + 1 }, "script"));
```

- 그 밖에: `getScriptId()`, `getScriptName()`, `getAllEnabledScriptButtons()`(실행 중인 스크립트들의 버튼)
- ES 모듈 가능(`import x from "https://…"`, 이름만 쓰면 jsDelivr, 맨 위 `await`)
- jQuery `$`, lodash `_`, zod `z`, `YAML`은 스크립트·페이지가 쓰면 jsDelivr에서 받음
- 아직 안 됨: MVU(MagVarUpdate)

## 페이지 명령어

메시지 번호는 1번부터, 음수는 끝에서부터(`-1` = 마지막). 페이지가 같은 이름을 만들면 페이지 쪽 우선.

| 명령어 | 기능 |
|---|---|
| `sendMsg("text")` | 내 메시지로 보내고 답변 받기(둘째 인자 `true`: 먼저 확인) |
| `sendRegen()` | 마지막 답변 다시 쓰기. `sendRegen("shorter")`로 지침 추가 |
| `await genChat("prompt")` | 채팅 전체를 알고 AI가 답함. 채팅에 안 남음 |
| `await genChat("prompt", { history: 10 })` | 저렴한 버전: 카드·페르소나·상시 로어북·최근 메시지 10개만 |
| `await genRaw("prompt")` / `genRaw([{ role, content, images? }, …])` | 요청만, 또는 내가 정한 순서의 메시지 |
| 옵션 | `connection`(저장한 연결), `onToken: (text, piece) => …`, `id`. `genChat`: `guide`, `preset`. `genRaw`: `images`, `temperature`, `maxTokens`, `topP`, `topK`, `minP`, `frequencyPenalty`, `presencePenalty`, `reasoningEffort` |
| `stopGen(id)` / `stopMyGens()` | 페이지가 부른 AI 하나 / 전부 멈추기 |
| `getVars()` / `setVars({ name: value })` | 채팅 변수(채팅 안에 저장). `setVars`는 안쪽까지 합침, `null`은 지우기, 목록은 통째로 |
| `getVars(종류)` / `setVars({ … }, 종류)` | `"global"`, `"character"`(카드 값에서 시작), `"script"`: Fornello 저장소 |
| `await getMsgs({ role, hidden })` | 메시지: `num`, `role`, `name`, `text`, `hidden`, `data`, `swipe`, `swipes` |
| `await getLastMsg("user" \| "char")` / `getMsgId()` | 마지막 말 / 이 페이지의 메시지 번호 |
| `await setMsg(num, "text")` / `delMsg(num)` | 메시지 고치기 / 지우기(확인) |
| `await addMsg("text", "char")` | 생성 없이 메시지 넣기(`"user"`, `"char"`, `"narrator"`. `name`, `hidden`, `data`, `before`). 번호를 돌려줌 |
| `getMsgData(num)` / `await setMsgData(num, { … })` | 스와이프마다 붙는 데이터(AI에겐 안 보임. 합침, `{ replace: true }`) |
| `await getSwipes(num)` / `setSwipe(num, i)` / `addSwipe(num, "text")` / `delSwipe(num, i)` | 스와이프(0번부터). `addSwipe`는 `{ show: false }` 아니면 바로 보임 |
| `await hideMsg(num)` / `showMsg(num)` | AI에게 숨기기 / 다시 보이기(하나 또는 목록) |
| `await branch(num)` | `num`번까지로 새 채팅. id를 돌려줌 |
| `stopGen()` | 생성 중인 답변 멈추기 |
| `getInput()` / `setInput("text")` | 입력칸 읽기 / 채우기 |
| `getUser()` / `getChar()` | 내 페르소나 / 채팅 캐릭터(`name`, `description`, `personality`, `scenario`, `firstMessage`, `greetings`, `example`, `tags`, `books`, `data`) |
| `await getChar("name")` / `getUser("name")`, `getChars()` / `getUsers()` | 라이브러리의 다른 캐릭터·페르소나 / 이름 목록 |
| `await setChar(name, { … })` / `setUser(…)`, `addChar` / `addUser`, `delChar` / `delUser` | 고치기(합침, 한 번 확인) · 만들기 · 지우기(항상 확인) |
| `await importChar(file)` / `useUser("name")` | 카드 가져오기(PNG `Blob`·JSON) / 이 채팅 페르소나 바꾸기 |
| `await getTracker()` / `setTracker({ "custom.mood": "happy" })` | 마리나라 트래커 읽기 / 고치기 |
| `onTracker((tracker, changed) => …)` | 트래커가 바뀌면 실행. `changed`는 바뀐 경로 |
| `await runTrackers()` / `getAgents()` / `runAgents(["type"])` | 트래커 다시 돌리기 / 에이전트 목록 / 에이전트 다시 돌리기(AI 호출) |
| `await getAgentMemory(type)` / `setAgentMemory(type, { … })` / `clearAgentMemory(type)` | 에이전트의 이 채팅 기억 |
| `await genImage("prompt")` | 채팅의 이미지 연결로 그림. 주소를 돌려줌 |
| `await speak("text")` / `stopSpeaking()` | 마리나라 음성 설정으로 읽기 |
| `await getSummary()` / `makeSummary({ last })` / `setSummary([{ title, text }])` | 채팅 요약 |
| `await getLore()` / `getLore("book")` | 책 이름 / 책 항목. `"@chat"` = 이 채팅 전용 책 |
| `await setLore("book", { name, content, keys, constant, … })` | 이름으로 항목 추가·수정(`{ replace: true }`: 딱 이것들로) |
| `await delLore("book", "name")` | 항목 지우기(같이 쓰는 책이거나 여러 개면 확인) |
| `getBooks()` / `await linkBook("book", 어디)` / `unlinkBook(…)` | 쓰는 책 / `"chat"`, `"global"`, `"char"` / `{ char }`, `"persona"` / `{ persona }`에 연결(채팅 밖이면 확인) |
| `await delBook("book")` / `importBook(json)` | 책 지우기(확인) / 로어북 가져오기(JSON) |
| `getRegex(범위)` / `await setRegex([...], 범위)` / `delRegex("name")` | 마리나라 정규식. 범위 `{ char: "current" }`, `{ preset: "in_use" }`, `{ global: true }`(고치기 전 확인) |
| `applyRegex(text, { on, mode, depth })` | 이 채팅 정규식을 적용한 글 |
| `await runSlash("/command")` | Dispensa가 있으면 슬래시 명령 전부, 없으면 `/send`, `/trigger`, `/gen`, `/genraw`, `/sendas`, `/sys`, `/regen`, `/stop`, `/echo`, `/setinput` |
| `onMsg` / `onSend` / `onSwipe` / `onEdit` / `onDelete` | 새 답변 / 내 메시지 / 스와이프 / 글 수정 / 삭제 때 실행 |
| `onBeforeGen(gen => …)` | 답을 쓰기 직전(최대 5초 기다림): `gen.kind`, `gen.num`, `await gen.inject(text, { depth, role })` |
| `onGenDone(fn)` / `onGenError(fn)` | 답 완료 / 실패·멈춤 |
| `await inject("text", { id, depth, role, position, once })` | 다음 답들의 프롬프트에 글. `remove()`, 페이지 닫힘, 한 번 뒤(`once`)에 빠짐 |
| `on("name", fn)` / `emit("name", …values)` | 페이지·스크립트끼리 이벤트 |
| `showToast("text")` | 작은 알림 |
| `share("name", value)` / `await getShared("name")` | 다른 페이지·스크립트에 값 건네기 / 기다려 받기 |
| `catchErrors(fn)` / `reload()` / `getVersion()` | 오류를 알림으로 / 다시 시작 / Fornello 버전 |

## 안전

페이지는 마리나라 안에서 동작. 마음먹은 스크립트는 마리나라 데이터에 접근 가능. 실수 방지용 울타리일 뿐 보안 경계는 아님.

- 모르는 사람의 카드: **눌러서 실행** 권장
- 카드의 컴포넌트·스크립트는 허락해야 실행
- `localStorage`는 채팅별 페이지 저장소. 마리나라 주소로 가는 `fetch` 차단
- 먼저 확인: 메시지 지우기 · 같이 쓰는 로어북 항목 지우기 · 1분 안에 AI 12번 넘게 호출(`runSlash`·`triggerSlash`도 셈)

## 알아 둘 점

- 해석기라 브라우저보다 느림. 탭·모달·퀴즈는 괜찮고 무거운 애니메이션은 느릴 수 있음
- 외부 스크립트(`<script src="https://…">`)도 받아 실행. 큰 라이브러리는 느리거나 안 될 수 있음
- CDN 리액트(18 이하)는 마리나라 리액트로 대체(제 속도). `ReactDOM.render`, `<script type="module">` 가능
- 4초 넘게 쉬지 않는 반복문은 오류로 중지(반복문만 감시)
- `<script>`마다 따로 실행. 맨 위 `var`는 `window.이름`으로 못 읽음
- 이미지 저장은 외부 그림·글꼴이 빠질 수 있음


## 라이선스와 출처

- Fornello: [GNU AGPL-3.0](LICENSE). Copyright (C) 2026 FigCake
- 마리나라 엔진(AGPL-3.0)의 API와 동작에 맞춰 제작
- 실리태번 호환을 위해 [JS-Slash-Runner](https://github.com/N0VI028/JS-Slash-Runner)(PolyForm Noncommercial 1.0.0)의 함수 이름과 사용법을 따름. JS-Slash-Runner 코드는 없음
- 포함한 오픈소스: [sval](https://github.com/Siubaak/sval)(MIT, JavaScript 해석기), [acorn](https://github.com/acornjs/acorn)(MIT), [Sucrase](https://github.com/alangpierce/sucrase)(MIT, JSX 변환)와 lines-and-columns(MIT), [html-to-image](https://github.com/bubkoo/html-to-image)(MIT, 이미지 저장), [Lucide](https://lucide.dev) 아이콘(ISC). 라이선스 전문은 `THIRD_PARTY_NOTICES.txt`와 `extension.js` 끝에
- 전체 소스: 배포 저장소의 `ME-Fornello-<버전>-source.zip`. 고쳐서 배포하거나 서비스로 제공하면 고친 소스도 AGPL-3.0으로 공개

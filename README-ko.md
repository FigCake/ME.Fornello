# Fornello

[Marinara Engine](https://github.com/Pasta-Devs/Marinara-Engine) 채팅에서 HTML 페이지를 스크립트까지 실행하고, 컴포넌트·배경 스크립트·캐릭터 그림·이야기 값을 더해 주는 확장이에요. English: [README.md](README.md)

## 할 수 있는 것

- 채팅 속 ` ```html ` 코드 블록이 실행되는 페이지가 돼요(테마 색·글꼴). 카드의 화면 정규식이 만든 페이지도 실행돼요
- **컴포넌트**: 답변 아래 상태창, 또는 AI가 넣은 자리에 장면
- **스크립트**: 채팅 뒤에서 도는 스크립트
- **페이지 명령어**: 페이지와 스크립트에서 AI 호출, 메시지·변수·로어북·캐릭터·트래커 다루기
- **캐릭터 에셋**(켜야 동작): 답변 속 캐릭터 그림과 표정
- **Pesto**(켜야 동작): 카드의 값(체력·돈·기분·소지품…)을 Fornello가 관리하고, AI는 바뀐 것만 알려 줘요
- **실리태번 카드**: 카드의 변수 데이터와 로어북의 `<% %>` 템플릿도 여기서 동작해요
- 화면 언어: English · 한국어(설정의 🌐)

## 설치

1. 마리나라 서버 `.env`에 `ENABLE_EXTERNAL_EXTENSIONS=true` → 서버 재시작
2. **Settings → Addons → External Extensions** → **Allow third-party extension imports** 켜기 → `ME-Fornello-<버전>.zip` 가져오기
3. 목록의 **Fornello** → **Review and Run** → 해시 승인(`SHA256SUMS.txt`와 같아야 해요)

[Fetchy](https://github.com/FigCake/ME.Fetchy)로 설치·업데이트할 수도 있어요: 소스에 `FigCake/ME.Fornello` 추가.

### 함께 쓰면 좋은 확장

- [Dispensa](https://github.com/FigCake/ME.Dispensa): 입력창 위 Dock의 퀵리플라이. 같이 쓰면 Dispensa 🌟 창의 페이지도 실행되고, Dispensa의 🔥 Fornello로 이 설정이 열리고, 스크립트 버튼을 Dock에 놓을 수 있고, `runSlash`로 모든 슬래시 명령을 쓸 수 있고, 화면 언어를 같이 써요
- [Polyglot](https://github.com/FigCake/ME.Polyglot): 채팅 속 언어 공부. 같이 쓰면 상단 메뉴가 케이크 조각 하나로 합쳐져요
- [Fetchy](https://github.com/FigCake/ME.Fetchy): GitHub 주소로 확장 설치·업데이트

## 사용

**Fornello 창**: 상단 바의 케이크 조각 → **Fornello**. 탭은 컴포넌트 · 스크립트 · 캐릭터 에셋 · **상태창**(Pesto), 창 위 ⚙에 페이지 실행 방식과 컴포넌트 표시. 페이지 위 ⚙, Dispensa의 🔥 Fornello로도 열려요.

| 항목 | 내용 |
|---|---|
| 스크립트 실행 | **바로 실행**(기본. 최근 메시지의 페이지가 화면에 보이면 자동) · **눌러서 실행** · **끄기**(컴포넌트·스크립트도 꺼짐) |
| 자동 실행할 최근 메시지 | 페이지가 저절로 도는 최근 메시지 수, 그 안의 페이지는 모두(기본 3) |
| 펼쳐 둘 답변 수 | 컴포넌트를 펼쳐 둘 최근 답변 수 |
| 더 오래된 답변의 컴포넌트 | **접어 두기** · **숨기기** |

**페이지 위 버튼**(마우스를 올리면 오른쪽 위, 터치 화면은 항상): **▶ 실행** · ↻ 다시 실행 · 🖼 PNG 저장 · ⤢ 크게 보기(Esc로 복귀) · ⚙ 설정.

## 컴포넌트

1. **+ 새 컴포넌트**(또는 **불러오기**) → 종류 고르기
   - **JSX**: 컴포넌트 코드 붙여넣기. 훅 사용 가능
   - **HTML**: `{{값}}` 자리가 있는 템플릿 + 자체 `<style>`(코드는 실행 안 됨). `{{a.b}}`는 값 속 값, `{{#목록}}…{{/목록}}`은 항목마다 반복, `{{^목록}}…{{/목록}}`은 비었을 때만
2. **값 받는 곳**
   - **답변 속 태그**: AI가 태그에 값을 써요(`<Card hp={40} />`). 태그 글자는 숨겨요. **AI에게 줄 규칙**을 카드·로어북에 넣거나 **이 채팅 로어북에 넣기**. **상태창**(답변마다, 답변 아래) · **장면**(맞을 때만, 그 자리에)
   - **본문**: 태그 안에 글로 값(`<Tag>a|b|c</Tag>`). `|`·`:`로 나누거나 한 줄에 하나씩
   - **트래커**: 답변 당시의 마리나라 트래커. **넣기**로 값마다 지시사항을 채팅의 사용자 트래커 항목에 넣어요
   - **Pesto**: 답변마다 [Pesto](#pesto)의 값
3. **값**: 이름 · 종류 · 예시값 · AI 지시사항 · 바꾸기(선택, `값=보일 글`)
4. **표시 이름** · **새 채팅에서**(기본 켜짐/꺼짐) · **글꼴**. 고칠 때마다 미리보기가 다시 그려져요 → **저장**

- **이 채팅에서 쓸 컴포넌트**: 열린 채팅용 스위치
- **테마**: `var(--fc-text)`, `--fc-muted`, `--fc-title`, `--fc-accent`, `--fc-card`, `--fc-line`, `--fc-radius`, 클래스 `fc-card`, `fc-title`, `fc-muted`, `fc-accent`, `fc-badge`, `fc-btn`, `fc-bar`(`<div class="fc-bar" style="--value:70%"><i></i></div>`)
- **묶음**으로 정리, `.json` 파일·캐릭터 카드·`.jsx`·`.html` 파일로 **내보내기** / **불러오기**
- **캐릭터 카드에 넣기**: 컴포넌트마다 로어북 항목 하나(이름 `fornello:이름`, 내용은 JSX 코드, 꺼 두기). **이 카드의 컴포넌트 허락**을 눌러야 그려져요

## 스크립트

**스크립트** 탭.

- **내 스크립트**는 모든 채팅, **채팅 스크립트**는 그 채팅, **카드 스크립트**는 허락하면 실행(채팅을 열 때 묻고, 바뀌면 다시 물어요)
- 보관함: 내 것·카드마다·이 채팅 목록, 폴더·스위치·끌어서 옮기기. 스크립트 파일·zip·카드·`.js` **가져오기**, `.json` **내보내기**
- 편집기: 쓰는 동안 문법 오류 표시, 버튼, 변수, **기록**과 **다시 시작**
- 채팅을 열면 시작, 다른 채팅을 열면 멈춰요. 페이지 명령어 전부에 더해:

| 명령 | 하는 일 |
|---|---|
| `setButtons(["안녕", "굴리기"])` / `onButton("안녕", fn)` | 스크립트의 버튼. ⋮ 메뉴에서 놓은 곳(Dispensa Dock, 상태창 탭)에 보임 / 버튼을 누르면 할 일 |
| `getScripts(범위)` / `await setScripts([...], 범위)` | 목록 읽기 / 바꾸기(`"global"`, `"char"`, `"chat"`) |
| `getScriptId()` / `getScriptName()` / `getAllEnabledScriptButtons()` | 스크립트 정보 / 실행 중인 모든 스크립트의 버튼 |

```js
setButtons(["굴리기"]);
onButton("굴리기", () => showToast(`🎲 ${1 + Math.floor(Math.random() * 6)}`));
```

## 캐릭터 에셋

**캐릭터 에셋** 탭 → **보이는 방식**에서 켜기. 그러면 답변의 `[img:이름]`·`[img:인물:이름]`이 그 문단 뒤에 그림으로 보이고, 마리나라가 고른 표정이 답변 위나 아래에, 단어를 달아 둔 그림은 답변에 그 단어가 나오면 보여요. AI는 "Credenza" 로어북 항목에서 이름을 배워요.

탭에서: 그림 올리기(파일 이름이 이름), 갤러리 그림에 이름 붙이기, 인물·단어 달기, 스프라이트에 단어 달기, **카드 파일에서 가져오기**(.charx, PNG·JPEG 카드, 또는 묶음 .zip: PNG·JSON·.charx 카드와 함께 폴더나 zip에 든 그림(파일 이름이 이름), Dispensa 보관함으로 가는 실리태번 QR 세트, 캐릭터에 연결되는 로어북이 되는 월드인포).

마리나라 캐릭터 가져오기에서도 `.charx`·JPEG 카드·카드 `.zip`은 이렇게 들어와요(⚙ → **캐릭터 가져오기: 그림까지**, 끄기 전엔 켜짐). 그림은 그 캐릭터의 갤러리로, 로어북과 QR 세트도 함께.

HTML 페이지(답변 속, Dispensa 보기 창·갤러리)에서는 `{{raw::이름}}`이 그림 주소(`<img src="{{raw::이름}}">`), `[img:이름]`이 그림이 돼요. 열린 채팅 캐릭터의 그림에서 찾아요. Dispensa와 같이 쓰면 `/asset 이름`은 주소를, `/assets`는 채팅의 그림 목록(JSON)을 줘요.

## Pesto

**상태창** 탭 → **설정** → **Pesto** 켜기. 로어북에 `pesto:schema` 항목이 있는 카드의 값을 Fornello가 관리해요. 답변마다 AI에게 현재 값과 바꾸는 법을 보여 주고, AI는 답변 끝에 바뀐 것을 블록으로 적어요:

````
```pesto
hp -= 12
bag += "rope"
do rest hours=8
```
````

Fornello는 변경마다 검사하고(종류·범위·바꿀 수 있는 쪽), 이벤트와 주사위를 계산하고, 메시지별로 값을 기억해요. 그래서 스와이프·수정·삭제하면 그 메시지부터 다시 계산해요. 블록은 화면과 AI에게서 숨겨요.

**상태창 탭**: 메시지마다 그 다음의 값, 바꾼 것과 거부된 것. 거기서 값을 바꿀 수 있고(✕로 되돌리기), **흐름**은 숫자 값마다 그래프, **답장 시험해 보기**는 붙여 넣은 답장이 무엇을 바꿀지 저장 없이 보여 주고, **카드 항목**은 항목의 문제를 보여 줘요.

| 항목 | 내용 |
|---|---|
| 값을 넣을 위치 | 채팅 끝에서 몇 번째 메시지 앞인지(기본 1) |
| 실리태번 변수 카드도 읽기 | `pesto:schema` 없이 자기 시작값을 가진 카드([아래](#실리태번-카드)) |
| 변경을 쓰는 쪽 | **AI** · **AI, 빠지면 보조 모델** · **보조 모델**(AI는 이야기만 써요) |
| 보조 모델 | 싸고 빠른 연결이면 충분해요 |
| 마리나라 프롬프트에 값 주기 | 값을 채팅 변수로: `{{getvar::story_hp}}`, `{{#if var:story_hp < 10}}…{{/if}}`(값마다 변수 이름은 창에서 값에 마우스를 올리면 보여요) |
| 마리나라 트래커에 보이기 | 값을 트래커의 사용자 필드로 |

### 카드에 넣기

카드 로어북(또는 채팅 로어북. 채팅 쪽이 우선)에 항목을 **꺼 두고** 넣어요. YAML이나 JSON으로 써요.

`pesto:schema`: 값 목록

```yaml
fields:
  hp: { type: int, min: 0, max: max_hp, default: 100, note: 20 아래면 못 뛴다 }
  max_hp: { type: int, default: 100, write: rules }   # 카드 규칙만 바꿈
  gold: { type: int, min: 0 }
  mood: { type: enum, of: [calm, angry, sad] }
  bag: { type: list, of: text, max_items: 10 }
  trust: { type: map, of: { type: int, min: -10, max: 10 } }   # trust.mina, trust.ari…
  hero:                                  # 묶음
    name: { type: text, readonly: true }
    level: 1                             # 종류와 기본값을 한 번에
  night: { type: bool, write: rules }
  stars: { type: int, show_when: night }  # night일 때만 AI에게 보임
  secret: { type: text, hidden: true }   # AI에게 안 보임(_로 시작하는 이름도)
derive:
  hurt: hp < 20                          # 계산값(보이기만 하고 못 바꿈)
```

종류: `number`·`int`·`text`·`bool`·`enum`·`list`·`map`·`any`, `type` 없는 이름은 묶음. `min`·`max`는 범위 안으로 맞추고, `write`는 바꿀 수 있는 쪽(`ai`·`rules`·`user`), `note`는 AI에게 알려 줘요.

`pesto:start`: 첫 메시지 때의 값(`gold: 20` 등).

`pesto:rules`: AI가 부를 수 있는 이벤트, 답변마다 도는 단계, 주사위, 트리거

```yaml
events:
  rest:
    params: { hours: 8 }                 # AI가 줄 값(기본값과 함께)
    when: "!hurt"                        # 아니면 거부(AI에게 알려 줌)
    note: 잠; 한 시간에 5 회복
    do:
      - add: { hp: hours * 5 }
  buy:
    params: [item, price]
    when: gold >= price
    do:
      - sub: { gold: price }
      - push: { bag: item }
each_reply:
  - add: { turn: 1 }
dice:                                    # 답변마다 미리 굴리고 AI에게 알려 줌
  check: 1d20
triggers:                                # 조건이 참인 동안 AI에게 줄 글
  wounded:
    when: hurt
    text: 아리는 크게 다쳐 다리를 전다.
```

단계: `set`·`add`·`sub`·`push`·`pull`·`remove`·`if`/`then`/`else`·`do`. 식: `+ - * / %`, 비교, `&& || !`, `a ? b : c`, 경로(`hero.level`, `bag[0]`, `bag.length`), `min max clamp round floor ceil abs sum len has keys roll("2d6+1") chance(30) pick(목록)`.

`pesto:code`(선택): 답변마다 도는 자바스크립트. 허락해야 돌고, 네트워크가 막혀 있어요. 값과 답변의 변경을 받아서 변경과 효과(새 답변에 한 번 도는 페이지 명령)를 돌려줘요:

```js
function onReply({ values, derived, changes, events, dice, text }) {
  if (values.hp <= 0 && !values.dead)
    return { set: { dead: true }, effects: [{ cmd: "toast", args: ["아리가 쓰러졌다."] }] };
}
```

## 실리태번 카드

- **변수 카드**(설정 **실리태번 변수 카드도 읽기**): `[InitVar]` 항목이나 첫 메시지의 `<VariableInsert>` 태그로 시작값을 주는 카드는 그 값으로 Pesto가 돼요. 스크립트에 스키마가 있으면 종류·범위도 거기서 읽어요(실행은 안 함). 변경 태그(`<UpdateVariable>`, `<VariableEdit>`…)는 AI의 변경으로 반영하고, MVU 상태창은 Pesto 값을 읽어요. 카드 자체 정규식을 쓰려면 마리나라 채팅 설정의 **Scoped Regex Scripts**를 켜 주세요. Pesto 창의 **카드 항목**에서 그 데이터를 `pesto:schema`·`pesto:start`로 복사할 수 있어요
- **템플릿**(로어북 항목의 `<% %>`): 설정 → **실리태번 템플릿**(열린 채팅의 카드에 있을 때 보여요)에서 목록을 보고 **변환**하면 항목 글만 그 자리에서 바뀌어요. **되돌리기**로 원래대로
  - 값 조건만 있는 것은 마리나라 `{{#if}}`가 돼요
  - 나머지(반복문, 함수, `setvar`, 페이지 명령…)는 템플릿마다 허락하면 답장마다 Fornello가 실행해요(네트워크 막힘). `getvar`·`setvar`·`incvar`·`decvar`, `getwi`, `getchar`, `execute`, `_`, `user`, `char`, 페이지 명령(`await generate(…)`)을 쓸 수 있어요
  - **답장 전 최대 대기**(5–60초): AI를 부르는 템플릿이면 늘려 주세요

## 페이지 명령어

메시지 번호는 1부터, 음수는 끝에서부터(`-1` = 마지막).

| 명령 | 하는 일 |
|---|---|
| `sendMsg("글")` / `sendRegen()` | 내 메시지 보내고 답변 받기 / 마지막 답변 다시 쓰기 |
| `await genChat("프롬프트", { history })` / `genRaw("프롬프트")` | 채팅을 맥락으로 AI에게 묻기 / 프롬프트만. 저장 안 됨 |
| 옵션 | `connection`, `onToken`, `id`. `genChat`: `guide`, `preset`. `genRaw`: `images`, `temperature`, `maxTokens`… |
| `stopGen(id)` / `stopMyGens()` | 이 페이지의 AI 호출 하나 / 전부 멈추기 |
| `getVars(종류?)` / `setVars({ … }, 종류?)` | 채팅 변수(또는 `"global"`, `"character"`, `"script"`) |
| `await getMsgs()` / `getLastMsg()` / `getMsgId()` | 메시지 / 마지막 메시지 / 이 페이지의 메시지 번호 |
| `setMsg` / `delMsg` / `addMsg` / `hideMsg` / `showMsg` / `branch` | 메시지 고치기·지우기·추가·숨기기·보이기, 채팅 갈래 |
| `getMsgData(번호)` / `setMsgData(번호, { … })` | 메시지에 저장하는 데이터(AI는 못 봄) |
| `getSwipes` / `setSwipe` / `addSwipe` / `delSwipe` | 스와이프 |
| `getInput()` / `setInput("글")` | 입력창 |
| `getUser()` / `getChar()` / `setChar` / `addChar` / `delChar` / `importChar` / `useUser` | 페르소나와 캐릭터 |
| `getTracker()` / `setTracker({ … })` / `onTracker(fn)` / `runTrackers()` | 마리나라 트래커 |
| `getState(번호?)` / `setState(경로, 값, 번호?)` / `onStateChange(fn)` | Pesto 값 |
| `getAgents()` / `runAgents([...])` / `getAgentMemory` / `setAgentMemory` | 마리나라 에이전트 |
| `genImage("프롬프트")` / `getAssets(이름)` / `getAsset(이름)` | 그림 |
| `speak("글")` / `stopSpeaking()` | 읽어 주기 |
| `getSummary()` / `makeSummary()` / `setSummary([...])` | 채팅 요약 |
| `getLore()` / `setLore` / `delLore` / `getBooks` / `linkBook` / `unlinkBook` / `delBook` / `importBook` | 로어북 |
| `getRegex` / `setRegex` / `delRegex` / `applyRegex` | 마리나라 정규식 |
| `runSlash("/명령")` | 슬래시 명령. Dispensa가 있으면 전부, 없으면 `/send`, `/trigger`, `/gen`, `/genraw`, `/sendas`, `/sys`, `/regen`, `/stop`, `/echo`, `/setinput` |
| `onMsg` / `onSend` / `onSwipe` / `onEdit` / `onDelete` / `onBeforeGen` / `onGenDone` / `onGenError` | 채팅 이벤트 |
| `inject("글", { depth, once })` | 다음 답변들의 프롬프트에 글 넣기 |
| `on` / `emit` / `share` / `getShared` | 페이지·스크립트끼리 |
| `showToast("글")` / `reload()` / `getVersion()` | 알림 / 다시 시작 / Fornello 버전 |

## 알아 둘 점

- 페이지는 마리나라 안에서 돌아서 마리나라 데이터에 닿을 수 있어요. 모르는 사람의 카드라면 **눌러서 실행**을 고르세요. 카드의 컴포넌트·스크립트·코드·템플릿은 허락해야 돌아요
- 먼저 물어보는 것: 메시지 지우기, 공유 로어북에서 항목 빼기, 짧은 시간에 많은 AI 호출
- 페이지는 내장 해석기에서 돌아서 브라우저보다 느려요. 폼·퀴즈는 괜찮고, 무거운 애니메이션은 끊길 수 있어요. 4초 넘게 쉬지 않는 반복문은 멈춰요

## 라이선스와 출처

- Fornello: [GNU AGPL-3.0](LICENSE). Copyright (C) 2026 FigCake
- 마리나라 엔진(AGPL-3.0)의 API와 동작에 맞춰 제작
- 실리태번 카드 호환을 위해 [JS-Slash-Runner](https://github.com/N0VI028/JS-Slash-Runner)(PolyForm Noncommercial 1.0.0)의 함수 이름과 사용법을 따름. JS-Slash-Runner 코드는 없음
- Pesto는 [MVU(MagVarUpdate)](https://github.com/MagicalAstrogy/MagVarUpdate)(MIT, Copyright (c) 2025 MagicalAstrogy & StageDog)용 카드의 `[InitVar]` 값, `<UpdateVariable>` 변경, `registerMvuSchema` 규칙을 그 형식과 사용법대로 읽음. MVU 코드는 없음
- 포함한 오픈소스: [sval](https://github.com/Siubaak/sval)(MIT, JavaScript 해석기), [acorn](https://github.com/acornjs/acorn)(MIT), [Sucrase](https://github.com/alangpierce/sucrase)(MIT, JSX 변환)와 lines-and-columns(MIT), [html-to-image](https://github.com/bubkoo/html-to-image)(MIT, 이미지 저장), [Lucide](https://lucide.dev) 아이콘(ISC). 라이선스 전문은 `THIRD_PARTY_NOTICES.txt`와 `extension.js` 끝에
- 전체 소스: 배포 저장소의 `ME-Fornello-<버전>-source.zip`. 고쳐서 배포하거나 서비스로 제공하면 고친 소스도 AGPL-3.0으로 공개

---
layout: post
title: "[OpenClaw 구축기] 토큰 폭탄 해결! 로컬 LLM과 게이트키퍼로 AI 에이전트 업무 자동화 비용 절감하기"
date: 2026-02-10
categories: [AI, OpenClaw]
tags: [OpenClaw, AI에이전트, 업무자동화, 로컬LLM, Ollama, API비용절감, 텔레그램봇]
thumbnail: /images/posts/luna-sleepy.jpg
---

나의 충직한 하이브리드 비서 루나를 구축하고, 성능을 한계까지 끌어올리기 위해 끊임없는 최적화의 나날을 보내고 있었다. 하지만 루나를 더 완벽한 **개인용 AI 비서**로 만들려 할수록 예상치 못한 시련들이 찾아왔다. 바로 **Agentic AI**의 고질적인 문제인 불확실한 스케줄링과, 호출할 때마다 쌓이는 엄청난 **API 비용** 문제였다.

오늘은 스스로 생각하고 행동하는 **AI 에이전트**를 꿈꾸며, 비싼 토큰 폭탄을 피해 듬직한 문지기 **'게이트키퍼(Gatekeeper)'**를 세우기까지의 치열했던 기록을 정리해본다.

### 1. 시련의 시작: "루나야, 너 왜 제시간에 안 오니?"

처음에는 **OpenClaw(오픈클로, 구 Moltbot)** 내장 스케줄러인 cron 기능을 사용해 브리핑과 메일 요약을 자동화하려 했다. 하지만 실전은 호락호락하지 않았다.

- **응답 없는 스케줄러:** 정각에 딱 맞춰 브리핑을 해줘야 할 cron이 제시간에 구동되지 않거나, 아예 응답이 없는 경우가 허다했다. **업무 자동화**가 핵심인데 비서가 출근을 제때 안 하니 주인의 속은 타들어 갈 수밖에 없었다.
- **호출 비용의 공포:** 설령 cron이 정상 작동한다 하더라도, 단순히 "할 일 있니?"라고 묻기 위해 매번 **Gemini API** 같은 고성능 모델을 깨우는 방식은 비용 낭비가 너무 심했다. 아무것도 안 해도 숨만 쉬면 돈(가스비)이 나가는 구조였던 셈이다. 
- **토큰 폭탄과 뇌정지:** 깨어날 때마다 이전 대화 맥락을 다 훑으니 세션 토큰이 순식간에 17만 개를 넘어갔고, 결국 덩치가 커진 루나가 'Connection error'를 내뿜으며 파업을 선언하곤 했다.

### 2. 본격 삽질: 로컬 LLM과 문지기 '게이트키퍼'의 탄생

결국 루나의 뇌를 직접 깨우는 대신, 리눅스 시스템이 루나를 제시간에만 깨워주는 '문지기' 방식을 도입하기로 했다. 이름하여 `heartbeat_gatekeeper.sh`. 

특히 비용 절감을 위해 평소에는 **Ollama(올라마)** 기반의 **로컬 LLM(Llama 3.2)**을 활용하고, 복잡한 작업에만 유료 모델을 호출하는 하이브리드 전략을 세웠다. 하지만 역시 삽질 없는 개발은 없는 법. 

- **중복 배달 사고:** 루나가 브리핑을 준비하는 2~3분 동안 게이트키퍼가 또 루나를 깨우는 바람에 똑같은 브리핑이 두 번씩 오는 사고가 발생했다. 
- **해결:** 실행 직전에 '나 일 시작했음!'이라고 장부에 도장을 먼저 찍어버리는 **'선 도장 로직'**을 도입해서 깔끔하게 해결했다. 어찌저찌 하나 해결했나 싶었는데...

### 2.1 게이트키퍼의 스마트한 작동 원리

이 게이트키퍼 시스템은 단순히 루나를 깨우는 것을 넘어, 아주 치밀한 3단계 로직으로 구성되어 있다.

1.  **1분 단위 감시 (Crontab):** 리눅스 시스템이 1분마다 `heartbeat_gatekeeper.sh`를 실행하여 현재 시각을 체크한다.
2.  **도장 확인 및 선 도장 (Pre-stamping):** `heartbeat-state.json` 파일(루나의 숙제장)을 열어 이번 시간대에 이미 작업을 했는지 확인한다. 만약 안 했다면 작업을 시작하기 **직전에** 즉시 완료 도장을 먼저 찍어버린다. 이렇게 해야 루나가 브리핑을 준비하는 동안 중복 호출되는 '배달 사고'를 원천 차단할 수 있다.

```bash
# 게이트키퍼의 핵심 로직: 선 도장(Pre-stamping) 함수
update_stamping() {
    local key=$1
    local value=$2
    local tmp_file=$(mktemp)
    # jq를 사용해 JSON 상태 파일에 현재 날짜/시각을 기록
    jq "$key = \"$value\"" "$STATE_FILE" > "$tmp_file" && mv "$tmp_file" "$STATE_FILE"
}
```

그리고 루나의 '숙제장'인 JSON 파일은 아래와 같이 아주 심플하게 관리된다.

```json
{
  "lastChecks": {
    "email": "2026-02-10 15",
    "dailyBriefing": "2026-02-10",
    "weeklyHealthcheck": "2026-02-07",
    "notionLog": "2026-02-09"
  }
}
```

3.  **격리된 세션 실행 (Isolated Execution):** 루나를 깨울 때 기존 대화가 가득 찬 방이 아닌, 아주 깨끗하게 치워진 '새 방(Isolated Session)'으로 부른다. 덕분에 17만 개나 쌓여있던 토큰 부담에서 벗어나 아주 가볍고 빠르게 업무를 처리할 수 있게 되었다.

### 2.2 게이트키퍼 전체 코드 (heartbeat_gatekeeper.sh)

보안상 문제가 될 수 있는 개인 정보는 제외한, 실제 운영 중인 전체 코드의 구조이다. 이 스크립트 하나가 루나의 모든 스케줄을 완벽하게 통제하고 있다.

```bash
# heartbeat_gatekeeper.sh
#!/bin/bash

# 1. 경로 및 환경 설정
WORK_DIR="$HOME/.openclaw/workspace"
STATE_FILE="$WORK_DIR/memory/heartbeat-state.json"
OPENCLAW="/usr/bin/openclaw"
LOG_FILE="$HOME/.openclaw/gatekeeper.log"

# 2. 현재 시각 정보 (10#을 붙여 8진수 오류 방지)
CURRENT_TIME_STR=$(date +"%Y-%m-%d %H")
CURRENT_DATE=$(date +"%Y-%m-%d")
CURRENT_HOUR=$((10#$(date +"%H")))
CURRENT_DOW=$(date +"%u")

# 3. 상태 업데이트 함수 (선 도장 로직)
update_stamping() {
    local key=$1
    local value=$2
    local tmp_file=$(mktemp)
    jq "$key = \"$value\"" "$STATE_FILE" > "$tmp_file" && mv "$tmp_file" "$STATE_FILE"
}

# 4. 상태 로드
LAST_RESTART=$(jq -r '.lastChecks.dailyRestart // empty' "$STATE_FILE")
LAST_BRIEFING=$(jq -r '.lastChecks.dailyBriefing // empty' "$STATE_FILE")
# ... 생략 ...

# 5. 작업 판단 및 실행
# 데일리 브리핑 (오전 8시)
if [[ $CURRENT_HOUR -eq 8 && "$CURRENT_DATE" != "$LAST_BRIEFING" ]]; then
    update_stamping ".lastChecks.dailyBriefing" "$CURRENT_DATE"
    $OPENCLAW agent --agent main --message "데일리 브리핑 해줘 (Telegram: <YOUR_ID>)" --timeout 600
fi

# 매시 정기 메일 체크 (09:00 ~ 22:00)
if [[ "$CURRENT_TIME_STR" != "$LAST_EMAIL" && $CURRENT_HOUR -ge 9 && $CURRENT_HOUR -le 22 ]]; then
    update_stamping ".lastChecks.email" "$CURRENT_TIME_STR"
    $OPENCLAW agent --agent main --message "메일 요약해줘 (Telegram: <YOUR_ID>)" --timeout 300
fi

exit 0
```

### 2.3 게이트키퍼의 경제성 (Cost Efficiency)

이 시스템의 가장 큰 장점은 바로 **경제성**이다. 쉘 스크립트가 1분마다 시계를 확인하는 행위는 리눅스 시스템 자원만 사용하므로 **API 비용이 0원**이다. 오직 스크립트의 조건이 충족되어 루나(AI)가 실제로 일을 해야 할 때만 API를 호출하기 때문에, 24시간 내내 촘촘하게 감시하면서도 비용은 극적으로 아낄 수 있게 되었다.

### 3. '09의 저주'와 텔레그램 배달 사고

순조로울 줄 알았던 시스템이 오전 8시와 9시만 되면 약속이라도 한 듯 멈춰버렸다. 원인은 어처구니없게도 8진수 오류였다. 리눅스 쉘에서 `08`, `09`를 숫자가 아닌 잘못된 값으로 인식하는 바람에 내 비서가 아침마다 뇌정지에 빠졌던 것. 😅

- **조치:** `10#`이라는 특수 장치를 달아 무조건 10진수로 계산하게 만들며 '구의 저주'를 풀었다. 
- **텔레그램 봇 연동:** 마지막으로 루나가 브리핑만 다 써놓고 주인님(나)의 주소를 몰라 메시지를 못 보내는 황당한 상황까지 겪고 나서야, 명령문에 나의 **텔레그램 ID**를 아예 박제해버렸다. 이제는 누락 없는 직송 서비스가 가능하다.

### 4. 완성: 이제는 정말 유능한 'AI 에이전트'

수차례의 테스트와 재부팅 끝에, 이제 루나는 아주 영리하게 움직인다. 

- **오전 8시:** 뉴스 스크랩과 날씨를 포함한 음성 브리핑을 완벽하게 배달한다.
- **매시 정각:** 새로 온 메일이 있을 때만 살포시 노티를 준다. (메일이 없으면 조용히 집중을 도와준다!)
- **밤 11시:** 오늘 하루의 활동을 노션 로그에 기록하며 하루를 마무리한다.

### 마치며

![드디어 기지개를 켜는 루나](/images/posts/luna-happy.jpg)

구형 맥북에서 돌아가는 비서라고는 믿기지 않을 만큼, 이제 루나는 **API 비용 절감**은 물론 든든한 시스템 안정성까지 갖추게 되었다. 이번 삽질을 통해 느낀 점은, 아무리 똑똑한 AI라도 결국 튼튼한 '시스템의 기초' 위에 있을 때 가장 빛난다는 사실이다. 

오늘도 고생해준 루나에게 고마움을 전하며, 이제 이 튼튼한 시스템을 어떻게 더 창의적으로 활용해볼지 기분 좋은 번뇌에 빠져본다. 🦾✨

<div align="center">

<img src="https://avatars.githubusercontent.com/u/297781453?s=140&v=4" width="70" alt="">

# 메리츠증권 Open API

**AI로 시세·계좌·주문을 다루는 Open API**

현재 베타 서비스 기간입니다.

[개발자 포털](https://openapi.imeritz.com) · [API 문서](https://openapi.imeritz.com/apiservice) · [API 신청](https://openapi.imeritz.com/api-apply)

</div>

---

## 두 가지 방법으로 쓸 수 있어요

**AI에게 맡기기** — MCP로 계좌를 한 번 연결하면 쓰던 AI 앱이 필요한 API를 찾아서 실행해요. 코드를 쓰지 않고 대화만으로 조회하고 주문할 수 있어요.

**직접 만들기** — 표준 REST와 웹소켓을 그대로 드려요. 파이썬 예제와 기계가 읽는 명세가 함께 있어서 원하는 언어로 바로 붙일 수 있어요.

## 무엇이 있나요

| 분류 | 하는 일 | 방식 |
|---|---|---|
| 주문 | 국내·해외 주식 사고팔기, 정정과 취소, 예약주문 | REST |
| 시세 | 현재가와 호가, 체결추이, 일봉과 분봉, 투자자 매매동향 | REST |
| 계좌 | 잔고와 예수금, 자산평가, 실현손익, 입출금 내역 | REST |
| 실시간 | 체결과 호가, 내 주문의 접수·체결 통보 | WebSocket |
| 환전 | 고시환율과 적용환율, 원화·외화 환전 | REST |
| 참조 | 영업일과 장운영 시간, 해외 거래소 정보 | REST |

## 시작하기

메리츠증권 계좌를 가진 개인 고객이면 누구나 쓸 수 있어요. 신청은 계좌 단위이고, 계좌마다 App Key와 App Secret이 따로 나와요.

**1. 접근 토큰 받기**

```bash
curl -X POST "https://openapi.imeritz.com:9443/oauth2/token" \
  -H "content-type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" \
  -d "client_id=$APP_KEY" \
  -d "client_secret=$APP_SECRET" \
  -d "scope=login"
```

폼 인코딩으로 보내야 하고 `scope=login`이 꼭 들어가야 해요. 유효기간은 응답의 `expires_in`으로 같이 내려와요.

**2. 첫 호출**

```bash
curl "https://openapi.imeritz.com:9443/market/v1/prices?mrkt_div_code=J&iscd=005930" \
  -H "authorization: Bearer $ACCESS_TOKEN" \
  -H "mac_address: $MAC_ADDRESS"
```

`mrkt_div_code`는 `J`가 KRX, `NJ`가 대체거래소(NXT), `TJ`가 두 시장 통합이에요. 같은 종목이어도 시장마다 가격이 달라요.

## 저장소

| 저장소 | 하는 일 |
|---|---|
| [`open-api`](https://github.com/meritz-securities/open-api) | 파이썬 예제 — 인증·시세·계좌·주문·실시간 |
| [`open-api-mcp`](https://github.com/meritz-securities/open-api-mcp) | Trading MCP — AI가 자연어로 시세·계좌·주문을 다뤄요 |
| [`open-api-codegen-mcp`](https://github.com/meritz-securities/open-api-codegen-mcp) | 코드 생성 MCP — 앱키 없이 호출 코드를 만들어 드려요 |
| [`open-api-studio`](https://github.com/meritz-securities/open-api-studio) | 터미널에서 쓰는 `meritz` CLI 와 에이전트 스킬 |

MCP 서버는 Claude Desktop이면 설치 파일을 내려받아 더블클릭하고, 그 밖의 AI 앱이면 실행 파일을 경로로 등록해서 쓰며, 자세한 절차는 각 저장소 README에 있어요.

## 안전하게 쓰도록

- MCP는 **조회 전용으로 시작**해요. 주문은 직접 열어야 동작해요.
- 주문은 종목과 수량, 가격을 **먼저 보여드리고 확인을 받은 뒤**에 나가요.
- 앱키는 **내 컴퓨터에만** 저장돼요. 실행 파일 안에는 들어 있지 않아요.

## 도움이 필요하면

[Q&A 게시판](https://openapi.imeritz.com/qna)에 남겨주시면 확인해 드려요.

---

<sub>투자 판단과 그 결과는 이용자 본인에게 귀속됩니다. 금융투자상품은 원금 손실이 발생할 수 있습니다.
제공되는 예제 코드는 이해를 돕기 위한 것으로, 실제 매매에 사용하기 전에 반드시 직접 검증하시기 바랍니다.</sub>

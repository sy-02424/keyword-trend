# 키워드 관심도 비교 대시보드

캠페인 키워드나 브랜드 이름을 최대 5개까지 넣으면, 기간 동안 관심도가 어떻게 달라졌는지 한 그래프로 비교해 주는 마케터용 대시보드입니다.

- v1: 영어 위키백과 조회수(Wikimedia Pageviews API, 키 없음)
- v2 예정: 네이버 데이터랩 API로 교체

> 위키 조회수는 정보를 찾아본 관심도이며 검색량이 아닙니다. 하루 단위로 집계되므로 실시간 데이터가 아닙니다.

## 기능

1. 키워드 1개를 넣고 [비교하기]를 누르면 최근 90일 선 그래프가 보이고, 세로축 최대가 100이다. (#1)
2. 키워드 5개가 색깔별 선으로 한 그래프에 보이고, 6번째 입력은 "최대 5개까지" 안내와 함께 막힌다. (#2)
3. 30일 / 90일 / 1년 / 3년 중 고르면 그래프가 그 기간으로 바뀌고(1년 이상은 월 단위), "가장 관심 높은 키워드 · 최고치 날짜"가 그래프 위에 보인다. (#3)

## API 문서

**Wikimedia Pageviews API** (영어 위키백과 문서별 조회수)

| ① 기본 주소 | ② 메뉴 | ③ 옵션 | ④ 응답 모양 | ⑤ 키 | ⑥ 규칙 |
|---|---|---|---|---|---|
| `https://wikimedia.org/api/rest_v1` | `/metrics/pageviews/per-article` (문서별 조회수, GET) | 주소 뒤에 순서대로 붙임:<br>`/{프로젝트}/{접속}/{사용자}/{문서제목}/{단위}/{시작일}/{종료일}`<br>· 프로젝트: **`en.wikipedia`**<br>· 접속: **`all-access`** / `desktop` / `mobile-web` / `mobile-app`<br>· 사용자: **`user`**(사람) / `spider` / `automated` / `all-agents`<br>· 문서제목: 예 `Kimchi` (띄어쓰기는 `_`)<br>· 단위: **`daily`** / `monthly`<br>· 시작일·종료일: `YYYYMMDD`<br>(굵은 글씨 = 이 서비스가 쓰는 값) | JSON `{ "items": [ … ] }`<br>항목 하나 = 하루:<br>`timestamp`(예 `2026101900`), `views`(그날 조회수), `article`, `project`, `access`, `agent`, `granularity`<br>· 조회가 0인 날은 목록에서 빠짐<br>· 없는 문서는 `404` | 필요 없음 | · **데이터 라이선스: CC0 1.0** (퍼블릭 도메인, 자유 이용)<br>· **User-Agent 표기 요청:** 모든 요청에 앱 이름·연락처를 알리는 User-Agent를 보내야 하고, 없으면 예고 없이 차단될 수 있음. 브라우저 JavaScript는 User-Agent를 바꿀 수 없어 `Api-User-Agent` 헤더를 대신 씀<br>· **User-Agent 한계(v1):** 브라우저에서 `Api-User-Agent`를 붙이면 사전 요청이 `405`로 거절돼 호출이 실패하므로 v1은 브라우저 기본 User-Agent만 보냄 → v2에서 서버를 거쳐 부를 때 앱 이름·연락처를 넣음<br>· **요청 예절:** 한꺼번에 너무 많이 보내지 않기 (정확한 한도는 공식 Rate limits 문서 기준)<br>· **데이터 한계:** 하루 단위 집계라 실시간이 아님, 오늘 데이터는 아직 없음<br>· **출처 표기:** 화면에 "Data: Wikimedia Pageviews API"와 기준 기간을 적음 |

예시: [Kimchi의 2026-09-01 ~ 2026-10-20 일별 조회수](https://wikimedia.org/api/rest_v1/metrics/pageviews/per-article/en.wikipedia/all-access/user/Kimchi/daily/20260901/20261020)

참고: [Wikimedia APIs 접근 정책](https://www.mediawiki.org/wiki/Wikimedia_APIs/Access_policy) · [Analytics API 접근 정책](https://doc.wikimedia.org/generated-data-platform/aqs/analytics-api/documentation/access-policy.html)

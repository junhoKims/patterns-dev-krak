# Title

왜 `rspress`를 사용해서 SSG를 구현하는지

## Status

actived

## Context

  - rspress, vitepress, docusaurus 등의 대안이 많음
  - docusaurus는 확장성은 높지만 러닝커브가 크고 초기비용이 큼
  - vitepress는 대중적이고, 문서 프리셋과 디자인 모두 제공 이미 사용해보았음
  - rspress는 문서프리셋 디자인 제공, 새로운 경험, rust의 빠른 속도, mdx지원. 대신 중국쪽에서 개발된다.

### 참고

  - https://rspress.dev/guide/start/introduction#why-rspress
  - (사실 vitepress도 그렇고 대부분의 문서작성을 위한 SSGTools은 이런 기능은 기본적 탑재)

## Decision

  `rspress`로 결정

  ### 사용적 측면
  
  - mdx를 지원해서 jsx등의 표현에 있어서 유리하다
  - 플러그인 지원은 다른 SSG에서도 작동하지만 검색 등이 기본적으로 들어가있다.
  - 로고가 취향이다

  ### 기술적 측면

  - rust기반, 본인들이 만든 rsbuild 기반으로 빌드 및 실행속도가 매우 빠름
  - 설정 자체에서 biome 지원 (prettier대비 빠른 속도 및 설정 쉬움)

  ### 리스크

  - 중국쪽 툴이라 언어 관련 불안함이 존재

## Consequences

  - 진행중

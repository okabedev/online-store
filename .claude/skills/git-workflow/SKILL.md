---
name: git-workflow
description: 이 프로젝트의 Git Flow 브랜치 전략과 커밋 컨벤션(subject/body 한글). 커밋, 브랜치 생성, 머지, 릴리스, 핫픽스, 태그 작업을 할 때 반드시 사용한다.
---

# Git Workflow

## 1. 브랜치 전략 (Git Flow)

| 브랜치 | 분기 원본 | 머지 대상 | 이름 규칙 | 용도 |
|---|---|---|---|---|
| `main` | - | - | `main` | 배포된 버전만 존재. 릴리스마다 태그 `vX.Y.Z` |
| `develop` | `main` | - | `develop` | 다음 릴리스를 위한 통합 브랜치 |
| `feature` | `develop` | `develop` | `feature/<이슈번호>-<짧은-설명>` | 기능 개발 |
| `release` | `develop` | `main`, `develop` | `release/vX.Y.Z` | 릴리스 준비(버전 갱신, 버그 수정만) |
| `hotfix` | `main` | `main`, `develop` | `hotfix/vX.Y.Z` | 운영 긴급 수정 |

- 브랜치 설명은 kebab-case 영문으로 쓴다. 이슈 번호가 없으면 생략한다.
  - 예: `feature/12-order-create-api`, `feature/product-search`
- 머지는 항상 `--no-ff`로 하여 머지 커밋을 남긴다.
- 버전은 SemVer(`MAJOR.MINOR.PATCH`)를 따른다.

## 2. 커밋 메시지 컨벤션

```
<type>(<scope>): <subject>

<body>

<footer>
```

### type (영어 소문자)
| type | 의미 |
|---|---|
| `feat` | 새로운 기능 |
| `fix` | 버그 수정 |
| `docs` | 문서 변경 |
| `style` | 포맷팅, 세미콜론 등 동작 변화 없는 변경 |
| `refactor` | 동작 변화 없는 코드 구조 개선 |
| `test` | 테스트 추가·수정 |
| `perf` | 성능 개선 |
| `build` | 빌드 시스템, 의존성 변경 |
| `ci` | CI 설정 변경 |
| `chore` | 그 밖의 잡무(설정 파일 등) |
| `revert` | 이전 커밋 되돌리기 |

### scope
- 선택 사항. 변경된 모듈·도메인 이름을 영어 소문자로 쓴다. 예: `order`, `product`, `cart`, `auth`, `member`

### subject (한글)
- 50자 이내, 마침표 없음.
- 명사형/개조식으로 끝낸다: "~ 추가", "~ 수정", "~ 제거", "~ 개선".
- 무엇을 했는지 구체적으로 쓴다. ("수정", "작업" 같은 모호한 표현 금지)

### body (한글)
- subject와 빈 줄 하나로 구분한다.
- **무엇을, 왜** 변경했는지 쓴다. 어떻게는 코드가 말해준다.
- 한 줄은 72자 내외로 줄바꿈한다. 여러 항목은 `-` 목록으로 쓴다.
- 변경이 사소해 subject만으로 충분하면 생략할 수 있다.

### footer
- 이슈 연결: `Closes #12`, `Refs #34`
- 호환성 깨짐: `BREAKING CHANGE: <설명>`

### 예시
```
feat(order): 주문 생성 API 추가

- 장바구니 상품으로 주문을 생성하는 POST /api/orders 추가
- 재고가 부족한 상품이 있으면 주문 전체를 거부하도록 검증

Closes #12
```
```
fix(cart): 수량 0 이하 입력 시 예외 처리 누락 수정
```
```
refactor(product): 상품 조회 로직을 서비스 계층으로 이동
```

## 3. 작업 절차

### 기능 개발
0. 작업할 Issue를 확인하거나 새로 만든다. (`git-issue` skill)

```bash
git switch develop
git pull                                   # 원격이 있는 경우
git switch -c feature/12-order-create-api
# ... 작은 단위로 작업 → 테스트/빌드 통과 확인 → 사용자 승인 후 커밋 반복
git switch develop
git merge --no-ff feature/12-order-create-api
git branch -d feature/12-order-create-api
```
- 원격 저장소가 있으면 develop에 직접 머지하지 않고 PR을 만들어 머지한다. PR은 `git-pr` skill의 규칙을 따른다.

### 릴리스
```bash
git switch -c release/v1.2.0 develop
# 버전 정보 갱신, 릴리스 관련 버그 수정만 커밋
git switch main && git merge --no-ff release/v1.2.0
git tag -a v1.2.0 -m "v1.2.0 릴리스"
git switch develop && git merge --no-ff release/v1.2.0
git branch -d release/v1.2.0
```

### 핫픽스
```bash
git switch -c hotfix/v1.2.1 main
# 수정 커밋
git switch main && git merge --no-ff hotfix/v1.2.1
git tag -a v1.2.1 -m "v1.2.1 핫픽스"
git switch develop && git merge --no-ff hotfix/v1.2.1
git branch -d hotfix/v1.2.1
```

> 원격 저장소가 있으면 `main`/`develop`은 브랜치 보호로 직접 push가 막혀 있다. 위의 로컬 머지 대신 release/hotfix 브랜치를 push하고 `main`, `develop` 각각으로 PR을 만들어 머지한다(`git-pr` skill). 태그는 `main` 머지 후 `main`을 pull 받아 생성하고 `git push origin <태그>`로 올린다.

## 4. 커밋 전 체크리스트
- [ ] 현재 브랜치가 `main`/`develop`이 아닌가?
- [ ] 이 커밋은 하나의 목적만 담고 있는가?
- [ ] 테스트·빌드·린트를 통과했는가?
- [ ] 비밀값, 디버그 코드, 불필요한 파일이 포함되지 않았는가? (`git diff --staged`로 확인)
- [ ] 메시지가 위 컨벤션을 따르는가?
- [ ] 스테이징할 파일 목록과 커밋 메시지 초안을 사용자에게 보여주고 승인을 받았는가?

## 5. 금지 사항
- `main`/`develop`에 직접 커밋
- `main`/`develop`에 force push
- 여러 목적을 섞은 커밋 (예: 기능 추가 + 무관한 리팩터링)
- 사용자 승인 없는 커밋
- 사용자 승인 없는 `push`, 태그 push, 브랜치 삭제(원격)

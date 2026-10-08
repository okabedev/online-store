---
name: git-issue
description: 이 프로젝트의 GitHub Issue 규칙. Issue 생성·수정·코멘트·라벨 변경 작업을 할 때 반드시 사용한다.
---

# Issue Workflow

- 모든 작업은 Issue에서 시작한다. (아주 사소한 오타 수정 등은 예외)
- Issue 하나가 PR 하나로 끝날 수 있는 크기로 만든다. 크면 쪼갠다.
- Issue가 생기면 `git-workflow` skill에 따라 `feature/<이슈번호>-<짧은-설명>` 브랜치를 만들어 작업한다.

## 1. 제목
`[타입] 한글 요약`

| 타입 | 용도 |
|---|---|
| `Feat` | 새로운 기능 |
| `Fix` | 버그 수정 |
| `Refactor` | 동작 변화 없는 구조 개선 |
| `Docs` | 문서 |
| `Test` | 테스트 |
| `Chore` | 설정, 빌드, 의존성 등 기타 작업 |

예: `[Feat] 주문 생성 API 구현`, `[Fix] 장바구니 수량 음수 입력 허용 문제`

## 2. 본문
`.github/ISSUE_TEMPLATE/`의 템플릿을 따른다.
- 기능 요청(`feature.md`): 배경·목적, 할 일, 완료 조건
- 버그 리포트(`bug.md`): 현상, 재현 방법, 기대 동작, 실제 동작, 환경
- 일반 작업(`task.md`): 목적, 할 일, 완료 조건

**완료 조건(Acceptance Criteria)은 반드시 검증 가능한 문장으로 쓴다.**

## 3. 라벨
- 타입 라벨 1개 필수: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`
- 필요 시 우선순위: `priority: high`, `priority: medium`, `priority: low`

## 4. Claude 행동 규칙
- Issue **생성·수정·코멘트·라벨 변경·닫기 전에는 항상 제목과 본문 초안을 보여주고 사용자 승인**을 받는다. 외부에 게시되는 작업이므로 이전 승인은 다음 작업에 적용되지 않는다.
- `gh` CLI를 사용한다. 본문은 임시 파일로 작성해 `--body-file`로 넘긴다.
  ```bash
  gh issue create --title "[Feat] 주문 생성 API 구현" --label feat --body-file <본문 파일>
  ```
- 원격 저장소가 없거나 `gh` 인증이 안 되어 있으면 Issue 단계는 건너뛰고, 그 사실을 사용자에게 알린다.

---
name: git-pr
description: 이 프로젝트의 GitHub Pull Request 규칙. PR 생성·수정·리뷰·코멘트·머지 작업을 할 때 반드시 사용한다.
---

# Pull Request Workflow

브랜치·커밋 규칙은 `git-workflow` skill을 따른다.

## 1. base 브랜치
| 작업 브랜치 | base |
|---|---|
| `feature/*` | `develop` |
| `release/*` | `main` (머지 후 `develop` 역머지 PR) |
| `hotfix/*` | `main` (머지 후 `develop` 역머지 PR) |

## 2. 제목
커밋 컨벤션과 동일: `<type>(<scope>): <subject>` (subject는 한글)
예: `feat(order): 주문 생성 API 추가`

## 3. 본문
`.github/pull_request_template.md`를 따른다. 반드시 `Closes #<이슈번호>`로 관련 Issue를 연결한다.

## 4. 크기
- 하나의 목적만 담는다.
- 리뷰 가능한 크기를 유지한다. (변경 400줄 이하 권장, 넘으면 쪼갤 수 있는지 먼저 검토)

## 5. 머지 전 조건
- [ ] 테스트·빌드·린트 통과
- [ ] 셀프 리뷰 완료 (필요 시 `/code-review` 실행)
- [ ] 리뷰 코멘트 모두 해결
- [ ] 관련 문서 갱신

## 6. 머지
- 방식: **Merge commit** (`git-workflow`의 `--no-ff` 규칙과 일치)
- 머지 후 작업 브랜치를 삭제한다.

## 7. Claude 행동 규칙
- PR **생성·수정·코멘트·머지 전에는 항상 제목과 본문 초안(머지는 대상 PR과 방식)을 보여주고 사용자 승인**을 받는다. 외부에 게시되는 작업이므로 이전 승인은 다음 작업에 적용되지 않는다.
- PR 생성 전 브랜치 push가 필요하면 그것도 함께 승인받는다.
- `gh` CLI를 사용한다. 본문은 임시 파일로 작성해 `--body-file`로 넘긴다.
  ```bash
  gh pr create --base develop --title "feat(order): 주문 생성 API 추가" --body-file <본문 파일>
  gh pr merge <번호> --merge --delete-branch
  ```
- 원격 저장소가 없거나 `gh` 인증이 안 되어 있으면 PR 단계는 건너뛰고, 그 사실을 사용자에게 알린다.

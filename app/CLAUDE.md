# app/CLAUDE.md

이 디렉터리는 독립 Gradle 프로젝트다. 저장소 공통 규칙은 루트 `CLAUDE.md`를 따른다.

## 빌드
- 모든 Gradle 명령은 `app/`에서 Wrapper로 실행한다: `cd app && ./gradlew <task>`. 시스템 `gradle` 명령은 쓰지 않는다.
- 빌드 스크립트는 Kotlin DSL(`*.gradle.kts`)로 작성한다.
- 요구 JDK: 25.
- `gradle.properties`에서 configuration cache가 켜져 있다. 빌드 스크립트·커스텀 태스크는 configuration cache와 호환되게 작성한다(태스크 실행 중 `project` 접근 금지 등). 경고가 나면 끄지 말고 원인을 고친다.

## Gradle Wrapper 버전 변경
- `gradle-wrapper.properties`에 `distributionSha256Sum`이 고정되어 있다. 버전을 바꿀 때는 Gradle 공식 사이트의 체크섬(`https://services.gradle.org/distributions/gradle-<버전>-bin.zip.sha256`)을 함께 지정한다.
  `./gradlew wrapper --gradle-version <버전> --distribution-type bin --gradle-distribution-sha256-sum <체크섬>`

## 코드 규칙
- 베이스 패키지: `com.onlinestore`

## IDE (IntelliJ)
- Gradle 프로젝트는 저장소 루트가 아니라 `app/settings.gradle.kts` 기준으로 연결한다.
- 저장소 루트에 `.gradle/`이 생기면 IDE가 루트를 Gradle 프로젝트로 연결한 것이다. 이때 Gradle 탭에서 Unlink하면 Project 뷰가 비므로, Close Project → `.idea`, `.gradle` 삭제 → 루트 폴더 다시 Open → `app/settings.gradle.kts` Link 순서로 복구한다.

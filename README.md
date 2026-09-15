# 260915_cc

![Spring Boot](https://img.shields.io/badge/Spring%20Boot-4.1.1-6DB33F?logo=springboot&logoColor=white)
![Java](https://img.shields.io/badge/Java-17-007396?logo=openjdk&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring%20Security-7-6DB33F?logo=springsecurity&logoColor=white)
![Spring AI](https://img.shields.io/badge/Spring%20AI-2.0.1-6DB33F)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-pgvector-4169E1?logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![Claude Code](https://img.shields.io/badge/Claude%20Code-D97757?logo=claude&logoColor=white)

Google OAuth2 로그인 기반 Spring Boot 4 웹 애플리케이션. 이 저장소는 `Claude Code`로 개발을 진행하며, 협업 방식(rules), 반복 작업 지식(skills), 자동 기록(hooks)을 `.claude/` 아래에 함께 관리한다.

## 기술 스택

- **Spring Boot 4.1.1** / Java 17 toolchain
- **Spring Security 7** (Google OAuth2 로그인)
- **Spring Data JPA** + PostgreSQL(pgvector), **Redis**
- **Spring AI 2.0.1** (Google GenAI 챗/임베딩 모델)
- Thymeleaf, springdoc-openapi, Lombok

## Rules — `CLAUDE.md`

프로젝트 루트의 `CLAUDE.md`는 `Claude Code`가 이 저장소에서 항상 따르는 행동 규칙이다.

- 모든 작업을 시작하기 전에 적절한 모델(`Opus 5` / `Sonnet 5`)과 effort를 먼저 제안하고, 사용자 동의와 `"시작하자"` 명시적 입력을 받은 뒤에만 실행한다.

**의의**: 모델/effort를 매번 사람이 직접 확인하게 함으로써, 작업 규모에 안 맞는 과금이나 응답 품질 저하를 방지하고 실행 전 합의 지점을 명시적으로 만든다.

## Skills — `.claude/skills/`

Skill은 특정 작업을 수행할 때 `Claude Code`가 참고하는 재사용 가능한 절차/지식 문서다.

| Skill | 설명 |
|---|---|
| `commit-convention` | 커밋 메시지를 `<영어 category> : <한글 설명>` 형식(`feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `style`)으로 강제한다. |
| `spring-boot-4` | 이 프로젝트가 쓰는 `Spring Boot 4` / `Spring Security 7` / Jackson 3 / `JSpecify` 등 최신 문법과 breaking change를 정리해, 3.x 시절 문법(`antMatchers`, `spring-boot-starter-web` 등)으로 회귀하지 않도록 가이드한다. |

**의의**: 코드베이스별 컨벤션과 프레임워크 버전 지식을 대화 맥락이 아니라 파일로 고정해, 세션이 바뀌거나 새로 합류한 협업자(사람이든 에이전트든)도 동일한 기준으로 작업하게 한다.

## Hooks — `.claude/settings.json`

Hook은 `Claude Code`의 세션 라이프사이클 이벤트에 맞춰 자동 실행되는 셸 커맨드다. 이 프로젝트는 주요 이벤트마다 프로젝트 루트의 `claude.log`에 한 줄씩 기록을 남긴다.

| 이벤트 | 기록 내용 |
|---|---|
| `SessionStart` | 세션 시작 및 source(startup/resume/clear 등) |
| `UserPromptSubmit` | 사용자가 입력한 프롬프트 앞 50자 |
| `PreToolUse` (Bash) | 실행 직전 bash 명령어 앞 50자 |
| `PostToolUse` (Write/Edit) | 생성/수정된 파일 경로 |
| `Notification` | 알림 메시지 앞 50자 |
| `PreCompact` | 컨텍스트 압축 트리거(manual/auto) |
| `Stop` | 응답 종료 시각, session id, 마지막 응답 텍스트 앞 50자 |

**의의**: 세션이 끝나면 사라지는 대화 로그와 달리, 무엇을 언제 실행했고 무엇을 응답했는지가 파일로 남아 사후 감사·디버깅·작업 이력 추적이 가능해진다.

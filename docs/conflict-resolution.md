# Merge Conflict Resolution Log

## 1. 충돌 1: 자명한 충돌 (Hunk 충돌)
- **참여자**: 장양환, 조은익
- **대상 파일**: `docs/CONTRIBUTING.md`
- **발생 원인**: 동일 위치에 커밋 규칙 항목(`test` vs `style`)을 동시 추가하여 라인 충돌 발생
- **충돌 마커**: `<<<<<<< HEAD` (style) vs `>>>>>>> origin/main` (test)
- **해결 전략 및 절차**: GitHub 웹의 `Resolve conflicts` 편집기에서 충돌 마커를 확인하고 두 규칙(`test`, `style`)을 순서대로 모두 유지(Union Merge)하여 병합 커밋(`04d6a73`) 생성 후 최종 머지(`15ae97d`)
- **배운 점(Learnings)**:
  - 단순 라인 충돌(Hunk conflict)은 GitHub 웹 인터페이스에서도 신속하게 해결할 수 있음을 확인했습니다.
  - 다만 웹 편집기에서는 해결과 동시에 추가 문서(`conflict-resolution.md`)를 함께 스테이징할 수 없으므로, 기록 문서 생성이 수반되거나 복잡한 충돌(비자명 충돌 등)은 로컬 터미널(CLI) 병합이 훨씬 유연하고 안전하다는 점을 체득했습니다.

## 2. 충돌 2: 비자명한 충돌 (Rename vs Modify)
- **참여자**: 조은익, 김건우
- **대상 파일**: `notes/03-conflict-guide.md` ➔ `notes/advanced/03-conflict-guide.md`
- **발생 원인**: 한쪽은 파일 경로 이동(Rename) 및 예방수칙 추가, 다른 쪽은 구 경로 파일의 동일 위치에 3-Way Merge 내용 추가(Modify)를 동시에 진행하여 3-Way 머지 시 충돌 발생
- **충돌 마커**: `HEAD:notes/03-conflict-guide.md` vs `origin/main:notes/advanced/03-conflict-guide.md` (Git이 이전 경로와 새 경로를 동시에 표시)
- **해결 전략**: 이동된 새 경로(`notes/advanced/...`)를 최종 경로로 채택하고 두 내용을 순서대로 통합
- **해결 커밋**: PR #20 머지 커밋
- **배운 점(Learnings)**:
  - 대규모 디렉터리 리팩터링이나 파일 이름 변경 시에는 반드시 팀원들에게 사전 공지하여 브랜치를 최신화하도록 해야 합니다.
  - Git은 파일 이름이 바뀌어도 내용 유사도를 기반으로 추적하여 새 경로에 충돌 마커를 생성한다는 내부 원리를 체득했습니다.
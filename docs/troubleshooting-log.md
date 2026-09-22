# Git Troubleshooting Practice Log

## 1. 종합 실습 요약표
| 팀원 | 사용 명령어 | 의도적 실수 상황 | 해결 및 복구 결과 |
|:---:|:---|:---|:---|
| **김상교** | `git commit --amend` | 커밋 메시지 오타 발생 | 최신 커밋 해시 재생성 및 메시지 수정 확인 |
| **장양환** | `git reset --soft` | 불필요한 임시 파일 포함 커밋 | unstage 후 커밋 취소, 임시 파일 안전 제외 |
| **조은익** | `git revert` | 원격 연습 브랜치에 push한 잘못된 노트 커밋 | 원본을 보존하고 역커밋을 같은 원격 브랜치에 push |
| **김건우** | `git stash` & `pop` | 작업 중 긴급 브랜치 전환 | 미완성 변경사항 임시 격리 보관 후 무손실 복구 |

## 2. 명령어별 선택 이유(Why) 및 협업 시 주의점
- **`git commit --amend`**:
  - **Why**: 오타 수정을 위해 불필요한 "오타 수정" 커밋을 추가로 남기지 않고 직전 커밋을 깔끔하게 덮어쓰기 위함입니다.
  - **주의점**: 이미 원격에 푸시된 커밋에 amend를 적용하면 커밋 해시가 바뀌어 강제 푸시가 필요해지므로, 반드시 **로컬에만 존재하는 커밋**에만 사용해야 합니다.
- **`git reset --soft`**:
  - **Why**: 실수로 들어간 파일을 커밋에서 제외하되, 정성껏 작성한 다른 코드 작업물은 유실 없이 Staging 상태로 보존하기 위함입니다.
  - **주의점**: `--hard`를 쓰면 작업 트리의 모든 변경사항이 영구 삭제되므로 반드시 `--soft`를 사용해야 하며, reset 후 `git restore --staged`로 제외할 파일을 명시적으로 unstage해야 합니다.
- **`git revert`**:
  - **Why**: 원격에 공유된 커밋의 히스토리를 재작성하지 않고 역커밋으로 변경을 취소하기 위함입니다. 이번 실습은 `practice/eunik-revert`에서 수행했으며 `main`에는 병합하지 않았습니다.
  - **주의점**: 머지 커밋을 revert할 때는 `-m 1` 옵션을 지정하여 부모 브랜치를 지정해야 합니다.
- **`git stash`**:
  - **Why**: 미완성 변경사항을 다른 브랜치 작업에 섞지 않고 보관하기 위함입니다. 변경이 있어도 전환 가능한 경우는 있지만, 전환 대상과 충돌하면 Git이 전환을 거부할 수 있습니다.
  - **주의점**: stash 스택에 너무 많은 작업을 오래 방치하면 나중에 pop 시 충돌이 발생할 수 있으므로, 용무를 마친 후 즉시 pop하여 적용해야 합니다.

## 3. 팀원별 상세 재현 절차 (Reproducible Steps)
### 3-1. 김상교: 직전 커밋 메시지 수정 (`git commit --amend`)
- **상황**: 커밋 메시지에 오타 발생 (`docs: Ad git basic summar note`)
- **수행 명령**: `git commit --amend -m "docs: Add git basics summary note"`
- **결과**: `git log -1` 확인 시 새로운 커밋 해시로 갱신되고 오타가 정정됨

### 3-2. 장양환: 실수 커밋 무손실 취소 (`git reset --soft`)
- **상황**: 보존할 `notes/reset-practice.md`와 불필요한 `temp_draft.txt`를 함께 커밋
- **수행 명령**:
  ```bash
  git reset --soft HEAD~1
  git restore --staged temp_draft.txt
  Remove-Item temp_draft.txt
  git diff --cached --name-only
  git commit -m "docs: Add study note excluding temp files"

  결과: reset 직후 두 파일이 Staging에 보존됨을 확인하고, 임시 파일만 제외하여 노트만 다시 커밋함. 실제 전후 출력과 커밋 해시를 첨부한다.

### 3-3. 조은익: 원격에 push한 일반 커밋 안전 취소 (git revert)
**상황**: practice/eunik-revert에 잘못된 주석을 커밋하고 원격에 push함
**수행 명령**: 원본 push 성공 확인 → git revert HEAD --no-edit → git push origin practice/eunik-revert → git log -2 --oneline

결과: 원본과 역커밋이 원격 연습 브랜치에 함께 보존됨. main에는 병합하지 않음. 실제 원본·역커밋 URL과 push 결과를 첨부한다.

#### 3-4. 김건우: 미완성 작업 임시 격리 및 복원 (git stash & pop)
**상황**: 미완성 작업 중 긴급하게 브랜치를 전환해야 하는 Dirty 상태 발생
**수행 명령**:
git stash push -m "work-in-progress"
git checkout -b practice/gunwoo-stash-check
git status
git checkout main
git stash pop
git diff -- notes/04-open-source.md
git branch -d practice/gunwoo-stash-check

결과: 작업 트리가 깨끗해져 브랜치 이동이 가능했고, 복귀 후 미완성 작업물이 완벽 복구됨
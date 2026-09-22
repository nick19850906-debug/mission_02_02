# 01. Git 기초 개념과 3대 영역

## 1. Git의 3대 작업 영역
1. **Working Directory**: 실제 파일을 수정하고 작업하는 로컬 디렉터리
2. **Staging Area (Index)**: 커밋할 파일들이 준비되는 중간 대기 영역 (`git add`)
3. **Repository (Commit History)**: 영구적으로 버전 기록이 저장되는 저장소 (`git commit`)

## 2. 기본 라이프사이클
- 파일 수정 ➔ `git add`로 스테이징 ➔ `git commit`으로 스냅샷 기록 ➔ `git push`로 원격 공유
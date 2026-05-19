# career-jd-matcher v1.3 패치 묶음 (Phase 1 Quick Wins)

이 폴더는 `jason-risk-lab/career-jd-matcher` 저장소에 **drop-in 교체**용으로 만든 v1.3 패치다. risk-lab 세션에서 career-jd-matcher 저장소를 직접 push할 권한이 없어 별도 폴더에 옮겨 두었다.

## 변경 요약 (v1.2 → v1.3, 비파괴적)

| # | 변경 | 파일 | 근거 |
|---|------|------|------|
| 1 | `when_to_use`·`allowed-tools` 프론트매터 추가, description 트리거 키워드 보강 | `SKILL.md` | [Anthropic Skills 베스트 프랙티스](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) |
| 2 | references 전체에 TOC(>100 line 파일 6개) 추가 | `references/*.md` | Anthropic 권고: Claude가 `head -100`로 잘라 읽어 누락하는 사고 방지 |
| 3 | "한성님 컨텍스트" 하드코딩 블록을 `user-profile.example.md`로 분리 | `references/user-profile.example.md` (신규) + 3개 파일 수정 | [proficiently-claude-skills](https://github.com/proficientlyjobs/proficiently-claude-skills) 프로필 외부화 패턴. 공유 가능성 확보 |
| 4 | 회사 분석 웹검색을 **타입 강제 쿼터**(3 재무 + 3 시장·뉴스 + 2 평판 + 2 경쟁사·JD 부서)로 명시 | `references/company-analysis.md` | [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents) typed-search 패턴 |
| 5 | changelog v1.3 항목 추가 | `SKILL.md` frontmatter | - |

## 적용 방법

### 옵션 A. 수동 복사 (가장 단순)

career-jd-matcher 저장소를 clone한 뒤 이 폴더의 파일들을 동일 경로로 덮어쓴다.

```bash
git clone https://github.com/jason-risk-lab/career-jd-matcher.git
cd career-jd-matcher
git checkout -b v1.3-phase1

# 이 proposals/career-jd-matcher-v1.3/ 폴더 내용을 복사
cp -r /path/to/Risk-Lab/proposals/career-jd-matcher-v1.3/SKILL.md ./SKILL.md
cp -r /path/to/Risk-Lab/proposals/career-jd-matcher-v1.3/references/* ./references/

# .gitignore에 user-profile.md 추가 (개인정보 보호)
cat >> .gitignore <<EOF

# v1.3: 사용자별 프로필 (커밋 금지)
references/user-profile.md
EOF

git add -A && git commit -m "v1.3: Phase 1 quick wins (frontmatter, TOC, user-profile, typed search)"
```

### 옵션 B. PR로 적용

이 폴더 그대로 별도 브랜치에 올린 뒤 cherry-pick하거나, 위 명령들을 PR description에 붙여 자동 적용.

## 적용 후 사용자가 해야 할 일

1. **본인 프로필 작성**: `references/user-profile.example.md` 를 보고 `references/user-profile.md` 를 만든다. (이 파일은 .gitignore에 들어가 커밋되지 않음)
2. **스킬 호출 시 메모리/CLAUDE.md 또는 첨부로 user-profile.md 로딩** — 스킬은 schema만 알고 데이터는 사용자가 주입.

## 적용하지 않은 것 (Phase 2·3 후속 작업)

- 페르소나 9명 병렬 sub-agent 분리 → Phase 3
- Fact-Checker tiered 게이트 → Phase 2
- Bull/Bear 토론 라운드 → Phase 3
- `scripts/build_docx.py` → Phase 2
- 면접 준비 스킬 분리 → Phase 3
- 한국어 AI 작문 디텍션 룰 → Phase 2
- Cross-talk 페르소나 라운드 → Phase 2

자세한 백로그는 `PATCH-SUMMARY.md` 참조.

## 변경 없는 파일

- `references/jd-matching-rubric.md` (95줄, 100줄 미만이라 TOC 불필요, 사용자 컨텍스트 없음) — 기존 파일 유지.

## 호환성

- v1.2와 워크플로우·출력 형식 100% 동일. 사용자에게 보이는 동작 변화 없음.
- 단, **사용자별 컨텍스트가 자동 반영되던 부분은 user-profile.md 파일을 로딩해야 동작**. 미로딩 시 일반 모드로 동작.

## 검증 체크리스트

- [ ] SKILL.md `description` 1024자 이내 확인
- [ ] SKILL.md `description` + `when_to_use` 합쳐 1536자 이내 확인
- [ ] SKILL.md 본문 500줄 이내 확인 (현재 ~260줄)
- [ ] 각 reference 파일 TOC 헤더 정상 렌더링 확인
- [ ] "한성님" 단어가 references 본문에서 사라졌는지 확인 (`grep -r "한성" references/`)
- [ ] `references/user-profile.md`가 .gitignore에 있는지 확인

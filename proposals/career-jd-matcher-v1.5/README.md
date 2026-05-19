# career-jd-matcher v1.5 패치 묶음 (Phase 2 + Phase 3)

이 폴더는 `jason-risk-lab/career-jd-matcher`에 적용할 v1.5 패치 (Phase 2 전체 + Phase 3 대부분)다. **v1.3 패치를 먼저 적용한 다음 이 패치를 적용한다** (혹은 v1.5만 신규 적용도 가능 — v1.3 변경사항을 모두 포함).

## v1.4 → v1.5 변경 요약

Phase 2 전체:

| # | 변경 | 파일 | 근거 |
|---|------|------|------|
| P2-1 | docx 결정적 생성 스크립트 | `scripts/build_docx.py` (481줄, 실제 동작) | 매 호출 toxin·일관성 손실 제거 |
| P2-2 | Fact-Checker 격리 sub-agent | `references/fact-checker.md` | 편향 누출 차단 (MadeByTokens/resume-helper 패턴) |
| P2-3 | Fact-Checker 등급별 게이트 | `references/fact-checker.md` | 이진 → Hard/Soft/Ambiguous 3등급 (HalluMat 패턴) |
| P2-4 | 한국어 AI 작문 디텍션 | `references/korean-ai-detection.md` | jobstack "결이요" + claude-resume-kit AI fingerprint scan |
| P2-5 | 페르소나 cross-talk 라운드 | `references/persona-evaluation.md` + `parallel-persona-design.md` | CollabEval 3-phase 패턴 |

Phase 3 일부:

| # | 변경 | 파일 | 근거 |
|---|------|------|------|
| P3-6 | 페르소나 9명 병렬 sub-agent dispatch | `references/parallel-persona-design.md` | Anthropic 공식 `context: fork` + Task tool |
| P3-7 | Bull vs Bear 토론 라운드 | `references/company-analysis.md` (1.8.5 신규) | TauricResearch/TradingAgents 패턴 |
| P3-8 | 모의면접 신규 스킬 (`mock-interview`) | **별도 폴더 `proposals/mock-interview-v1.0/`** | jobstack /mock-interview + ResumeSkills interview-prep |
| P3-10 | 회귀 검증 하네스 골격 | `evaluations/` (README, run_eval.py, fixtures slot) | promptfoo / openevals 패턴 |

Phase 3 보류:

| # | 항목 | 사유 |
|---|------|------|
| P3-9 | 큰 스킬을 작은 명령어로 쪼개기 (plugin 재구성) | 네이밍·범위 결정이 사용자 입력 필요. v2.0 별도 안건. |

## 폴더 구조

```
proposals/career-jd-matcher-v1.5/
├── README.md                                  # 이 파일
├── SKILL.md                                   # v1.5 메인 워크플로우 (214줄)
├── references/
│   ├── company-analysis.md                    # v1.3 + Bull/Bear (237줄)
│   ├── persona-evaluation.md                  # v1.5 병렬 + cross-talk + AI 디텍션 통합 (343줄)
│   ├── fact-checker.md                        # 신규: 격리 + tiered (181줄)
│   ├── korean-ai-detection.md                 # 신규: BLACK 11번 항목 (154줄)
│   └── parallel-persona-design.md             # 신규: 페르소나 병렬 디스패치 (162줄)
├── scripts/
│   └── build_docx.py                          # 신규: 결정적 docx 변환 (481줄, 실 동작 검증)
└── evaluations/
    ├── README.md                              # 신규: 회귀 검증 가이드 (149줄)
    ├── run_eval.py                            # 신규: 검증 러너 (245줄)
    └── fixtures/
        └── README.md                          # 픽스처 채우는 법 (92줄)

proposals/mock-interview-v1.0/                 # 신규 독립 스킬 (별도 PR)
├── SKILL.md                                   # 5 모드 모의면접 (175줄)
└── references/
    ├── interview-modes.md                     # 5 모드 상세 (219줄)
    ├── star-bank.md                           # STAR 답변 뱅크 (204줄)
    └── question-generation.md                 # 질문 생성 룰 (207줄)
```

**총 3,063줄** (career-jd-matcher 2,258 + mock-interview 805).

## 변경 없는 파일 (v1.3 그대로 사용)

이 v1.5 패치에 포함되지 않은 파일은 v1.3 (이전 패치)의 것을 그대로 사용:

- `references/jd-matching-rubric.md` (95줄, 변경 없음)
- `references/resume-refinement.md` (124줄, v1.3 그대로)
- `references/cover-letter-refinement.md` (139줄, v1.3 그대로)
- `references/decision-matrix.md` (182줄, v1.3 그대로)
- `references/analysis-report.md` (201줄, v1.3 그대로)
- `references/user-profile.example.md` (129줄, v1.3 그대로)

따라서 v1.5 적용은 v1.3 + 본 패치 = 완전한 v1.5 상태가 된다.

## 적용 방법

### 옵션 A: v1.3 이미 적용한 저장소에 누적 적용

```bash
cd career-jd-matcher
git checkout -b v1.5-phase2-3

# v1.5에서 신규/변경된 파일만 복사
cp /path/to/Risk-Lab/proposals/career-jd-matcher-v1.5/SKILL.md ./SKILL.md
cp /path/to/Risk-Lab/proposals/career-jd-matcher-v1.5/references/company-analysis.md ./references/
cp /path/to/Risk-Lab/proposals/career-jd-matcher-v1.5/references/persona-evaluation.md ./references/
cp /path/to/Risk-Lab/proposals/career-jd-matcher-v1.5/references/fact-checker.md ./references/
cp /path/to/Risk-Lab/proposals/career-jd-matcher-v1.5/references/korean-ai-detection.md ./references/
cp /path/to/Risk-Lab/proposals/career-jd-matcher-v1.5/references/parallel-persona-design.md ./references/

mkdir -p scripts evaluations/fixtures
cp /path/to/Risk-Lab/proposals/career-jd-matcher-v1.5/scripts/build_docx.py ./scripts/
cp /path/to/Risk-Lab/proposals/career-jd-matcher-v1.5/evaluations/README.md ./evaluations/
cp /path/to/Risk-Lab/proposals/career-jd-matcher-v1.5/evaluations/run_eval.py ./evaluations/
cp /path/to/Risk-Lab/proposals/career-jd-matcher-v1.5/evaluations/fixtures/README.md ./evaluations/fixtures/

# Python 의존성 (scripts/build_docx.py + evaluations/run_eval.py 용)
echo "python-docx>=1.1.0" >> requirements.txt
echo "PyYAML>=6.0" >> requirements.txt

# fixtures 개인정보 보호
echo -e "\n# v1.5: 평가 픽스처 (개인정보 포함 가능)\nevaluations/fixtures/case-*/" >> .gitignore

git add -A && git commit -m "v1.5: Phase 2 + Phase 3 (parallel personas, tiered Fact-Checker, Bull/Bear, AI detection, docx script, eval harness)"
```

### 옵션 B: v1.3 미적용 상태에서 v1.5로 한 번에 적용

먼저 v1.3 패치 (Risk-Lab의 `proposals/career-jd-matcher-v1.3/`)를 적용한 다음 위 옵션 A를 따른다.

### mock-interview 별도 적용

mock-interview는 독립 스킬이라 별도 저장소나 별도 폴더로 분리하는 것이 적절:

```bash
cd ..
git clone https://github.com/jason-risk-lab/mock-interview.git  # 또는 신규 생성
cd mock-interview
cp -r /path/to/Risk-Lab/proposals/mock-interview-v1.0/* ./
git add -A && git commit -m "Initial mock-interview skill v1.0"
```

## 사용 방법 (v1.5)

기존 사용법과 동일. 사용자는 채용공고 + 이력서 + 자소서를 던지면 자동으로 5단계 실행. 변화는 **백엔드 동작**:

- **시간 단축**: 페르소나 병렬 dispatch로 Step 5 평가가 ~40% 빨라짐 (직렬 8명 → 동시 8명)
- **신뢰성 향상**: Fact-Checker 격리 + 등급별 → Hard claim 거짓은 절대 통과 불가
- **AI 작문 방지**: BLACK 페르소나가 한국어 AI 표지 자동 감지
- **분석 깊이**: Bull vs Bear 토론으로 Decision Matrix 근거 단단
- **결정적 산출**: docx-js 즉석 생성 대신 Python 스크립트 호출 (속도↑, 일관성↑)

사용자에게 보이는 출력 형식은 v1.2와 거의 동일. 추가:
- Section 1.8.5 (Bull/Bear 토론 표)
- Section 5에 cross-talk 라운드 표 (적용 시)
- Section 5에 AI 작문 표지 카운트

## 검증 체크리스트

적용 후 확인:

- [ ] SKILL.md `description` + `when_to_use` 합산 1536자 이내
- [ ] SKILL.md 본문 500줄 이내 (현재 214줄)
- [ ] scripts/build_docx.py 실행 가능: `python3 scripts/build_docx.py --help`
- [ ] evaluations/run_eval.py 실행 가능: `python3 evaluations/run_eval.py --help`
- [ ] 신규 references 5개 파일 TOC 정상
- [ ] mock-interview SKILL.md description 1024자 이내
- [ ] `python-docx`, `PyYAML` 의존성 설치 확인
- [ ] fixtures/ 본인 케이스 1개 이상 채워 회귀 검증 1회 실행

## 호환성 / 폴백

- **병렬 dispatch 미지원 환경**: `parallel-persona-design.md`의 폴백 패턴 자동 적용 (직렬 sub-agent 또는 단일 sub-agent 위임)
- **docx 스크립트 미동작**: 환경에 `python-docx` 미설치 시 v1.4의 docx-js 즉석 생성으로 자동 fallback (Claude가 SKILL.md의 fallback 안내를 따름)
- **fact-checker.md sub-skill 인식 안 됨**: `context: fork` 미지원 환경에서 일반 Task로 디스패치
- **user-profile.md 부재**: v1.3과 동일하게 일반 모드 동작

## 알려진 제약

- **scripts/build_docx.py**: 마크다운 → docx 변환은 인라인 파서를 쓰므로 매우 복잡한 markdown(중첩 표, 인용 안 표, 각주)은 100% 변환 보장 안 됨. 본 스킬이 생성하는 마크다운 구조에 한해 검증됨.
- **회귀 검증 하네스**: v1.5는 골격만 제공. 본인 케이스를 fixtures/에 채워야 작동.
- **Cross-talk 라운드**: sub-agent 호출 ~50% 추가. 비용 민감 사용자는 1라운드에서 평균 9.0+면 자동 스킵 (이미 적용).

## 참고 (벤치마킹 출처)

- [MadeByTokens/resume-helper](https://github.com/MadeByTokens/resume-helper) — 적대적 멀티에이전트 + Fact-Checker 격리
- [thesun4sky/jobstack](https://github.com/thesun4sky/jobstack) — 한국어 AI 작문 진단 + 모의면접 5 모드
- [ARPeeketi/claude-resume-kit](https://github.com/ARPeeketi/claude-resume-kit) — provenance flag + AI fingerprint
- [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents) — Bull/Bear 토론
- [HalluMat (arxiv 2512.22396)](https://arxiv.org/pdf/2512.22396) — tiered fact-check
- [CollabEval (arxiv 2603.00993)](https://arxiv.org/pdf/2603.00993) — 3-phase 평가
- [Anthropic Skills best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [Claude Code subagents docs](https://code.claude.com/docs/en/sub-agents)

## 다음 단계 (Phase 4 후보)

- **P3-9 plugin 재구성** — `/analyze`, `/match`, `/refine`, `/evaluate`, `/decide` 단품 호출 가능하게 분해. 네이밍 결정 필요.
- **다국어 자소서 (영문 CV)** — 글로벌 기업 지원 시 영문 출력 옵션.
- **연봉 협상 별도 스킬** — 현 트리거에서 제외된 항목을 별도 스킬로 보완.
- **Skill 마켓플레이스 등록** — ComposioHQ/awesome-claude-skills 등록.

각 항목은 별도 PR로 진행 권장.

# AI-AGENT-LAB

> KTDS 생성형 AI Agent 교육 프로젝트

## 목표

Claude Code를 Operator로 두고, 역할별 Agent + Skill + Hook 구조로  
**토큰 효율적인 1인 풀스택 SaaS 개발 환경**을 GitHub 기반으로 구축한다.

---

## 프로젝트 구조

```
ai-agent-lab/
├── CLAUDE.md                  ← Operator 핵심 지침
├── .claude/
│   ├── agents/
│   │   ├── architect.md       ← 설계 / 스키마 / API 구조
│   │   ├── dev.md             ← 코드 구현
│   │   ├── test.md            ← 테스트 / 검증
│   │   ├── security.md        ← 보안 리스크 검토
│   │   └── review.md          ← 코드 리뷰 / 리팩토링
│   ├── skills/                ← 기술별 Skill (필요시 추가)
│   └── hooks/
│       ├── pre-hook.md        ← 요청 분류 규칙
│       └── post-hook.md       ← 결과 압축 규칙
├── frontend/                  ← 웹 프론트엔드 (Next.js)
├── backend/                   ← 백엔드 API 서버
├── docs/
│   ├── architecture/          ← 시스템 설계 문서
│   └── api-spec/              ← OpenAPI 스펙
└── tests/                     ← 테스트 코드
```

---

## 환경 구축 단계

### Step 1 — Node.js 설치

```bash
node -v  # v18 이상 확인
```

v18 미만이거나 없으면:

```bash
# macOS
brew install node

# Windows → https://nodejs.org 에서 LTS 버전 설치
```

---

### Step 2 — Claude Code 설치

```bash
npm install -g @anthropic-ai/claude-code
claude --version  # 설치 확인
```

---

### Step 3 — GitHub CLI 설치 + 인증

```bash
# macOS
brew install gh

# Windows
winget install GitHub.cli
```

Git Bash 사용 시 PATH 수동 등록 필요:

```bash
export PATH=$PATH:"/c/Program Files/GitHub CLI"
echo 'export PATH=$PATH:"/c/Program Files/GitHub CLI"' >> ~/.bashrc
```

GitHub 인증:

```bash
gh auth login
# GitHub.com → HTTPS → Login with a web browser 순으로 진행
```

---

### Step 4 — Repo 클론 + 디렉토리 구조 생성

```bash
gh repo clone YOUR_USERNAME/ai-agent-lab
cd ai-agent-lab

mkdir -p .claude/agents .claude/skills .claude/hooks
mkdir -p frontend backend docs/architecture docs/api-spec tests
```

---

### Step 5 — .gitignore 설정

> ⚠️ 커밋 전에 반드시 설정해야 node_modules가 repo에 올라가지 않는다.

```bash
cat > .gitignore << 'EOF'
node_modules/
.env
.env.local
dist/
build/
.next/
*.log
.DS_Store
.vscode/
EOF

git rm -r --cached node_modules  # 이미 추적 중이면 제거
```

---

### Step 6 — CLAUDE.md 생성 (Operator 지침)

```bash
cat > CLAUDE.md << 'EOF'
# AI-AGENT-LAB — Operator Instructions

## 역할
이 프로젝트의 오케스트레이터. 요청을 분류하고 적절한 Agent에게 위임한다.

## Agent 라우팅 규칙
- 설계 / 구조 / 스키마 / ERD  → .claude/agents/architect.md
- 구현 / 코드 / 만들어 / 개발  → .claude/agents/dev.md
- 테스트 / 검증 / 커버리지     → .claude/agents/test.md
- 보안 / 취약점 / 리스크       → .claude/agents/security.md
- 리뷰 / 리팩토링 / 개선       → .claude/agents/review.md

## 토큰 최적화 원칙
1. 각 Agent는 자신의 .md 파일만 참조한다
2. Skill은 해당 기술이 직접 사용될 때만 로드한다
3. 작업 완료 후 결과를 3줄 이내로 요약해 다음 단계에 전달한다
4. 파일 전체를 읽기 전에 필요한 부분만 먼저 확인한다

## 작업 흐름
요청 수신 → 분류 → Agent 지정 → Skill 로드 → 실행 → 요약 → 커밋
EOF
```

---

### Step 7 — Claude Code 실행

```bash
claude
```

인증 방식 선택:

| 방식 | 조건 | 비용 |
|---|---|---|
| Claude.ai Pro 계정 | Pro/Max/Team 구독 | 월 $20 고정 |
| Anthropic Console API 키 | console.anthropic.com에서 발급 | 사용량 × 단가 |

> **Pro 플랜 참고:** 5시간 롤링 윈도우 내 사용량 제한 있음.  
> 한국 시간 기준 낮 시간대(오전~오후)가 피크타임 외 시간으로 더 여유로움.

---

## 참고 링크

- [Claude Code 공식 문서](https://docs.anthropic.com/claude-code)
- [Anthropic Console](https://console.anthropic.com)
- [Claude.ai Pro 구독](https://claude.ai/upgrade)

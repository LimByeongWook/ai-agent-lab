# ai-agent-lab
KTDS 생성형 ai agent  교육

Claude Code를 Operator로 두고, 역할별 Agent + Skill + Hook 구조로 토큰 효율적인 1인 풀스택 SaaS 개발 환경을 GitHub 기반으로 구축한다.

Step 1 — Node.js 설치
터미널을 열고 현재 설치 여부 확인:
bashnode -v
v18 이상이면 다음 단계로 넘어가세요. 없거나 낮은 버전이면:
macOS (Homebrew)
bashbrew install node
Windows / 직접 설치
→ https://nodejs.org 에서 LTS 버전 다운로드 후 설치

Step 2 — Claude Code 설치
bashnpm install -g @anthropic-ai/claude-code
설치 확인:
bashclaude --version

Step 3 — GitHub CLI 설치 + 인증
macOS
bashbrew install gh
gh auth login
Windows
bashwinget install GitHub.cli
gh auth login
gh auth login 실행 시 → GitHub.com → HTTPS → 브라우저 인증 순으로 진행하면 돼요.

Step 4 — repo clone
AI-AGENT-LAB repo를 로컬로 가져오기:
bashgh repo clone YOUR_USERNAME/AI-AGENT-LAB
cd AI-AGENT-LAB
그 다음 디렉토리 구조 한 번에 생성:
bashmkdir -p .claude/agents .claude/skills .claude/hooks
mkdir -p frontend backend docs/architecture docs/api-spec tests

Step 5 — CLAUDE.md 생성
bashcat > CLAUDE.md << 'EOF'
# AI-AGENT-LAB Operator Instructions

## 역할
이 프로젝트의 오케스트레이터. 요청을 분류하고 적절한 Agent에게 위임한다.

## Agent 라우팅
- 설계 / 구조 / 스키마 → .claude/agents/architect.md
- 구현 / 코드 / 만들어 → .claude/agents/dev.md
- 테스트 / 검증 → .claude/agents/test.md
- 보안 / 취약점 → .claude/agents/security.md
- 리뷰 / 리팩토링 → .claude/agents/review.md

## 토큰 최적화 원칙
1. 각 Agent는 자신의 .md 파일만 참조한다
2. Skill은 해당 기술이 직접 사용될 때만 로드한다
3. 작업 완료 후 결과를 3줄 이내로 요약한다
4. 전체 파일 읽기 전 필요한 부분만 먼저 확인한다
EOF
Agent 파일들 생성:
bashfor agent in architect dev test security review; do
  echo "# ${agent} agent\n\n## 역할\n\n## 작업 범위\n\n## 출력 형식" \
  > .claude/agents/${agent}.md
done

Step 6 — Claude Code 실행
bashclaude
처음 실행 시 Anthropic API 키 또는 Claude.ai Pro 계정 연동을 요청해요.

Pro 계정 사용: claude 실행 후 브라우저 인증 방식 선택
API 키 사용: console.anthropic.com에서 키 발급 후 입력


# 처음 시작하기

이 폴더는 **Codex 또는 Claude Code와 함께** 움직이는 영상에서 벽면의 대상을 지우는
수업용 starter입니다. Python·FFmpeg가 없는 컴퓨터도 설치 단계부터 시작할 수 있습니다.
After Effects(Mocha AE 포함)는 Adobe 라이선스로 별도 준비하며, 앱과 모델은 이 폴더에
포함하지 않습니다.

## 폴더를 연 뒤 이 문장을 붙여 넣기

ZIP을 완전히 풀고 `AGENTS.md`, `scripts`, `docs`, `.agents`가 있는 폴더를 에이전트의
작업 폴더로 선택합니다. Git이나 전역 스킬 설치는 필요하지 않습니다.

> 이 폴더의 AGENTS.md와 .agents/skills/aftereffect-remove/SKILL.md를 읽고 시작해 주세요.
> 실제 OS와 설치된 도구를 확인하고, OS setup 문서의 공식 출처로 빠진 것만 준비해 주세요.
> Python 3.13(또는 3.12), 프로젝트 .venv와 tools.local.json을 사용해 주세요.
> 제가 주는 짧은 영상에서 벽에 붙은 대상을 지우는 과정을 docs/rules.md대로 한 단계씩
> 진행해 주세요. Mocha 추적은 제가 AE에서 직접 하겠습니다. Adobe 로그인·결제·약관
> 동의도 제가 직접 합니다. 원본은 보존하고, 기준을 통과했는지와 남은 한계를
> 숫자와 함께 구분해 알려 주세요. 실패를 성공으로 보고하지 마세요.

## 진행 순서

1. 현재 OS의 [Windows setup](docs/setup-windows.md) 또는 [Mac setup](docs/setup-macos.md)만
   읽고 도구를 준비합니다. `setup_environment.py --check`는 표준 라이브러리만으로
   검사하고, `--configure`는 경로만 저장하며, `--install`을 요청할 때만 `.venv`에
   패키지를 설치합니다.
2. [workflow](docs/workflow.md)대로 내 영상과 `work/<shot>` 폴더를 정하고, 추적 전에
   `contract.json`에 기준을 적습니다.
3. Mocha 추적 → 독립 검수 → 필요한 구간만 ECC 보정 → 클린 플레이트 → 패치 → AE 순서로
   진행합니다. 각 단계의 규칙은 [rules](docs/rules.md)에 있습니다.

아무 영상이나 성공하지 않습니다. 첫 샷은 짧고, 지울 대상이 평평하고 질감 있는
벽면에 붙어 있고, 사람이 대상을 크게 가리지 않는 영상으로 시작합니다.

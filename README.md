# AFTEREFFECT_REMOVE

**VFX 학생이 LLM 에이전트와 함께 시작하는 리무브 스킬의 출발점.**
움직이는 영상에서 벽면에 붙은 간판·창문·표지판을 지우고, 편집 가능한 After Effects
프로젝트로 정리합니다. Mocha AE로 추적하고, Python(OpenCV)으로 검수·보정·패치를 만들고,
Python이 만든 JSX로 AE 프로젝트를 구성합니다. AE MCP나 별도 서버는 쓰지 않습니다.

## 학생은 이렇게 시작합니다

공통강의의 Git 세팅을 마친 컴퓨터에서, 이 저장소를 내 GitHub 계정에 **포크**해서 시작합니다.

1. Codex 또는 Claude Code에 붙여 넣습니다.

   > https://github.com/hyun2xyz/AFTEREFFECT_REMOVE 를 제 GitHub 계정으로 포크해서 문서(Documents) 폴더에
   > 받아 주세요. 끝나면 origin과 upstream 주소를 보여 주세요.

2. 받은 `AFTEREFFECT_REMOVE` 폴더를 Codex에서 다시 열고 [START-HERE.md](START-HERE.md)의 문장이나
   다음처럼 요청합니다.

> START-HERE.md, AGENTS.md와 공유 스킬을 읽고 제 컴퓨터부터 확인해 주세요.
> 필요한 것만 설치 안내하고, 제가 주는 짧은 영상에서 벽에 붙은 간판을 지우는 과정을
> 한 단계씩 진행해 주세요. 원본은 보존하고, 기준을 통과했는지 숫자로 알려 주세요.

- [포크로 시작하고 업데이트 받기](docs/fork-and-update.md)
- [아무것도 없는 Windows 준비](docs/setup-windows.md)
- [아무것도 없는 Mac 준비](docs/setup-macos.md)
- [리무브 규칙](docs/rules.md)
- [첫 영상부터 AE까지](docs/workflow.md)

## 첫 버전의 범위

1. 짧은 CFR 영상의 프레임과 정확한 FPS를 준비하고, 추적 전에 기준을 정합니다.
2. Mocha AE로 같은 벽면을 추적하고, 추적에 쓰지 않은 원본 픽셀로 오차를 잽니다.
3. 기준을 넘는 구간만 같은 벽면의 ECC로 국소 보정합니다.
4. 기준 프레임 한 장으로 클린 플레이트를 만들고, 지울 영역만 패치로 렌더합니다.
5. 원본 + 패치 레이어의 AE 프로젝트를 만들고 최종 파일을 검수합니다.

이 범위는 실제로 기준(1920×1080에서 p95 1.5 px, 최대 3 px)을 통과한 빠른 패닝 간판
리무브 사례를 바탕으로 정했습니다. 사람·차처럼 움직이는 대상 지우기, 영상 인페인팅
모델, 화면 밖으로 크게 회전하는 드론 샷은 이 출발점에 포함하지 않습니다.

## 에이전트/개발자

공유 스킬은 [.agents/skills/aftereffect-remove/SKILL.md](.agents/skills/aftereffect-remove/SKILL.md)입니다.
Codex는 저장소 스킬을 쓰고, Claude Code는 `CLAUDE.md`에서 같은 지침을 읽습니다.
전역 설정이나 개인 스킬을 바꾸는 설치기는 없습니다.

```powershell
py -3.13 -B scripts/setup_environment.py --check
.\.venv\Scripts\python.exe -B -m unittest discover -s tests -v
```

```sh
python3.13 -B scripts/setup_environment.py --check
.venv/bin/python -B -m unittest discover -s tests -v
```

지금 `scripts/`에는 설치 점검 도구만 있습니다. 추적 검수·ECC 보정·패치 렌더·JSX 생성은
에이전트가 샷마다 `work/<shot>/`에 만들고, 실제 샷에서 통과한 것만 `scripts/`로 옮겨
각자의 스킬로 키웁니다. 실행 파일·Adobe 앱·학생 영상·AEP·생성 이미지는 재배포하지 않습니다.
[출처·라이선스](THIRD_PARTY_NOTICES.md)를 확인하세요.

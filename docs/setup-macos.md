# Mac 준비

[START-HERE](../START-HERE.md)대로 압축을 푼 폴더를 에이전트에 엽니다. Git은
필요하지 않습니다. 설치된 도구를 먼저 확인하고 빠진 것만 준비합니다.

## 1. Python과 FFmpeg

지원 버전은 **3.12 / 3.13**, 첫 설치 권장은 3.13입니다.

```sh
python3.13 -B scripts/setup_environment.py --check
```

빠진 항목이 있으면 [Homebrew 공식 설치 안내](https://brew.sh/)를 확인하고,
[Python 3.13](https://formulae.brew.sh/formula/python@3.13)과
[FFmpeg](https://formulae.brew.sh/formula/ffmpeg)을 설치합니다.
Homebrew가 없다면 공식 설치 명령을 먼저 실행합니다.

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

```sh
TASK_BREW="$(command -v brew 2>/dev/null || true)"
[ -x "$TASK_BREW" ] || TASK_BREW=/opt/homebrew/bin/brew
[ -x "$TASK_BREW" ] || TASK_BREW=/usr/local/bin/brew
"$TASK_BREW" install python@3.13 ffmpeg
TASK_PYTHON="$("$TASK_BREW" --prefix python@3.13)/bin/python3.13"
"$TASK_PYTHON" -B scripts/setup_environment.py --configure
"$TASK_PYTHON" -B scripts/setup_environment.py --install
.venv/bin/python -B scripts/setup_environment.py --check
```

`--configure`는 찾은 `ffmpeg`·`ffprobe`의 절대 경로만 `tools.local.json`에 씁니다.
경로가 다르면 `--ffmpeg /실제/경로/ffmpeg --ffprobe /실제/경로/ffprobe`로 지정합니다.
`--install`은 이 폴더의 `.venv`에 NumPy, OpenCV(headless), Pillow만 설치합니다.
이후 문서의 `python`은 `.venv/bin/python`입니다.

## 2. After Effects와 Mocha AE

[Adobe 공식 설치 안내](https://helpx.adobe.com/download-install/using/download-install-new-computer.html)를
따라 학생 자신의 계정·라이선스로 After Effects를 설치합니다. 로그인·결제·약관 동의는
계정 소유자가 직접 합니다. **Mocha AE는 After Effects에 함께 설치되는 플러그인**입니다.
AE에서 Effect → Boris FX Mocha → Mocha AE가 보이는지 확인합니다.
기본 위치가 아니면 `--configure --after-effects "/Applications/Adobe After Effects 2026/Adobe After Effects 2026.app"`처럼 지정합니다.

## 3. 클린 플레이트 도구

가려진 벽을 만들 이미지 도구는 학생이 고릅니다. 예: 사용 중인 에이전트의 이미지
생성 기능, Photoshop 생성형 채우기. 유료 생성이면 실행 전에 비용을 확인합니다.
비상업 라이선스 모델(예: ProPainter 기본 라이선스)은 상업 작업에 쓰지 않습니다.

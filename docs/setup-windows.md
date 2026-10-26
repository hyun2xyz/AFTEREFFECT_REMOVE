# Windows 준비

Windows 10/11 x64 기준입니다. [START-HERE](../START-HERE.md)대로 압축을 푼 폴더를
에이전트와 PowerShell의 작업 폴더로 선택합니다. Git은 필요하지 않습니다.
설치된 도구를 먼저 확인하고 빠진 것만 준비합니다.

## 1. Python

지원 버전은 **3.12 / 3.13**, 첫 설치 권장은 3.13입니다. 이미 있으면 확인부터 합니다.

```powershell
py -3.13 -B scripts/setup_environment.py --check
```

빠진 항목이 있으면 종료 코드 `1`과 이유를 출력하고 아무것도 바꾸지 않습니다.
Python이 없다면 [Python 공식 Windows 설치기](https://www.python.org/downloads/windows/)의
3.13 x64 설치기를 씁니다. WinGet이 있다면 사용자 범위로 설치할 수 있습니다.

```powershell
winget install --id Python.Python.3.13 --exact --source winget --scope user --architecture x64
```

새 터미널에서 `py -3.13 --version`을 확인합니다.

## 2. FFmpeg

[FFmpeg 공식 Windows 링크](https://ffmpeg.org/download.html#build-windows)가 연결하는
[Gyan release essentials ZIP](https://www.gyan.dev/ffmpeg/builds/)을 받습니다.
`ffmpeg.exe`와 `ffprobe.exe`가 함께 들어 있습니다. 저장소 밖 사용자 폴더에 둡니다.

```powershell
$ToolRoot = Join-Path $env:LOCALAPPDATA 'AFTEREFFECT_REMOVE\tools'
New-Item -ItemType Directory -Path $ToolRoot -Force | Out-Null
$Zip = Join-Path $ToolRoot 'ffmpeg-release-essentials.zip'
Invoke-WebRequest 'https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-essentials.zip' -OutFile $Zip
Invoke-WebRequest 'https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-essentials.zip.sha256' -OutFile "$Zip.sha256"
$Expected = [regex]::Match((Get-Content "$Zip.sha256" -Raw), '\b[a-fA-F0-9]{64}\b').Value
if (-not $Expected -or (Get-FileHash $Zip -Algorithm SHA256).Hash -ne $Expected) { throw 'FFmpeg SHA256 mismatch' }
Expand-Archive -Path $Zip -DestinationPath (Join-Path $ToolRoot 'FFmpeg')
$Ffmpeg = (Get-ChildItem (Join-Path $ToolRoot 'FFmpeg') -Filter ffmpeg.exe -Recurse | Select-Object -First 1).FullName
$Ffprobe = Join-Path (Split-Path $Ffmpeg) 'ffprobe.exe'
```

## 3. 프로젝트 설정과 패키지

```powershell
py -3.13 -B scripts/setup_environment.py --configure --ffmpeg $Ffmpeg --ffprobe $Ffprobe
py -3.13 -B scripts/setup_environment.py --install
.\.venv\Scripts\python.exe -B scripts/setup_environment.py --check
```

`--configure`는 찾은 실행 파일의 절대 경로만 `tools.local.json`에 저장합니다.
`--install`은 이 폴더의 `.venv`에 NumPy, OpenCV(headless), Pillow만 설치합니다.
시스템 PATH와 전역 설정은 바꾸지 않습니다. 이후 문서의 `python`은
`.\.venv\Scripts\python.exe`입니다.

## 4. After Effects와 Mocha AE

[Adobe 공식 설치 안내](https://helpx.adobe.com/download-install/using/download-install-new-computer.html)를
따라 학생 자신의 계정·라이선스로 After Effects를 설치합니다. 로그인·결제·약관 동의는
계정 소유자가 직접 합니다. **Mocha AE는 After Effects에 함께 설치되는 플러그인**입니다.
AE에서 Effect → Boris FX Mocha → Mocha AE가 보이는지 확인합니다.
기본 위치가 아니면 `--configure --after-effects "<AfterFX.exe 경로>"`로 지정합니다.

## 5. 클린 플레이트 도구

가려진 벽을 만들 이미지 도구는 학생이 고릅니다. 예: 사용 중인 에이전트의 이미지
생성 기능, Photoshop 생성형 채우기. 유료 생성이면 실행 전에 비용을 확인합니다.
비상업 라이선스 모델(예: ProPainter 기본 라이선스)은 상업 작업에 쓰지 않습니다.

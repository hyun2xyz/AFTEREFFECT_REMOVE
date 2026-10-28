# After Effects 컴프 만들기

원본 플레이트와 결과 시퀀스를 레이어로 쌓은 **편집 가능한 AE 컴프**를 만듭니다.
`scripts/ae_comp.py`(Python 표준 라이브러리)가 샷 값으로 JSX를 만들고, 학생이 AE에서 실행합니다.
AE MCP, 상시 브리지, 자동 저장·렌더는 쓰지 않습니다. 설정의 근거는 [rules](rules.md) 8장입니다.

## 만드는 컴프

| 레이어(위 → 아래) | 소스 | 해석 |
| --- | --- | --- |
| Patch | `work/<shot>/patch/` (`--patch`) | 알파 Straight. `--patch-start` 프레임부터 |
| Plate | `work/<shot>/frames/` (`--plate`) | 알파 무시. 컴프 크기·길이·FPS의 기준 |

16 bpc, 작업 색공간 Rec.709 Gamma 2.4, linear blending을 켭니다(기준 사례 설정).

- 패치는 이미 추적한 풀캔버스 RGBA이므로 Corner Pin을 **적용하지 않습니다**(이중 변환).
- `--patch-start`는 패치 첫 장이 놓일 컴프 프레임입니다. 원본 첫 장이 0입니다. 예를 들어
  `contract.json`의 `frames`가 `[0, 209]`이고 패치가 `visible_frames` `[12, 134]`만 있으면 12입니다.
  패치가 원본 길이를 넘으면 생성기가 멈춥니다.

스크립트는 **빈 프로젝트에서만** 폴더 하나(샷 이름), 푸티지, 컴프 하나를 추가합니다. 저장·닫기·렌더는
하지 않습니다. 중간에 실패하면 이번 실행에서 추가한 항목을 지우고 바꾼 프로젝트 설정을 되돌립니다.

## 1. 소스 확인

- 시퀀스는 **폴더 하나에 시퀀스 하나**입니다. 같은 이름, 같은 자릿수(0 채움), 빈 번호 없는 연속 번호여야
  합니다. 다른 파일이 섞여 있거나 번호가 빠지면 생성기가 멈춥니다(숨김 파일은 무시).
- 영상 파일은 받지 않습니다. FFmpeg로 PNG 시퀀스를 먼저 뽑습니다([workflow](workflow.md) 1단계).
- 결과 시퀀스는 원본과 같은 해상도여야 합니다. AE가 가져온 크기가 다르면 스크립트가 멈춥니다.
- FPS는 `--fps 30000/1001` 또는 `work/<shot>/contract.json`의 `"fps": [30000, 1001]`입니다.
- 샷 이름은 공백 없이 글자·숫자·`_`·`-`·`.`로 씁니다. 한글도 됩니다.

## 2. JSX 만들기(에이전트)

```sh
.venv/bin/python -B scripts/ae_comp.py --shot shot01 --patch-start 0
```

```powershell
.\.venv\Scripts\python.exe -B scripts\ae_comp.py --shot shot01 --patch-start 0
```

`work/<shot>/ae/build_comp.jsx`가 생기고, 레이어별 프레임 수와 `job` 번호가 출력됩니다.
옵션 전체는 `--help`로 봅니다. 경로를 생략한 레이어는 위 표의 기본 폴더를 씁니다.

## 3. AE에서 실행(학생)

1. 열려 있는 작업을 저장합니다. **File > New > New Project**로 빈 프로젝트를 엽니다.
2. **File > Scripts > Run Script File**에서 `work/<shot>/ae/build_comp.jsx`를 고릅니다.
3. "컴프를 만들었습니다" 알림이 뜨고 컴프가 Viewer에 열립니다.
4. **File > Save As**로 `work/<shot>/ae/<shot>.aep`에 직접 저장합니다.

## 4. 결과 확인(에이전트)

스크립트는 `work/<shot>/ae/build_comp.result.json`을 남깁니다. 다음을 확인합니다.

- `job_id`가 2단계에서 출력된 번호와 같은지(예전 기록을 읽지 않도록).
- `status`가 `ok`인지. 아니면 `error`를 학생에게 그대로 전합니다.
- `project`의 bpc·작업 색공간·linear blending, `comp`의 크기·픽셀 종횡비·프레임 수.
- `layers`(아래 → 위)의 `in_frame`·`out_frame`, 결과 레이어의 `alpha`가 `straight`인지.

그다음 학생과 함께 시작·중간·끝 프레임을 봅니다. 저장한 AEP를 다시 열어 원본·패치 경로와 대표 프레임이 같은지 확인합니다([rules](rules.md) 8장).

| 멈춘 이유 | 뜻 |
| --- | --- |
| 프로젝트에 이미 항목이 있음 | 아무것도 바꾸지 않았습니다. 새 프로젝트에서 다시 실행합니다 |
| `has no alpha channel` | 결과 시퀀스에 알파가 없습니다. RGBA로 다시 렌더합니다 |
| `imported as N frames` | AE가 읽은 프레임 수가 다릅니다. 폴더 내용을 확인합니다 |
| `is WxH, plate is WxH` | 결과 해상도가 원본과 다릅니다 |
| 색 관리가 OCIO | Project Settings > Color를 Adobe로 둔 프로젝트에서 실행합니다 |
| 결과 기록을 쓰지 못함 | AE 설정 Scripting & Expressions의 'Allow Scripts to Write Files and Access Network'가 꺼져 있으면 컴프는 만들어지고 기록만 빠집니다. 학생이 알림 내용을 알려 주거나 직접 켭니다. 에이전트가 이 설정을 바꾸지 않습니다 |

## 검증 기록

2026-10-28, macOS, After Effects 26.3(26.3x87)에서 이 저장소의 `scripts/ae_comp.py`로 만든 JSX를
합성 시퀀스로 실행했습니다. 결과 기록은 [validation/ae-comp-macos-ae26.3-2026-10-28.json](validation/ae-comp-macos-ae26.3-2026-10-28.json)에 있습니다.
렌더 확인은 `saveFrameToPng`로 뽑은 8비트 PNG입니다.

| 확인 | 결과 |
| --- | --- |
| 프로젝트·컴프·레이어 읽기 | `16 bpc / Rec.709 Gamma 2.4 / linear on`, 640×360 PAR 1, 24프레임, 레이어 2개 |
| 패치 알파 밖(패치를 끈 컴프와 비교) | 최대 1/255. 그라데이션 6열의 8비트 반올림 |
| 패치 시작 전·끝난 뒤 프레임(3, 16) | 패치를 끈 컴프와 동일 |
| 불투명 패치 영역(프레임 4·10·15) | 원본과 다르고 세 프레임이 동일 |
| 알파 0 아래의 초록 RGB | 0픽셀(알파 Straight로 해석됨) |

File > Scripts 메뉴(최근 스크립트)로 실행해 "컴프를 만들었습니다" 알림이 한글로 뜨고 같은 `job_id`의 결과 기록이 남는 것도 확인했습니다. 함께 확인한 것(세 저장소가 같은 `ae_comp.jsx`를 쓰며 REMOVE 설정으로 시험): 항목이 있는 프로젝트에서는 아무것도 바꾸지 않고 멈춤, 알파 없는 결과로 실패하면
추가 항목이 지워지고 8 bpc·작업 색공간 없음·linear 끔 설정으로 되돌아감, 이 과정에서 AE 경고 창이 뜨지 않음.
실패 후 작업 색공간을 `"None"`으로 되돌리면 AE가 `Profile "None" is missing` 경고 창을 띄워 스크립트가 멈추던
문제를 찾아, 빈 문자열로 해제하도록 고쳤습니다.

아직 확인하지 않은 것: **Windows의 실제 AE**, 실제 학생 샷, 파일 쓰기 설정이 꺼진 AE, OCIO 프로젝트에서의
멈춤(읽기만 하는 검사), AE 26.3 이외 버전.

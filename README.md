# 🤖 Robot is Future 26-1 — LeRobot SO-ARM 스터디

> 로보틱스 스터디 그룹 | LeRobot + SO-ARM101으로 모방학습(Imitation Learning) 구현

---

## 📦 환경 설치

LeRobot 공식 문서를 기반으로 환경을 구성합니다.

**참고 문서**
- [LeRobot 공식 설치 가이드](https://huggingface.co/docs/lerobot/installation#step-1--conda-only-install-miniforge)
- [로보시지 LeRobot SO-ARM ACT 가이드](https://roboseasy.ai/docs/lerobot-so-arm/policy-act)

### 1. Miniforge 설치 (conda 환경)

```bash
# Miniforge 다운로드 및 설치
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
bash Miniforge3-Linux-x86_64.sh
```

### 2. 가상환경 생성 및 LeRobot 설치

```bash
conda create -n lerobot python=3.10
conda activate lerobot
pip install lerobot
```

---

## ⚙️ 실행 전 필수 설정

USB를 다시 연결할 때마다 포트 권한이 초기화됩니다. **매번 스크립트 실행 전** 아래 명령을 실행해야 합니다.

```bash
# 카메라 포트 권한 부여
sudo chmod 666 /dev/video*

# 모터 포트 권한 부여 (포트 번호는 환경에 따라 다를 수 있음)
sudo chmod 666 /dev/ttyUSB*
```

> ⚠️ **카메라 위치 고정 주의사항**: 스터디에서 정한 카메라 마운트 위치는 표준 화각입니다.
> 개인 테스트 후에는 반드시 자로 측정하여 원래 위치로 복구해 주세요. 위치가 바뀌면 데이터 오염이 발생합니다.

---

## 📋 스터디 진행 기록

### 3주차 — 환경 세팅 및 트러블슈팅

- chmod 권한 문제 파악 및 대응 절차 정립
- 카메라 마운트 표준 위치 확정

---

### 4주차 — 데이터셋 구축 & 첫 학습 성공 🎉

**주요 성과**

- **50 Episode 데이터셋 구축 완료**: 카메라 2대를 정상 연동하여 *'아이셔 박스 넣기'* Task 데이터 약 50 에피소드 녹화
- **ACT 모델 초기 학습 완료**: 수집 데이터 기반 첫 ACT 모델 학습 성공 (에러 없이 완료)

**트러블슈팅 — Rerun 튕김 현상 해결**

| 문제 | 원인 | 해결 |
|------|------|------|
| 데이터 기록 중 rerun 창이 꺼짐 | 일반 노트북의 USB 대역폭 부족 + 낮은 스펙 | 게이밍 노트북으로 교체 (넉넉한 USB 대역폭 + 외장 GPU 확보) |
| 파이프라인 막힘 | 기존 참고 문서의 한계 | [로보시지 가이드](https://roboseasy.ai/docs/lerobot-so-arm/policy-act)로 레퍼런스 변경 |

---

### 6주차 — 실증 실험 및 원인 분석

**실험 결과**

150개 에피소드로 학습한 모델이 50개 에피소드 초기 모델보다 성능이 저하됨:
- 사탕 집기 성공률 감소
- 동작 속도 눈에 띄게 저하

**원인 분석**

데이터 수집 시 정확도를 높이려고 각 단계마다 멈추며 조종한 것이 원인.
모델이 의도한 궤적뿐 아니라 **'멈추는 동작'과 '느린 속도'까지 그대로 모방**하여 학습.

**해결 방안**

> 다음 데이터 수집 시 **멈춤 없이, 자신감 있고 부드럽게 이어지는 매끄러운 동작**으로 재수집

```
❌ 나쁜 예: 각 단계마다 멈칫멈칫 → 모델도 멈춤을 학습
✅ 좋은 예: 처음부터 끝까지 자연스럽고 연속적인 동작
```

---

## 🔗 관련 링크

- [GitHub 레포지토리](https://github.com/hyunsoopark4/Robot_is_Future-26_1)
- [LeRobot 설치 문서](https://huggingface.co/docs/lerobot/installation)
- [로보시지 ACT 가이드](https://roboseasy.ai/docs/lerobot-so-arm/policy-act)

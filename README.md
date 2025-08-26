# 📌 PetRemote 프로젝트 구조 & 진행 현황 (업데이트 보고)

## 1. 전체 구조 개요

```
데이터셋 (강아지 관절 라벨링)
        ↓
데이터 전처리 (YOLO 포맷 변환, 증강)
        ↓
모델 학습
   ├─ YOLOv8 기반
   │   ├─ 객체 검출 (Bounding Box)
   │   └─ 포즈 추정 (Keypoint)
   │
   └─ ConvNeXt 기반
       ├─ 이미지 분류/특징 추출
       └─ YOLO와 결합하여 보조적 학습
        ↓
모델 성능 평가 (mAP, FPS, 메모리)
        ↓
최적화 (경량화, 증강 튜닝, 앙상블)
        ↓
실시간 추론/서비스 적용
```

* **YOLO** : 빠른 실시간성 확보, GPU 최적화
* **ConvNeXt** : 대규모 데이터에서 강력한 feature extractor, fine-grained feature 보정

---

## 2. 진행 현황

### ✅ 데이터 준비

* 강아지 관절 데이터셋 → **YOLO 포맷 변환 완료**
* 학습/검증/테스트 세트 분리

### ✅ Step5: YOLOv8 + Albumentations

* 기본 YOLO 증강 + Albumentations 커스텀 증강 적용
* 포즈 학습 가능 상태 확보

### ✅ Step6: YOLOv8 + MixUp/CopyPaste

* MixUp, CopyPaste 추가 → **학습 안정성 & 일반화 개선**
* 성능 지표:

  * `mAP50(P) ≈ 0.65`
  * `mAP50-95(P) ≈ 0.25`
    → 아직 개선 필요

### ⚠️ Step7 (진행 예정)

* **YOLO + ConvNeXt 병행 학습** 전략 시도

  * ConvNeXt 백본을 이용한 특징 추출 → YOLO 입력 강화
  * 또는 ConvNeXt 분류 결과를 YOLO 출력 후처리에 보조적으로 활용
* **학습 안정화 및 성능 개선**

  * Optimizer 튜닝 (AdamW → SGD+Momentum 비교)
  * Scheduler (cosine annealing warm restarts 등) 적용
  * Batch size/gradient accumulation 실험

---

## 3. 최근 업데이트 (Step6 기준)

* ✅ CUDA 12.6 + PyTorch 2.5.1 환경 정착
* ✅ RTX 4070 GPU 학습 가능 상태 확보
* ✅ YOLOv8 pose 학습이 정상 동작하여 결과(loss/metric) 저장 및 시각화 확인
* ⚠️ TorchVision 연산자(NMS) 관련 이슈 해결 → GPU 학습 정상화

---

## 4. 다음 단계 (Roadmap)

* **Step7:** ConvNeXt 모델 실험 병행

  * ConvNeXt-tiny/ConvNeXt-small 백본 실험
  * YOLOv8-pose vs YOLO+ConvNeXt 성능 비교
* **Step8:** 모델 앙상블/지식 distillation 시도

  * YOLO 빠른 추론 + ConvNeXt 세밀 보정 결합
* **Step9:** 최종 모델 선정 (속도·정확도 균형)
* **Step10:** 실시간 추론 데모 제작 (영상 입력 → 포즈 출력)

---

현재 **YOLOv8 단독 학습을 안정화한 시점**이고,
곧 **ConvNeXt를 병행하여 포즈 검출 성능을 끌어올리는 단계(Step7)** 로 들어가는 게 최근 업데이트 방향입니다.

---


.venv\Scripts\activate
.venv\Scripts\deactivate

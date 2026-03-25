# 중증도 분류에 따른 손동작 의도 파악 앱

# Hand Movement Intention Recognition App Based on Disability Severity Classification

---

## 프로젝트 개요 | Project Overview

뇌졸중, 외상성 뇌 손상 등으로 인해 손 운동 기능을 상실한 마비 환자를 위한 **생체신호 기반 손 운동 의도 분류 시스템**입니다. 환자의 상지 기능 수준(경증 / 중등도 / 중증)에 따라 서로 다른 생체신호(EMG, EEG-MI, EEG-SSVEP)를 활용하여 손을 쥐고 펴는 동작의 의도를 실시간으로 분류합니다.

A **biosignal-based hand movement intention classification system** for paralysis patients who have lost hand motor function due to stroke or traumatic brain injury. The system classifies hand grasp/release intentions in real time using different biosignals (EMG, EEG-MI, EEG-SSVEP) according to the patient's upper limb function level (mild / moderate / severe).

---

## 목표 | Objectives

- 환자의 장애 수준에 맞는 생체신호를 선택적으로 활용하여 손 운동 의도를 분류
- EEG, EMG, SSVEP 신호의 실시간 획득, 전처리, 분석, 시각화를 통합 수행하는 MATLAB ToolBox 개발
- SVM 기반 분류 모델을 통한 Fist(주먹) / Palm(보) 동작 의도 판별
- 신경 가소성(neuroplasticity) 촉진을 통한 재활 효과 극대화

- Classify hand movement intentions using biosignals tailored to the patient's disability level
- Develop a MATLAB ToolBox that integrates real-time acquisition, preprocessing, analysis, and visualization of EEG, EMG, and SSVEP signals
- Distinguish Fist / Palm movement intentions via SVM-based classification
- Maximize rehabilitation outcomes by promoting neuroplasticity

---

## 장애 수준별 신호 매핑 | Signal Mapping by Disability Level

| 수준 (Level) | 상태 (Condition) | 사용 신호 (Signal) |
|---|---|---|
| **Level 1** – 경증 (Mild) | 뇌 손상 경미, 손동작 가능 | **EMG** (근전도) |
| **Level 2** – 중등도 (Moderate) | 뇌 손상 있음, 손동작 불가 | **EEG – Motor Imagery (MI)** |
| **Level 3** – 중증 (Severe) | 뇌 손상 심함, 손동작 불가 | **EEG – SSVEP** |

---

## 시스템 구조 | System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    MATLAB ToolBox App                    │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │              시작화면 (Start Screen)              │    │
│  │   Level 1 (EMG) │ Level 2 (MI) │ Level 3 (SSVEP)│    │
│  └────────┬────────┴──────┬───────┴────────┬───────┘    │
│           │               │                │            │
│           ▼               ▼                ▼            │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐    │
│  │   EMG Mode   │ │   MI Mode    │ │  SSVEP Mode  │    │
│  └──────┬───────┘ └──────┬───────┘ └──────┬───────┘    │
│         │                │                 │            │
│         ▼                ▼                 ▼            │
│  ┌─────────────────────────────────────────────────┐    │
│  │             공통 파이프라인 (Common Pipeline)      │    │
│  │                                                  │    │
│  │  1. Signal Acquisition (신호 획득)                │    │
│  │     └─ Train Record / Test Record                │    │
│  │                                                  │    │
│  │  2. Preprocessing (전처리)                        │    │
│  │     ├─ EMG: BPF → Rectification → Smoothing     │    │
│  │     │       → Normalization                      │    │
│  │     └─ EEG: BPF (1-40Hz) → CAR → Down Sampling  │    │
│  │                                                  │    │
│  │  3. Feature Extraction (특징 추출)                │    │
│  │     ├─ EMG:   RMS, MPF                           │    │
│  │     ├─ MI:    Band Power (α, β), ERD/ERS         │    │
│  │     └─ SSVEP: FFT Power (target freq ± 0.5Hz)   │    │
│  │                                                  │    │
│  │  4. Classification (분류)                         │    │
│  │     └─ SVM (Support Vector Machine)              │    │
│  │        → Fist / Palm 의도 판별                    │    │
│  │                                                  │    │
│  │  5. Real-time Feedback (실시간 피드백)             │    │
│  │     └─ 정확도 표시 & 결과 시각화                   │    │
│  └─────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
```

---

## 실험 장비 | Equipment

| 장비 (Equipment) | 용도 (Purpose) |
|---|---|
| **CGX QUICK-20r** | EEG 측정 (26ch, 국제 10-20 시스템) |
| **Noraxon EMG System** | EMG 측정 (굴근/신근) |
| **PsychoPy** | 시각 자극 제시 (실험 패러다임) |
| **MATLAB App Designer** | ToolBox UI 개발 |
| **EEGLAB** | EEG 오프라인 신호 처리 |

---

## 실험 패러다임 | Experimental Paradigm

```
Rest (4s) → Fist (4s) → Rest (4s) → Palm (4s)   ×15 sets
```

- **MI**: 손동작 이미지/텍스트 제시 → 운동 상상
- **SSVEP**: 주파수 깜빡임 자극 (Fist: 15Hz, Palm: 20Hz)
- **EMG**: 실제 손 운동 수행 후 근전도 측정
- 피실험자: 뇌 질환 병력 없는 건강한 20대 3명

---

## 팀 구성 | Team

| 역할 (Role) | 이름 (Name) | 담당 (Responsibility) |
|---|---|---|
| 팀장 | 강채린 | EEG 신호 측정, ToolBox 제작 |
| 팀원 | 한승욱 | EEG 신호 측정, ToolBox 제작 |
| 팀원 | 이상민 | EEGLAB 신호처리, ToolBox 제작 |
| 팀원 | 장우진 | 머신러닝, SVM |
| 팀원 | 김재강 | EMG 신호 측정 |
| 팀원 | 최준혁 | EMG 데이터 신호 처리, ToolBox 제작 |
| 팀원 | 주하영 | EMG 논문 조사, ToolBox 제작 |

---

## 기술 스택 | Tech Stack

`MATLAB` · `EEGLAB` · `SVM` · `PsychoPy` · `LSL (Lab Streaming Layer)` · `App Designer`

---

## 라이선스 | License

이 프로젝트는 국립금오공과대학교 RIS사업 산학연계PBL 과제로 수행되었습니다.

This project was conducted as part of the RIS Industry-Academia Capstone Design program at Kyungpook National University.

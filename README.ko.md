# aidlc-karpathy-extension

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

AI-DLC 확장으로, Andrej Karpathy의 코딩 원칙(단순함, 최소 변경, 목표 기반 실행)을 코드 생성 단계에서 블로킹 제약 조건으로 적용합니다.

## 이게 뭔가요?

이 확장은 두 가지 접근법을 결합합니다:

- **[AI-DLC](https://github.com/awslabs/aidlc-workflows)** -- AI 기반 소프트웨어 개발을 위한 구조화된 적응형 워크플로우 방법론
- **[Andrej Karpathy의 코딩 원칙](https://github.com/forrestchang/andrej-karpathy-skills)** -- LLM 코딩 시 흔히 발생하는 실패 패턴을 행동 규칙으로 체계화한 것

결과물은 10개의 블로킹 규칙(KARPATHY-01 ~ KARPATHY-10)으로, AI-DLC 워크플로우에 옵트인 확장으로 통합되어 Construction 단계에서 코드 품질 규율을 강제합니다.

## 규칙 요약

| 규칙 | 제목 | 원칙 |
|---|---|---|
| KARPATHY-01 | 가정 명시 | 코딩 전에 생각하기 |
| KARPATHY-02 | 복잡성에 반박 | 코딩 전에 생각하기 + 단순함 |
| KARPATHY-03 | 투기적 기능 금지 | 단순함 우선 |
| KARPATHY-04 | 최소 추상화 | 단순함 우선 |
| KARPATHY-05 | 최소 변경만 | 수술적 변경 |
| KARPATHY-06 | 브라운필드 범위 규율 | 수술적 변경 |
| KARPATHY-07 | 목표 기반 검증 | 목표 기반 실행 |
| KARPATHY-08 | 반복적 수렴 | 목표 기반 실행 |
| KARPATHY-09 | 테스트 우선 버그 수정 | 목표 기반 실행 |
| KARPATHY-10 | 측정 가능한 완료 | 목표 기반 실행 |

## 설치

`extensions/coding/karpathy-principles/` 디렉토리를 AI-DLC 규칙 구조에 복사하세요:

```
aidlc-rules/
└── aws-aidlc-rule-details/
    └── extensions/
        ├── coding/
        │   └── karpathy-principles/
        │       ├── karpathy-principles.md          # 전체 규칙 (옵트인 시 로드)
        │       └── karpathy-principles.opt-in.md   # 옵트인 프롬프트
        ├── security/
        │   └── baseline/
        └── testing/
            └── property-based/
```

`core-workflow.md` 수정은 필요 없습니다. AI-DLC가 워크플로우 시작 시 `extensions/` 디렉토리를 재귀적으로 스캔하여 자동으로 발견합니다.

## 작동 방식

1. 워크플로우 시작 시, AI-DLC가 `extensions/`를 스캔하고 `karpathy-principles.opt-in.md`를 로드
2. Requirements Analysis 단계에서 사용자에게 이 확장 활성화 여부를 질문
3. 활성화되면 `karpathy-principles.md`가 로드되고 10개 규칙이 블로킹 제약 조건이 됨
4. 이후 각 적용 가능한 단계에서 진행 전 준수 여부를 검증

## 옵트인 옵션

- **A) Yes** -- 모든 규칙 적용 (브라운필드/유지보수에 권장)
- **B) Partial** -- 단순함 + 수술적 변경 규칙만, 목표 기반 실행은 건너뜀
- **C) No** -- 모든 규칙 건너뜀 (빠른 프로토타이핑용)

## 크레딧

- [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) (MIT-0 License)
- [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) (MIT License)

## 라이선스

MIT

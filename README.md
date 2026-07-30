# Vision Model Playground

사전학습된 비전 모델(diffusion inpainting, SAM 등)을 직접 실행하며
동작 방식과 한계를 관찰하는 실험 저장소입니다.

체계적인 딥러닝 기초 학습은 별도 저장소
([ai-grad-prep](https://github.com/while-true-study/ai-grad-prep))에서 진행하며,
이 저장소는 특정 문제를 빠르게 탐색해보는 것을 목적으로 합니다.

## 실험 목록

| # | 주제 | 내용 | 노트북 |
|---|------|------|--------|
| 01 | Diffusion Inpainting | 사전학습 모델(SD 1.5)로 마스크 영역을 재생성하고 경계·프롬프트 의존성 관찰 | [notebook](notebooks/01_inpainting.ipynb) |
| 02 | SAM Segmentation | 사전학습 SAM으로 포인트 기반 객체 마스크 추출, 포인트 위치·신뢰도 변화 관찰 | [notebook](notebooks/02_segmentation_sam.ipynb) |

## 관찰 메모

### 01. Diffusion Inpainting
- 사전학습된 Stable Diffusion 1.5 inpainting 파이프라인으로, 이미지의 특정 영역을
  마스크로 지정해 다시 생성하는 과정을 실행했다. (학습 없이 추론만 수행)
- 마스크 경계가 원본과 어색하게 이어지는 아티팩트가 관찰되었다.
- 같은 마스크라도 프롬프트와 주변 맥락에 따라 생성 결과가 크게 달라졌다.
  (예: 특정 사물을 지시했으나 주변 형태에 이끌려 다른 형상으로 채워짐)
- 사전학습 모델을 그대로 쓰면 특정 도메인에서는 부자연스러운 결과가 나오기 쉬우며,
  도메인에 맞춘 파인튜닝이 필요하다는 점을 확인했다.

### 02. SAM Segmentation
- 사전학습된 SAM(`facebook/sam-vit-base`)으로 포인트 프롬프트 기반 객체 마스크를
  추출했다. (학습 없이 추론만 수행)
- 지정하는 포인트의 위치에 따라 분할 대상이 크게 바뀌었다.
  (같은 이미지에서 포인트를 옮기자 서로 다른 객체/영역이 선택됨)
- SAM이 후보 마스크별로 함께 출력하는 신뢰도(IoU score)가 포인트 위치에 따라
  달라졌으며, 경계가 애매한 위치일수록 낮게 나오는 경향을 보였다.
- 경계가 뚜렷한 일반 객체에는 잘 동작하지만, 경계가 애매하거나 대상이
  불분명한 케이스에서는 결과가 흔들릴 수 있음을 확인했다.

## 환경

- Google Colab (T4 GPU)
- Python 3.12, PyTorch, diffusers, transformers

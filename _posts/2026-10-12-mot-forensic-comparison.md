---
title: "[논문] 디지털 포렌식 영상 분석을 위한 다중 객체 추적 기법의 비교 분석"
date: 2026-10-12 09:00:00 +0900
categories: [논문/컨퍼런스, conference, MOT, Digital Forensics]
tags: [MOT, Multi-Object Tracking, Digital Forensics, Video Forensics, ByteTrack, OC-SORT, StrongSORT, BoT-SORT, MOT17]
---

## 0. 논문 정보

- **제목**: 디지털 포렌식 영상 분석을 위한 다중 객체 추적 기법의 비교 분석
- **영문 제목**: A Comparative Analysis of Multi-Object Tracking Methods for Digital Forensic Video Analysis
- **저자**: 김경빈\*, 배나현\*, 지효빈\*, 김기윤\*\*
- **소속**: 국립부경대학교 (학부생\*, 교수\*\*)
- **핵심 키워드**: Multi-Object Tracking (MOT), Digital Video Forensics, Low-Resolution Surveillance Video, IDR / IDP / IDSW / ML
- **원문 PDF**: [다운로드](/assets/pdf/2026-mot-forensic-comparison.pdf)

학부 연구로 진행한 논문의 전문을 기록으로 남깁니다.

---

## 요약

디지털 포렌식 영상 분석에서 다중 객체 추적은 인물의 이동 경로와 동일성을 파악하는 핵심 기술이며, 사건 후 영상을 분석하므로 정확성이 중요하다. 다중 객체 추적 기법은 다양하게 개발되고 있으나, 주요 기법의 성능은 대부분 고해상도 공개 데이터셋에서 보고되어 있어, 수사에서 다루는 저해상도 감시 영상에서도 같은 성능을 기대할 수 있는지는 알기 어렵다. 이에 따라 본 논문에서는 주요 다중 객체 추적 기법의 공개 성능을 증거 장면의 누락, 다른 인물과의 오인, 끊긴 동선을 수사관이 직접 이어야 하는 검토 부담 등 포렌식 관점의 지표로 재구성하고, 사람이 만든 정답이 있는 공개 영상을 감시 영상 수준의 저해상도로 줄여 비교·검증한다. 그리고 검증된 결과를 토대로 영상 포렌식 도구에 필요한 지표를 확립하고 디지털 포렌식 수사에 적합한 영상 포렌식 도구 설계 방안을 제안한다.

---

## I. 서론

디지털 포렌식 수사에서는 CCTV, 스마트폰, 착용형 카메라 등에서 생성되는 영상 증거를 사건 상황과 특정 인물 또는 객체의 이동을 파악하는 중요 자료로 활용한다[1]. 영상 속 여러 인물의 이동 경로와 동일성을 프레임 간에 연속적으로 파악하려면 각 객체의 식별자(ID)를 유지하며 추적하는 다중 객체 추적(Multi-Object Tracking, MOT) 기술이 필요하다[2].

사건 후 녹화 영상을 분석하는 포렌식에서는 처리 속도보다 수사 판단의 오류를 줄이는 것이 중요하다. 그러나 DeepSORT[3] 이후 제안된 MOT 기법들의 공개 성능은 고해상도 영상 중심의 벤치마크에서 종합 점수로 제시된다. 따라서 공개 성능만으로는 오류 유형을 구분할 수 없고, 인물이 작게 기록되는 저해상도 감시 영상에서도 같은 순위가 유지되는지 알기 어렵다.

본 연구에서는 ByteTrack[5], OC-SORT[6], StrongSORT[7], BoT-SORT-ReID[8]를 대상으로 공개 성능을 누락·오인·검토 부담에 대응하는 지표로 재구성하고, 사람이 만든 정답이 있는 영상을 저해상도로 줄여 같은 지표를 직접 측정한다. 이를 통해 해상도 저하가 각 오류 유형에 미치는 영향과 공개 성능 순위의 유지 여부를 확인하고, 영상 포렌식 도구가 추적 기법을 선정·평가할 때 확인해야 할 지표와 설계 방향을 제시한다.

---

## II. 관련 연구

### 2.1 포렌식 영상과 다중 객체 추적

Javed 등은 디지털 영상 포렌식 연구를 정리하여 저화질 영상과 대량의 영상 데이터가 분석의 주요 과제임을 지적하였다[4]. 다중 객체 추적(MOT)은 매 프레임의 검출 결과를 기존 궤적과 짝지어 같은 객체에 같은 ID를 이어 붙이는 기술로[2], 연결이 끊기면 증거 장면이 누락되고 잘못 이어지면 다른 인물로 오인된다.

### 2.2 다중 객체 추적 기법

Wojke 등은 외형 특징을 객체 연관에 함께 쓰는 DeepSORT를 제안해, 가림 뒤에도 객체의 동일성을 유지하여 ID 전환을 45% 줄였다[3]. 이후 기법은 쓰는 정보에 따라 두 계열로 나뉜다. 움직임 정보만 쓰는 계열에서 Zhang 등의 ByteTrack은 신뢰도가 낮은 검출도 궤적과 한 번 더 짝지어 가림으로 약하게 검출된 인물의 누락을 줄였고[5], Cao 등의 OC-SORT는 가림 뒤 다시 관측된 위치로 예측 오차를 보정해 궤적이 끊기는 것을 줄였다[6]. 외형 특징을 함께 쓰는 계열에서 Du 등의 StrongSORT는 재식별(Re-ID) 특징과 연관 방식을 개선해 다른 인물과의 오인을 줄였다. 여기에 끊어진 궤적을 사후에 다시 잇는 후처리(AFLink·GSI)를 더한 StrongSORT++는 수사관이 직접 이어야 할 동선을 줄였다[7]. Aharon 등의 BoT-SORT-ReID는 카메라 움직임을 보정하고 Re-ID 특징을 함께 써 움직이는 카메라에서도 ID를 유지하였다[8].

### 2.3 추적 기법 비교 연구

추적 기법은 주로 Dendorfer 등이 정리한 MOTChallenge 벤치마크[9]에서 같은 영상과 지표로 비교된다. 그러나 Alikhanov 등은 기성 검출기를 쓰는 실제 조건에서 입력 해상도와 검출 신뢰도 임곗값만 조정해도 HOTA가 10%에서 최대 30%까지 달라지고, 하나의 임곗값만으로는 추적기 순위를 확정할 수 없음을 보였다[10]. 이는 공개 벤치마크의 결과를 저화질 포렌식 영상에 그대로 옮기기 어려움을 뜻하며, 이에 본 연구는 운영 해상도인 240p에서 같은 검출 결과로 네 기법을 다시 비교한다.

---

## III. 추적 기법 성능 비교 분석

### 3.1 비교 대상 선정 및 평가 기준

본 연구는 객체 ID 유지 성능을 증거 장면의 누락, 다른 인물과의 오인, 수사관의 검토 부담으로 나누어 평가한다.

- **누락**: 정답 인물이 나타난 프레임 중 올바른 ID로 이어진 비율인 **IDR**[11]과, 등장 프레임의 20% 미만만 추적되어 사실상 결과에서 빠진 인물의 비율인 **ML**[9]로 본다.
- **오인**: 한 ID로 묶인 프레임 중 실제로 같은 인물인 비율인 **IDP**[11]로 측정한다.
- **검토 부담**: 수사관이 끊긴 동선을 직접 이어야 하는 ID 전환(**IDSW**) 횟수[12]로 측정한다.

비교 대상은 MOT17 공개 벤치마크(고해상도)에서 성능이 검증된 기법[13] 가운데, 움직임만 쓰는 ByteTrack·OC-SORT와 외형 특징을 함께 쓰는 StrongSORT·BoT-SORT-ReID를 골랐다. 실험 해상도는 UCF-Crime[14], UCSD Ped2[15] 등 CCTV 공개 자료 다수가 240p 내외이므로 세로 240픽셀로 정하였다.

### 3.2 추적 기법 비교

\<Table 1\>의 공개 결과[13]에서는 BoT-SORT-ReID가 IDR과 IDP에서, 후처리를 더한 StrongSORT++가 IDSW와 ML에서 가장 우수하였다. 그러나 공개 결과는 고해상도 영상과 MOT17로 학습한 검출기를 전제로 한다. 240p에서는 StrongSORT가 IDR(27.5%)과 IDP(81.1%)에서, BoT-SORT-ReID가 IDSW(197회, 다른 기법 231∼335회)에서 가장 우수하여, 공개 결과와 마찬가지로 모든 지표에서 앞서는 기법은 없었다. ML은 공개 결과에서 13.9∼20.9%였으나 240p에서는 네 기법 모두 약 50%(49.8∼51.3%)로 늘었다. 인물 단위의 누락은 기법 간 차이(1.5%p)보다 해상도의 영향을 훨씬 크게 받은 것이다.

| 기법 | 구분 | IDR (%) | IDP (%) | IDSW (회) | ML (%) |
|:---|:---|---:|---:|---:|---:|
| ByteTrack | 공개 | 73.3 | 81.7 | 2,196 | 14.5 |
| | 240p | 26.4 | 76.2 | 335 | 49.8 |
| OC-SORT | 공개 | 71.2 | 85.0 | 1,950 | 20.9 |
| | 240p | 26.0 | 80.1 | 231 | 51.3 |
| StrongSORT | 공개(++) | 75.4 | 84.1 | 1,194 | 13.9 |
| | 240p | 27.5 | 81.1 | 307 | 49.8 |
| BoT-SORT-ReID | 공개 | 75.7 | 85.3 | 1,212 | 16.2 |
| | 240p | 26.8 | 80.0 | 197 | 50.2 |

_\<Table 1\> Public MOT17 results and measured results at 240p_

![MOT17-04 원본 해상도와 240p에서의 네 기법 추적 결과 비교](/assets/img/mot-forensic-fig1.png)
_Figure 1. Tracking results on MOT17-04 at the original resolution and 240p_

\<Figure 1\>은 고정 카메라 영상(MOT17-04)의 같은 프레임을 원본(위)과 240p(아래)로 비교한 것으로, 빨간 상자는 같은 인물에 새 ID가 붙어 동선이 끊긴 경우이다. 240p에서는 인물 높이가 중앙값 172픽셀에서 38픽셀로 줄어, 이 프레임의 정답 인물 48명 중 추적된 인물이 원본의 7∼11명에서 6∼9명으로 줄었다(7편 전체 IDR 감소는 1.8∼2.8%p). 끊긴 동선은 수사관이 직접 이어야 하는데, 240p에서 새 ID는 ByteTrack 3개, StrongSORT 2개, OC-SORT 1개였고 BoT-SORT-ReID는 없었다. 고정 카메라 3편 전체에서도 BoT-SORT-ReID가 오인(IDP 84.7%)과 검토 부담(IDSW 50회) 모두 가장 적었으나, 이동 카메라 4편에서는 StrongSORT의 IDP(80.7%)가 가장 높아(BoT-SORT-ReID 75.8%), 고정 카메라와 이동 카메라 영상에서 적합한 기법이 달라진다.

### 3.3 종합 평가

사후 처리가 가능한 포렌식 분석에서는 후처리 모듈의 구성만으로도 수사관의 검토 부담이 달라진다. 공개 결과에서 후처리가 없는 StrongSORT의 IDSW는 1,446회로 BoT-SORT-ReID(1,212회)보다 많았지만, 후처리를 더한 StrongSORT++는 1,194회로 가장 적었다[7]. 240p 실험에서도 누락·오인이 가장 적은 기법(StrongSORT)과 검토 부담이 가장 적은 기법(BoT-SORT-ReID)이 달랐고, 오인이 가장 적은 기법은 촬영 환경에 따라 바뀌어 모든 기준에서 앞서는 기법은 없었다.

가장 큰 위험은 누락이다. 같은 검출기를 쓴 원본 해상도에서도 출력은 정답 상자의 33~35%에만 대응되어 IDR은 28~30%, ML은 44.0~46.5%에 그쳤다. 이는 240p에서의 기법 간 IDR 차이(1.5%p)나 해상도를 낮춘 영향(IDR 2~3%p)보다 훨씬 커서, 성능을 가장 크게 제한한 것은 추적 기법이 아니라 검출기의 재현율이었다. 따라서 추적 결과에 나타나지 않은 인물을 영상에 없는 인물로 판단해서는 안 된다.

이 결과는 포렌식 영상 분석 도구가 하나의 추적 기법에 고정되기보다, 촬영 환경과 수사 목적에 따라 검출·추적·후처리 모듈을 교체·조합할 수 있도록 설계되어야 함을 시사한다.

---

## IV. 결론

본 연구는 주요 다중 객체 추적 기법을 포렌식 관점의 지표로 비교하여, 공개 평가값과 저해상도 재현 실험 모두에서 단일 기법이 모든 조건을 만족하기 어렵고 촬영 환경과 모듈 구성에 따라 성능이 달라짐을 확인하였다. 또한 기법 간 차이나 해상도의 영향보다 검출기의 재현율이 성능을 더 크게 제한함을 확인하였다.

향후 연구에서는 이를 바탕으로 검출, 추적, 후처리 단계를 모듈 단위로 교체·조합할 수 있는 포렌식 영상 분석 프레임워크를 구현하고, 저해상도에 강한 검출 모듈과 압축·낮은 프레임률 등 실제 감시 영상의 열화 조건, 실제 수사 영상으로 검증 범위를 넓힐 계획이다.

---

## 참고문헌

1. Geradts, Z., van Lierop, S., and Kombrink, M., "Interpol review of forensic image and video analysis, 2022-2025," *Forensic Sci. Int.: Synergy*, vol. 13, 100697, 2026.
2. Guan, Z., et al., "Multi-object tracking review: retrospective and emerging trend," *Artif. Intell. Rev.*, vol. 58, no. 8, 235, 2025.
3. Wojke, N., Bewley, A., and Paulus, D., "Simple online and realtime tracking with a deep association metric," *Proc. IEEE ICIP*, pp. 3645-3649, 2017.
4. Zhang, Y., et al., "ByteTrack: Multi-object tracking by associating every detection box," *Proc. ECCV*, pp. 1-21, 2022.
5. Cao, J., et al., "Observation-centric SORT: Rethinking SORT for robust multi-object tracking," *Proc. IEEE/CVF CVPR*, pp. 9686-9696, 2023.
6. Du, Y., et al., "StrongSORT: Make DeepSORT great again," *IEEE Trans. Multimedia*, vol. 25, pp. 8725-8737, 2023.
7. Aharon, N., Orfaig, R., and Bobrovsky, B. Z., "BoT-SORT: Robust associations multi-pedestrian tracking," *arXiv:2206.14651*, 2022.
8. Javed, A. R., et al., "A comprehensive survey on digital video forensics: Taxonomy, challenges, and future directions," *Eng. Appl. Artif. Intell.*, vol. 106, 104456, 2021.
9. Dendorfer, P., et al., "MOTChallenge: A benchmark for single-camera multiple target tracking," *Int. J. Comput. Vis.*, vol. 129, pp. 845-881, 2021.
10. Alikhanov, J., et al., "Practical evaluation framework for real-time multi-object tracking: Achieving optimal and realistic performance," *IEEE Access*, vol. 13, pp. 34768-34788, 2025.
11. Ristani, E., et al., "Performance measures and a data set for multi-target, multi-camera tracking," *Proc. ECCV Workshops*, pp. 17-35, 2016.
12. Bernardin, K. and Stiefelhagen, R., "Evaluating multiple object tracking performance: The CLEAR MOT metrics," *EURASIP J. Image Video Process.*, vol. 2008, 246309, 2008.
13. MOTChallenge, "MOT17 results," <https://motchallenge.net/results/MOT17/> (accessed Oct. 8, 2026).
14. Sultani, W., Chen, C., and Shah, M., "Real-world anomaly detection in surveillance videos," *Proc. IEEE/CVF CVPR*, pp. 6479-6488, 2018.
15. Mahadevan, V., Li, W., Bhalodia, V., and Vasconcelos, N., "Anomaly detection in crowded scenes," *Proc. IEEE CVPR*, pp. 1975-1981, 2010.

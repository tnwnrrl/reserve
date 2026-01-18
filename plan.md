# Reserve Audio Analyzer - 개선 계획

## 완료된 작업 (2026-01-18)

- [x] scipy.signal import 최적화 (600ms → 0.3ms/frame)
- [x] glow effect 3중 렌더링 → 단일 라인
- [x] Matplotlib blitting 적용 (5x 성능 향상)
- [x] 애니메이션 인터벌 50ms → 100ms
- [x] bare except → OSError 명시적 예외 처리
- [x] tempfile import 파일 상단 이동
- [x] 오디오 로드 에러 메시지 상세화

## 남은 작업

### 🟢 Low Priority - 코드 정리 ✅ 완료

- [x] **Constants 추출**
  - Magic numbers를 상수로 정리
  - `DISPLAY_SAMPLES = 4000`
  - `WINDOW_SIZE_MS = 2000`
  - `ANIMATION_INTERVAL_MS = 100`
  - `PLAYBACK_STEP_MS = 100`
  - `FFT_SIZE = 16384`
  - `SAVGOL_WINDOW = 51`
  - `SPEED_MIN/MAX/DEFAULT = 5/20/10`

- [x] **미사용 import 제거**
  - `audio_processor.py` - `pydub.playback.play` 제거됨

### 🟡 Medium Priority - 구조 개선 ✅ 완료

- [x] **설정 파일 분리** (`config.py`)
  - `Colors` 클래스 (GREEN_BRIGHT, YELLOW, RED 등)
  - `Fonts` 클래스 (TITLE_FAMILY, MONO_FAMILY 등)
  - `Layout` 클래스 (WINDOW_WIDTH, CONTROL_PANEL_WIDTH 등)
  - `get_stylesheet()` 함수 - QSS 동적 생성
  - Display/Animation/FFT 상수 통합

- [x] **시각화 코드 분리** (`visualization.py`)
  - `MplCanvas` 클래스
  - `style_scope_axis()` - 오실로스코프 스타일 축
  - `prepare_waveform_samples()` - 파형 샘플 준비
  - `compute_spectrum()` - FFT 스펙트럼 계산
  - `draw_waveform_static()` / `draw_spectrum_static()` - 정적 렌더링
  - `WaveformAnimator` 클래스 - 블리팅 애니메이션

- [x] **qt_scope.py 리팩토링**
  - config/visualization 모듈 import
  - Colors 상수 사용 (하드코딩 색상 제거)
  - numpy import 제거 (visualization으로 이동)
  - WaveformAnimator 통합

### 🔵 Optional - 기능 추가

- [ ] **오디오 내보내기 UI**
  - 역재생 오디오 저장 버튼 추가
  - WAV/MP3 포맷 선택

- [ ] **키보드 단축키**
  - Space: 재생/일시정지
  - R: 역재생
  - O: 파일 열기

- [ ] **드래그 앤 드롭**
  - 오디오 파일 드래그로 로드

### 🧪 Testing

- [ ] **Unit Test 추가**
  - `tests/test_audio_processor.py`
  - load_audio, reverse_audio, change_speed 테스트

## 성능 벤치마크 결과

| 항목 | 개선 전 | 개선 후 | 개선율 |
|------|---------|---------|--------|
| savgol_filter | 600ms/frame | 0.3ms/frame | 2000x |
| Animation render | 13.8ms/frame | 2.7ms/frame | 5.1x |
| Glow effect | 3 draw calls | 1 draw call | 3x |

## 아키텍처 점수

| 항목 | 점수 | 목표 |
|------|------|------|
| Quality | 75/100 | 85/100 |
| Security | 85/100 | 90/100 |
| Performance | 90/100 | 95/100 |
| Architecture | 70/100 | 80/100 |

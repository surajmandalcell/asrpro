# ASR Pro Test Summary

## Models Tested
- `whisper-tiny` (39M) - English/Hindi
- `whisper-base` (74M) - English/Hindi  
- `whisper-small` (244M) - English/Hindi

## Test Suite
- **Real Audio**: `tests/audio_for_test.mp3` (2:37)
- **Basic**: Silence test
- **Edge Cases**: Short (1s), Long (10s)
- **Performance**: 1s, 5s durations
- **Total**: 7 tests per model

## Usage
```bash
python main.py --test
```

## Results
- **Success Rate**: 100% (3/3 models)
- **Duration**: ~78s total
- **Outputs**: Auto-cleaned before each run

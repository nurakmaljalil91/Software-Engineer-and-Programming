---
title: Create Video Translator using Python & Whisper AI
category: python
tags:
  - python
created: 2026-03-28
updated: 2026-03-28
status: active
---
## Japanese-to-English Subtitle Generator

### Prerequisites  

- [[Python]] 3.7 or later  
- NVIDIA GPU (e.g. RTX 2060) with [[CUDA]] drivers installed  
- [[FFmpeg]] installed and on your PATH  

### Setup  

- Open a terminal in your project directory  
- Create and activate a virtual environment  
 
```bash
python -m venv .venv
```  
  
  - Windows: 
  
```bash
.\.venv\Scripts\activate
```  
  
  - macOS/Linux: 
  
```
source .venv/bin/activate
```

- Install CUDA-enabled [[PyTorch]]  
  
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117
```

- Install the remaining Python packages  
  
```bash
pip install openai-whisper deep-translator tqdm srt
```

### Create the script  

- Create a file named `video_translator.py`  
- Paste the following into it:

```python
import whisper
import torch
from deep_translator import GoogleTranslator
import srt
import datetime
from tqdm import tqdm
import time

def translate_with_retry(text: str, retries: int = 3, delay: float = 1.0) -> str:
    for attempt in range(1, retries + 1):
        try:
            return GoogleTranslator(source='ja', target='en').translate(text)
        except Exception as e:
            print(f"  ⚠️ Translation attempt {attempt} failed: {e}")
            if attempt < retries:
                time.sleep(delay)
    return "[Translation error]"

def transcribe_and_translate(video_path: str, srt_path: str):
    device = "cuda" if torch.cuda.is_available() else "cpu"
    print(f"Using device: {device}")

    print("Loading Whisper model…")
    model = whisper.load_model("small").to(device)

    print("Transcribing audio → text…")
    t0 = time.time()
    result = model.transcribe(video_path, language="ja", fp16=(device!="cpu"))
    t1 = time.time()
    segments = result["segments"]
    print(f"  ↳ Done in {(t1-t0)/60:.1f} min, {len(segments)} segments\n")

    subs = []
    print("Translating segments to English:")
    for i, seg in enumerate(tqdm(segments, unit="seg"), start=1):
        start = datetime.timedelta(seconds=seg["start"])
        end   = datetime.timedelta(seconds=seg["end"])
        ja_text = seg["text"].strip()
        en_text = translate_with_retry(ja_text)

        subs.append(
            srt.Subtitle(index=i, start=start, end=end, content=en_text)
        )

    print("\nWriting .srt file…")
    with open(srt_path, "w", encoding="utf-8") as f:
        f.write(srt.compose(subs))

    print(f"\nSubtitles saved to {srt_path}")

if __name__ == "__main__":
    import argparse
    p = argparse.ArgumentParser(description="Japanese→English .srt generator")
    p.add_argument("video",  help="Input .mp4")
    p.add_argument("output", help="Output .srt")
    args = p.parse_args()
    transcribe_and_translate(args.video, args.output)
```

###  Usage

- Ensure your virtual environment is active
- Run the script with your video and desired SRT 

```bash
python video_translator.py input.mp4 output.srt
```

### Conclusion

- the script will transcribe the Japanese audio on your GPU
- translate each segment with retries
- and write a completed `.srt` file ready for your player
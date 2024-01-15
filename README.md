<div align="center">
<h2>
    WhisperPlus: Advancing Speech-to-Text Processing 🚀
</h2>
<div>
    <img width="500" alt="teaser" src="doc\openai-whisper.jpg">
</div>
<div>
    <a href="https://pypi.org/project/whisperplus" target="_blank">
        <img src="https://img.shields.io/pypi/pyversions/whisperplus.svg?color=%2334D058" alt="Supported Python versions">
    </a>
    <a href="https://badge.fury.io/py/whisperplus"><img src="https://badge.fury.io/py/whisperplus.svg" alt="pypi version"></a>
    <a href="https://huggingface.co/spaces/ArtGAN/WhisperPlus"><img src="https://huggingface.co/datasets/huggingface/badges/raw/main/open-in-hf-spaces-sm.svg" alt="HuggingFace Spaces"></a>
</div>
</div>

## 🛠️ Installation

```bash
pip install whisperplus
```

# 🤗 Model Hub

You can find the models on the [HuggingFace Spaces](https://huggingface.co/spaces/ArtGAN/WhisperPlus) or on the [HuggingFace Model Hub](https://huggingface.co/models?search=whisper)

## 🎙️ Usage

To use the whisperplus library, follow the steps below for different tasks:

### 🎵 Youtube URL to Text

```python
from whisperplus import SpeechToTextPipeline, download_and_convert_to_mp3

# Define the URL of the YouTube video that you want to convert to text.
url = "https://www.youtube.com/watch?v=di3rHkEZuUw"

# Initialize the Speech to Text Pipeline with the specified model.
audio_path = download_and_convert_to_mp3(url)
pipeline = SpeechToTextPipeline(model_id="openai/whisper-large-v3")

# Run the pipeline on the audio file.
transcript = pipeline(
    audio_path=audio_path, model_id="openai/whisper-large-v3", language="english"
)

# Print the transcript of the audio.
print(transcript)
```

### Summarization

```python
from whisperplus.pipelines.summarization import TextSummarizationPipeline

summarizer = TextSummarizationPipeline(model_id="facebook/bart-large-cnn")
summary = summarizer.summarize(transcript)
print(summary[0]["summary_text"])
```

### Speaker Diarization

```python
from whisperplus import (
    ASRDiarizationPipeline,
    download_and_convert_to_mp3,
    format_speech_to_dialogue,
)

audio_path = download_and_convert_to_mp3("https://www.youtube.com/watch?v=mRB14sFHw2E")

device = "cuda"  # cpu or mps
pipeline = ASRDiarizationPipeline.from_pretrained(
    asr_model="openai/whisper-large-v3",
    diarizer_model="pyannote/speaker-diarization",
    use_auth_token=False,
    chunk_length_s=30,
    device=device,
)

output_text = pipeline(audio_path, num_speakers=2, min_speaker=1, max_speaker=2)
dialogue = format_speech_to_dialogue(output_text)
print(dialogue)
```

## Applications

### RAG - Chat with Video

- user can use Whisperplus for converting audio into transcript files &  use it to create chat with video applications
- we are using LanceDB as the default vectordb & open source llm.
- feel free to do experiments with different prompts & llm to get better results.

```python
from whisperplus.pipelines.chatbot import ChatWithVideo

input_file = "trascript.txt"
llm_model_name = "TheBloke/Mistral-7B-v0.1-GGUF"
llm_model_file = "mistral-7b-v0.1.Q4_K_M.gguf"
llm_model_type = "mistral"
embedding_model_name = "sentence-transformers/all-MiniLM-L6-v2"

chat = ChatWithVideo(
    input_file, llm_model_name, llm_model_file, llm_model_type, embedding_model_name
)
query = "what is this video about ?"
response = chat.run_query(query)
print(response)
```

### Contributing

```bash
pip install -r dev-requirements.txt
pre-commit install
pre-commit run --all-files
```

## 📜 License

This project is licensed under the terms of the Apache License 2.0.

## 🤗 Acknowledgments

This project is based on the [HuggingFace Transformers](https://github.com/huggingface/transformers) library.

## 🤗 Citation

```bibtex
@misc{radford2022whisper,
  doi = {10.48550/ARXIV.2212.04356},
  url = {https://arxiv.org/abs/2212.04356},
  author = {Radford, Alec and Kim, Jong Wook and Xu, Tao and Brockman, Greg and McLeavey, Christine and Sutskever, Ilya},
  title = {Robust Speech Recognition via Large-Scale Weak Supervision},
  publisher = {arXiv},
  year = {2022},
  copyright = {arXiv.org perpetual, non-exclusive license}
}
```

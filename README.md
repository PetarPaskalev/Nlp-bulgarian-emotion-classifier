[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/uhJ9rk0N)

# Bulgarian Audio Pipeline

This repository contains a pipeline for processing Bulgarian audio content through multiple stages:
1. Downloading audio from YouTube
2. Transcribing Bulgarian speech
3. Translating text from Bulgarian to English
4. Classifying emotions in the translated text

## Requirements

### Libraries
- Python 3.8+ 
- PyTorch
- TensorFlow
- OpenAI Whisper
- Pandas
- Pytubefix (not pytube)
- Pydub
- Transformers (Hugging Face)
- NumPy

### Models
- Whisper large model (will be downloaded automatically)
- Translation model saved in folder "tf_model" (Helsinki-NLP/opus-mt-bg-en)
- BERT emotion classification model saved in folder "bert_large_8"

### Hardware
- GPU is recommended for faster processing (especially for transcription and emotion classification)
- Minimum 8GB RAM

## Installation

1. Clone this repository
2. Install the required libraries:

```bash
pip install torch tensorflow openai-whisper pandas pytubefix pydub transformers numpy
```

3. Ensure you have ffmpeg installed (required by Whisper and pydub for audio processing):
   
   - On Windows:
     ```
     # Using Chocolatey
     choco install ffmpeg
     ```
   
   - On macOS:
     ```
     # Using Homebrew
     brew install ffmpeg
     ```
   
   - On Linux:
     ```
     sudo apt update
     sudo apt install ffmpeg
     ```

4. Download or prepare the required models:
   - The Whisper model will be downloaded automatically
   - For the translation model:
     ```
     # Download Helsinki-NLP/opus-mt-bg-en and save to tf_model directory
     python -c "from transformers import TFAutoModelForSeq2SeqLM, AutoTokenizer, GenerationConfig; model = TFAutoModelForSeq2SeqLM.from_pretrained('Helsinki-NLP/opus-mt-bg-en'); tokenizer = AutoTokenizer.from_pretrained('Helsinki-NLP/opus-mt-bg-en'); config = GenerationConfig.from_pretrained('Helsinki-NLP/opus-mt-bg-en'); model.save_pretrained('tf_model'); tokenizer.save_pretrained('tf_model'); config.save_pretrained('tf_model')"
     ```
   - You'll need to provide your own BERT emotion classification model in the "bert_large_8" directory

## Usage

1. (Optional) Modify the configuration variables at the top of pipeline.py:
   - YOUTUBE_LINK: URL of the YouTube video to process
   - AUDIO_FILE: Output path for the downloaded audio
   - Other file paths as needed

2. Run the pipeline:
```bash
python pipeline.py
```

The pipeline will:
- Download audio from the specified YouTube link
- Transcribe the Bulgarian audio
- Translate the transcription to English
- Classify emotions in the translated text
- Save the results at each step

## Output Files

The pipeline generates the following files:
- audio.wav: The extracted audio from YouTube
- transcription.csv: Bulgarian transcription with timestamps
- translation.csv: Transcription with added English translations
- pipeline_predictionsv2.csv: Final output with timestamps, Bulgarian text, English translations, and emotion labels

## Limitations

- **BERT Emotion Classification Model**: You need to provide your own emotion classification model in the "bert_large_8" directory. This pipeline does not include instructions for training this model.
- **Resource Intensive**: The Whisper "large" model requires approximately 3GB of disk space and significant RAM to run effectively.
- **YouTube Compatibility**: The pytubefix library may encounter issues if YouTube changes its page structure or API, potentially causing the download step to fail.
- **Language Specificity**: This pipeline is specifically designed for Bulgarian audio input and may not work correctly with other languages.
- **Network Dependencies**: Downloading the models requires a stable internet connection and access to Hugging Face's model repository.
- **Processing Time**: Transcription of long videos can take several hours, especially without GPU acceleration.

## Notes

- The pipeline checks if each output file already exists and skips that step if it does. Delete the respective files if you want to rerun specific steps.
- GPU acceleration is used automatically if available. The pipeline will run on CPU if no GPU is detected.
- For large videos, the processing might take a considerable amount of time, especially the transcription step.

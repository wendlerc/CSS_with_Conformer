# Continuous Speech Separation with Conformer

## Introduction

We examine the use of the Conformer architecture for continuous speech separation. 
Conformer allows the separation model to efficiently capture both local and global context information, which is helpful for speech separation.
Experimental results using the LibriCSS dataset show that the Conformer separation model achieves state of the art results for both single-channel and multi-channel settings.

For a detailed description and experimental results, please refer to our paper: [Continuous Speech Separation with Conformer](https://arxiv.org/abs/2008.05773) (Accepted by ICASSP 2021).

## Environment
python 3.10.9, torch 2.0.1+cu118

## Get Started
1. Download the overlapped speech of [LibriCSS dataset](https://github.com/chenzhuo1011/libri_css).
    
    - from Google drive:
    ```bash
    wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=1PdloA-V8HGxkRu9MnT35_civpc3YXJsT' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=1PdloA-V8HGxkRu9MnT35_civpc3YXJsT" -O overlapped_speech.zip && rm -rf /tmp/cookies.txt && unzip overlapped_speech.zip && rm overlapped_speech.zip
   ```
    - from [Microsoft Azure Storage](https://valle.blob.core.windows.net/share/CSS_with_Conformer/overlapped_speech.zip?sv=2020-08-04&st=2023-03-01T07%3A51%3A05Z&se=2033-03-02T07%3A51%3A00Z&sr=c&sp=rl&sig=QJXmSJG9DbMKf48UDIU1MfzIro8HQOf3sqlNXiflY1I%3D):
    ```bash
    wget "https://valle.blob.core.windows.net/share/CSS_with_Conformer/overlapped_speech.zip?sv=2020-08-04&st=2023-03-01T07%3A51%3A05Z&se=2033-03-02T07%3A51%3A00Z&sr=c&sp=rl&sig=QJXmSJG9DbMKf48UDIU1MfzIro8HQOf3sqlNXiflY1I%3D" -O overlapped_speech.zip && rm -rf /tmp/cookies.txt && unzip overlapped_speech.zip && rm overlapped_speech.zip
   ```
   
2. Download the Conformer separation models.
    
    - from [Microsoft Azure Storage](https://valle.blob.core.windows.net/share/CSS_with_Conformer/checkpoints.zip?sv=2020-08-04&st=2023-03-01T07%3A51%3A05Z&se=2033-03-02T07%3A51%3A00Z&sr=c&sp=rl&sig=QJXmSJG9DbMKf48UDIU1MfzIro8HQOf3sqlNXiflY1I%3D):
    ```bash
    wget "https://valle.blob.core.windows.net/share/CSS_with_Conformer/checkpoints.zip?sv=2020-08-04&st=2023-03-01T07%3A51%3A05Z&se=2033-03-02T07%3A51%3A00Z&sr=c&sp=rl&sig=QJXmSJG9DbMKf48UDIU1MfzIro8HQOf3sqlNXiflY1I%3D" -O checkpoints.zip && rm -rf /tmp/cookies.txt && unzip checkpoints.zip && rm checkpoints.zip
    ```

3. Run the separation.

    3.1  Single-channel separation
    
    ```bash
    export MODEL_NAME=1ch_conformer_base
    python3 separate.py \
        --checkpoint checkpoints/$MODEL_NAME \
        --mix-scp utils/overlapped_speech_1ch.scp \
        --dump-dir separated_speech/monaural/utterances_with_$MODEL_NAME \
        --device-id 0 \
        --num_spks 2
    ```
        
    The separated speech can be found in the directory 'separated_speech/monaural/utterances_with_$MODEL_NAME'
    
    3.2 Seven-channel separation
    
    ```bash
    export MODEL_NAME=conformer_base
    python3 separate.py \
        --checkpoint checkpoints/$MODEL_NAME \
        --mix-scp utils/overlapped_speech_7ch.scp \
        --dump-dir separated_speech/7ch/utterances_with_$MODEL_NAME \
        --device-id 0 \
        --num_spks 2 \
        --mvdr True
    ```
    
    The separated speech can be found in the directory 'separated_speech/7ch/utterances_with_$MODEL_NAME'

## Citation
If you find our work useful, please cite [our paper](https://arxiv.org/abs/2008.05773):
```bibtex
@inproceedings{CSS_with_Conformer,
  title={Continuous speech separation with conformer},
  author={Chen, Sanyuan and Wu, Yu and Chen, Zhuo and Wu, Jian and Li, Jinyu and Yoshioka, Takuya and Wang, Chengyi and Liu, Shujie and Zhou, Ming},
  booktitle={ICASSP 2021-2021 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)},
  pages={5749--5753},
  year={2021},
  organization={IEEE}
}
```

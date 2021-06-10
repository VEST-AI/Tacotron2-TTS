# Tacotron2-TTS

VEST-AI 에게 한국말을 가르치기 위해 진행한 손석희 음성 TTS 구현 project 입니다. 결과는 다음의 링크에서 확인 할 수 있습니다.

 - 시연 영상: https://youtu.be/wQVzcQK5KXk
 - 결과 확인: https://livviee.github.io/sonTTS/


### 실행순서

##### 0. Requirements
  - python 3.7
  - tensorflow-gpu 1.8.0 이상
  - librosa

##### 1. Preprocess
  - 전처리를 위해 필요한 데이터셋은  오디오 wav파일과 text가 담긴 json파일입니다.
  - wav파일을 전처리하여 data/son디렉토리에 npz파일을 생성합니다.(repository에는 datasets/son 에 오디오파일 샘플과, data/son에 전처리된 npz파일 샘플 소량을 첨부하였습니다.)

```
python preprocess.py --num_workers 8 --name son --in_dir ./datasets/son --out_dir ./data/son
```

##### 2. Train
  - 전처리된 학습데이터셋으로 Tacotron2모델을 학습시킵니다. 2000step마다 logdir-tacotron2디렉토리에 학습 log가 생성됩니다.
  - train_tacotron2.py에서 data_path를 지정해줍니다.  ex) parser.add_argument('--data_paths', default='./data/son') 
  - 학습을 이어서 계속 할 경우 load_path를 지정해줍니다. ex) parser.add_argument('--load_path', default='logdir-tacotron2/son_2021-06-02_17-55-47')

```
python train_tacotron2.py
```
##### 3. Synthesize
  - 학습이 완료되면 --load_path를 지정후 원하는 text로 합성된 음성을 생성합니다. 
  - 음성합성결과는 logdir-tacotron2/generate 에 생성됩니다. 

```
python synthesizer.py --load_path logdir-tacotron2/son_2021-06-02_17-55-47 --num_speakers 1 --speaker_id 0 --text "안녕하세요 저희는 김소민 박지현 김민애 입니다."
```

### Results
8GB 가량의 손석희 음성데이터로 20000step의 학습을 진행 후 음성합성한 결과는 다음과 같습니다.
 - 결과 확인 : https://livviee.github.io/sonTTS/

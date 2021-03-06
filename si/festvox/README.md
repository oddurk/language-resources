# Setting up Sinhala TTS

## Festvox Environment

Run the following command (This should be executed while in the cloned language-resources folder)

```
export FESTIVAL_SUIT_PATH=<your desired path to install Festival>
mkdir ${FESTIVAL_SUIT_PATH} && ./third_party/festvox/setup.sh
```

- Make sure to add the environment variables to .bashrc

## Preparing the data

- Lexicon - ```bazel build //si/festvox:make_lexicon_scm```
- Festival prompts -  ```bazel build //si/festvox:make_festvox_prompts```

## Training the Voice

1. Download recordings from 

http://storage.googleapis.com/voice-builder-public-data/examples/sinhala/wavs.tar.gz

  ```
  export WAV_FOLDER=<your desired path to store downloaded wav files>
  mkdir -p ${WAV_FOLDER} && wget http://storage.googleapis.com/voice-builder-public-data/examples/sinhala/wavs.tar.gz && tar xzf wavs.tar.gz -C ${WAV_FOLDER}
  ```

2. Train TTS Voice (This should be executed while in the cloned language-resources folder).
This step will take couple of hours to finish.

  ```
  export OUTPUT_VOICE_FOLDER=<your desired path to store output voice>
  ./festival_utils/build_festvox_voice.sh ${WAV_FOLDER} si ${OUTPUT_VOICE_FOLDER}
  ```

3. Synthesize one example sentence (This always needs to run from the produced output voice directory).

  ```
    cd ${OUTPUT_VOICE_FOLDER}
    echo 'ආයතනයක්වත්' |
    ${FESTIVALDIR}/bin/text2wave \
      -eval festvox/goog_si_unison_cg.scm \
      -eval '(voice_goog_si_unison_cg)' \
      > example.wav
  ```

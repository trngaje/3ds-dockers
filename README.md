# 3ds-dockers

> docker4retroarch

3ds 환경에서 retroarch 를 빌드 하기 위한 3ds 용 기본 라이브러리가 설치되어 있음 

> 3ds_r45

3ds 환경에서 stand alone 인 snes9x 에뮬레이터를 빌드 하기 위한 3ds 용 기본 라이브러리가 설치되어 있음 

> docker4zeldapicross

3ds 환경에서 zelda picross 를 빌드하기위한 라이브러리가 설치되어 있음

- devkitARM r46
- libctru v1.2.0
- citro3d v1.2.0
- zlib
- libjpeg-turbo
- libpng
- sf2dlib
- sfillib

한번은 docker container를 생성해야 함

    ./build_container.sh

미리 생성된 docker container를 실행할 수 있음

    ./run_container.sh

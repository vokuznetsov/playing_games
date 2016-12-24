# Simple DQN

[GitHub](https://github.com/tambetm/simple_dqn)

Deep Q-learning agent for replicating DeepMind's results in paper ["Human-level control through deep reinforcement learning"](http://www.nature.com/nature/journal/v518/n7540/full/nature14236.html).

## Примеры

[![Breakout](http://img.youtube.com/vi/KkIf0Ok5GCE/default.jpg)](https://youtu.be/KkIf0Ok5GCE)
[![Pong](http://img.youtube.com/vi/0ZlgrQS3krg/default.jpg)](https://youtu.be/0ZlgrQS3krg)
[![Seaquest](http://img.youtube.com/vi/b6g6A_n8mUk/default.jpg)](https://youtu.be/b6g6A_n8mUk)
[![Space Invaders](http://img.youtube.com/vi/Qvco7ufsX_0/default.jpg)](https://youtu.be/Qvco7ufsX_0)

## Зависимости

 * Neon
 * ALE [native Python interface](https://github.com/bbitmaster/ale_python_interface/wiki/Code-Tutorial).
 * OpenAI Gym [OpenAI Gym](https://gym.openai.com/).
 * [Fastest convolutions](https://github.com/soumith/convnet-benchmarks) from [Neon deep learning library](http://neon.nervanasys.com/docs/latest/index.html).

## Установка. Инструкция для Ubuntu.

### Neon (виртуализация)

Пререквизиты:

`sudo apt-get install libhdf5-dev libyaml-dev libopencv-dev pkg-config`

`sudo apt-get install python python-dev python-pip python-virtualenv`

Скачиваем и компилируем Neon:

`git clone https://github.com/NervanaSystems/neon.git`

`cd neon`

`make`

Neon устанавливается в папку `.venv`. 

Запуск Neon:

`source .venv/bin/activate`

Важно! `Все дальнейшие действие должны проходить с активированным Neon.`

### Arcade Learning Environment

Можно пропустить, если планируется использование OpenAI Gym.

Пререквизиты:

`sudo apt-get install cmake libsdl1.2-dev`

Скачиваем и компилируем ALE:

`git clone https://github.com/mgbellemare/Arcade-Learning-Environment.git`

`cd Arcade-Learning-Environment`

`cmake -DUSE_SDL=ON -DUSE_RLGLUE=OFF -DBUILD_EXAMPLES=ON .`

`make -j 4`

Установка:

`pip install .`

### OpenAI Gym

Можно пропустить, если планируется использование Arcade Learning Environment.

Установка:

`pip install gym`

`pip install gym[atari]`


### Simple DQN

Пререквизиты:

`pip install numpy argparse logging`

Установка OpenCV, небольшой хак для установки в виртуальной среде Neon.
Перейдите в папку /usr/lib/python2.7/dist-packages/ и посмотрите точное имя файла cv2*.so и выполните следующую команду:

`sudo apt-get install python-opencv`

`ln -s /usr/lib/python2.7/dist-packages/cv2*.so NEON_HOME/.venv/lib/python2.7/site-packages/`

где `NEON_HOME` - папка с установленным Neon. 

Скачиваем код:

`git clone https://github.com/tambetm/simple_dqn`

`cd simple_dqn`

### Опционально

Установка `matplotlib`:

`pip install matplotlib`

Для записи видео с процессом игры устанавливаем `avconv`:

`sudo apt-get install libav-tools`

## Запуск кода

### Обучение

Обучение на игре Breakout:

`./train.sh roms/breakout.bin`

Или используя OpenAI Gym:

`./train.sh Breakout-v0 --environment gym`

Опции можно посмотреть, используя ключ `--help`. 
Во время обучения веса сохраняются в папку `snapshots` после каждого epoch. Имя файла `<game>_<epoch_nr>.pkl`. 
Статистика обучения сохраняется в `results/<game>.csv`.

### Продолжение обучения

Можно продолжить обучение с помощью команды:

`./resume.sh snapshots/breakout_10.pkl`

Память с реплеями в таком случае будет пустой.

### Тестирование

Запуск тестирования обученной модели:

`./test.sh snapshots/breakout_77.pkl`

Или используя OpenAI Gym:

`./test_gym.sh snapshots/Breakout-v0_77.pkl`

Результаты тестирования сохраняются в `results/Breakout-v0`. 

Загружаем результаты в OpenAI Gym:

`./upload_gym.sh results/Breakout-v0 --api_key <your_key>`

### Просмотр одной игры обученной модели

Запуск игры:

`./play.sh snapshots/breakout_77.pkl`

По умолчанию запуск происходит на GPU.

Чтобы запустить на CPU:

`./play.sh snapshots/breakout_77.pkl --backend cpu`

Клавишы управления:

* `a` - замедлить,
* `s` - ускорить,
* `m` - ручное управление,
* `[` - прибавить звук,
* `]` - убавить звук.

### Запись видео игры

Чтобы записать видео одной игры:

`./record.sh snapshots/breakout_77.pkl`

Фреймы записываются в`videos/<game>` как PNG файлы. `avconv` конвертирует их в видео, которые сохраняются в `videos/<game>_<epoch_nr>.mov`.

### Построение графика результатов

График:

`./plot.sh results/breakout.csv`

Создается `results/breakout.png`, с графиками: average reward per game, number of games per phase (training, test or random), average Q-value of validation set and average network loss.

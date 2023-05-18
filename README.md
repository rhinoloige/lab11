```ShellSession
$ cd ~
$ mkdir install
$ mkdir tmp
$ export HOME_PREFIX=`pwd`/install
$ echo $HOME_PREFIX
$ export USERNAME=`...`
```
```ShellSession

$ cd tmp
```
```ShellSession
Подключаем событийную библиотеку libevent
$ wget https://github.com/libevent/libevent/releases/download/release-2.1.8-stable/libevent-2.1.8-stable.tar.gz
$ tar -xvzf libevent-2.1.8-stable.tar.gz        # Распоковываем архив
$ cd libevent-2.1.8-stable                      # Переходим в распакованную папку 
$ ./configure --prefix=${HOME_PREFIX}
$ make && make install 
$ cd ..
```
```ShellSession
Подключаем библиотеку для ввода и вывода термнала
$ wget http://invisible-island.net/datafiles/release/ncurses.tar.gz
$ tar -xvzf ncurses.tar.gz
$ cd ncurses-5.9
$ ./configure --prefix=${HOME_PREFIX}
$ make && make install 
$ cd ..
```
```ShellSession
Подключаем менеджер терминалов tmux
$ wget https://github.com/tmux/tmux/releases/download/2.5/tmux-2.5.tar.gz
$ tar -xvzf tmux-2.5.tar.gz
$ cd tmux-2.5
$ ./configure --prefix=${HOME_PREFIX} CFLAGS="-I${HOME_PREFIX}/include -I${HOME_PREFIX}/include/ncurses" LDFLAGS="-L${HOME_PREFIX}/lib"
$ make && make install
$ cd ..
```

```ShellSession
Скачиваем нгрок
$ wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
$ unzip ngrok-stable-linux-amd64.zip
$ mv ngrok ${HOME_PREFIX}/bin
```

```ShellSession
$ export LD_LIBRARY_PATH=${HOME_PREFIX}/lib
$ export PATH="${HOME_PREFIX}/bin:${PATH}"
$ tmux new -s session_with_group
```

```ShellSession
Регистрируемся на сайте получаем токен подключаемся к 22 порту
# Alisa:
$ open https://ngrok.com/signup
$ export NGROK_TOKEN=<токен>
$ ngrok authtoken ${NGROK_TOKEN}
$ ngrok tcp 22
<порт_ngrok_сервера>
```

```ShellSession
# Bob:
$ ssh ${USERNAME}@0.tcp.ngrok.io -p<порт_ngrok_сервера>
(<пароль_от_учетной_записи>)
$ tmux a -t session_with_group
$ <C-B>"
```

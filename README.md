# DEBIAN 10, RaspberryPi 3b+, aarch kernal

## Скомпилированную библиотеку, готовую к установке можно скачать в **[релизах](https://github.com/MiXaiLL76/numpy_openblas/releases)**

## Просто информация

[инфа по компиляции для чайников](https://stackoverflow.com/questions/11443302/compiling-numpy-with-openblas-integration)


## Raspberry Pi

1. Установите последний [релиз OpenBLAS](https://github.com/MiXaiLL76/OpenBLAS_RaspberryPi/releases)
2. Клонируйте репозиторий numpy

```
pi@raspberrypi:~ $ git clone https://github.com/numpy/numpy
pi@raspberrypi:~ $ cd numpy
```

3. Скопируем и сохраним изначальные настройки site.cfg.example

```
pi@raspberrypi:~/numpy $ cp site.cfg.example site.cfg
pi@raspberrypi:~/numpy $ sed -i "s@\/opt\/OpenBLAS@\/usr\/local@" site.cfg
pi@raspberrypi:~/numpy $ nano site.cfg

```

4. Раскоментируем данные библиотеки openblas

```
...
[openblas]
libraries = openblas
library_dirs = /usr/local/lib
include_dirs = /usr/local/include
runtime_library_dirs = /usr/local/lib
...
```

5. Проверим конфигурация 

```
pi@raspberrypi:~/numpy $ pip3 install Cython --upgrade
pi@raspberrypi:~/numpy $ python3 setup.py config
```

6. В итоговом выводе команды будут строки 

```
  FOUND:
    libraries = ['openblas', 'openblas']
    library_dirs = ['/usr/local/lib']
    language = c
    define_macros = [('HAVE_CBLAS', None)]
    runtime_library_dirs = ['/usr/local/lib']

  FOUND:
    libraries = ['openblas', 'openblas']
    library_dirs = ['/usr/local/lib']
    language = c
    define_macros = [('HAVE_CBLAS', None)]
    runtime_library_dirs = ['/usr/local/lib']
```

7. Далее нам нужно скомпилировать [колесо](https://habr.com/ru/post/210450/)

```
pi@raspberrypi:~/numpy $ python3 setup.py bdist_wheel
```
![Несколько минут спустя](fml.jpg "я ждаль")
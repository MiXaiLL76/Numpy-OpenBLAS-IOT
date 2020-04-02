# DEBIAN 10, RaspberryPi 3b+, aarch kernal

## Скомпилированную библиотеку, готовую к установке можно скачать в **[релизах](https://github.com/MiXaiLL76/numpy_openblas/releases)**

## Просто информация

[инфа по компиляции для чайников](https://stackoverflow.com/questions/11443302/compiling-numpy-with-openblas-integration)


## Raspberry Pi

1. Установите последний [релиз OpenBLAS](https://github.com/MiXaiLL76/OpenBLAS_RaspberryPi/releases)
2. Клонируйте репозиторий numpy
3. Выполняйте команды последовательно. Всмотритесь в то, что делаете!

```
ubuntu@ubuntu:~$ \
\
sudo apt update
sudo apt install -y python3-pip git gfortran
pip3 install Cython setuptools wheel -U
git clone https://github.com/numpy/numpy
cd numpy
```

1. Скопируем и сохраним изначальные настройки site.cfg.example

```
ubuntu@ubuntu:~/numpy$ cp site.cfg.example site.cfg
```

4. Добавим данные библиотеки openblas

```
ubuntu@ubuntu:~/numpy$ \
\
echo "" >> site.cfg
echo "[openblas]" >> site.cfg
echo "libraries = openblas" >> site.cfg
echo "library_dirs = /usr/local/lib" >> site.cfg
echo "include_dirs = /usr/local/include" >> site.cfg
echo "runtime_library_dirs = /usr/local/lib" >> site.cfg
```

5. Проверим конфигурация 

```
ubuntu@ubuntu:~/numpy$ python3 setup.py config
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
ubuntu@ubuntu:~/numpy$ time python3 setup.py bdist_wheel
```
![Несколько минут спустя](fml.jpg "я ждаль")

> real    8m44.266s
> 
> user    12m51.680s
> 
> sys     0m42.121s

8. Поиск нашего колеса

```
ubuntu@ubuntu:~/numpy$ find . -name *whl*
```

> FIND! ./dist/numpy-1.19.0.dev0+081c723-cp37-cp37m-linux_aarch64.whl

9. Установка колеса

```
ubuntu@ubuntu:~/numpy$ sudo pip3 install ./dist/numpy-1.19.0.dev0+081c723-cp37-cp37m-linux_aarch64.whl -U
```

> Processing ./dist/numpy-1.19.0.dev0+081c723-cp37-cp37m-linux_aarch64.whl
> 
> Installing collected packages: numpy
> 
> Successfully installed numpy-1.19.0.dev0+081c723


## Проверка СТАРОГО NUMPY КОТОРЫЙ ИЗ РЕПОЗИТОРИЯ RPI

```
pi@raspberrypi:~ $ python3 test_numpy.py
version: 1.16.2
maxint:  2147483647

BLAS info:
 * libraries ['openblas', 'openblas']
 * library_dirs ['/usr/local/lib']
 * language c
 * define_macros [('HAVE_CBLAS', None)]

dot: 2.638400 sec
```

## Проверка скомпилированного нами ~~мной~~ NUMPY

```
ubuntu@ubuntu:~$ python3 test_numpy.py
version: 1.19.0.dev0+081c723
maxint:  9223372036854775807

BLAS info:
 * libraries ['openblas', 'openblas']
 * library_dirs ['/usr/local/lib']
 * language c
 * define_macros [('HAVE_CBLAS', None)]
 * runtime_library_dirs ['/usr/local/lib']

dot: 0.115425 sec
```
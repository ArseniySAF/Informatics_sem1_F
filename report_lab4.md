# Отчёт по лабораторной работе №4
## Задание
Запустить в контейнере приложение “aafire”. Обратите внимание, что оно бесконечное и контейнер не будет автоматически отключаться.  
Приложить скриншот в процессе работы контейнера.

Далее в рамках лабораторной работы необходимо самостоятельно настроить сеть между двумя контейнерами, также как в предыдущей работе вы настраивали связь между двумя виртуальными машинами.

Для проверки сети между контейнерами вам потребуется утилита ping. Поскольку контейнеры очень маленькие и в них нет ничего лишнего (по сравнению с виртуальными машинами) - ping там не установлен. В вашем образе нужно будет установить пакет с этой утилитой, помимо aafire.

Далее запустите два контейнера с aafire и оставьте их в работающем состоянии.  
Откройте ещё одно окно терминала и создайте сеть при помощи команды

```
docker network create myNetwork
```

После этого нужно будет подключить контейнеры к вашей сети. Названия контейнеров можно увидеть при выводе списка действующих контейнеров у вас на машине.

```
docker network connect myNetwork mycontainer1
docker network connect myNetwork mycontainer2
```

Теперь при помощи следующей команды вы можете увидеть настройки созданной вами сети.

```
docker network inspect myNetwork
```

Далее вам нужно самостоятельно протестировать соединение между контейнерами утилитой ping и приложить скриншот.

## Ход работы
1. Для работы с `aafire` и `ping` был создан Dockerfile на базе Ubuntu 24.10 (версия взята с *Docker Hub*):
```Dockerfile
FROM ubuntu:24.10

RUN apt-get update && \
    apt-get install -y libaa-bin iputils-ping

ENV TERM=xterm

ENTRYPOINT [ "aafire" ]
```
**Пояснения**:
- `libaa-bin` — содержит утилиту `aafire`
- `iputils-ping` — добавляет утилиту `ping`
- `TERM=xterm` — решает проблему с ошибкой `TERM environment variable not set`
- `ENTRYPOINT [ "aafire" ]`  — запускает `aafire` при запуске контейнера 


2. **Сборка образа**:

Собрал образ с тегом `aafire` (находясь в директории с *Dockerfile*):

```bash
sudo docker build -t aafire .
```

3. **Запуск контейнеров**:

Запустил два контейнера в интерактивном режиме (в разных консолях):

```bash
sudo docker run -it --name fire1 aafire
sudo docker run -it --name fire2 aafire
```

- `-it` — запускает контейнер в интерактивном режиме
- `--name <name>` — позволяет назначить контейнеру имя `<name>`

**aafire внутри контейнера:**

<div align="left">
<img src="https://github.com/user-attachments/assets/766dfee9-9713-482e-8c96-2c6d5c90262e"/>
</div>

4. **Настройка сети:**

Создал виртуальную сеть `myNetwork`:

```bash
sudo docker network create myNetwork
```

Подключил контейнеры к сети:

```bash
sudo docker network connect myNetwork fire1
sudo docker network connect myNetwork fire2
```

Посмотрел настройки созданной сети:

```bash
sudo docker network inspect myNetwork
```

<div align="left">
<img src="https://github.com/user-attachments/assets/fed58ab4-f7a8-47ca-8d56-fa34632e2a33"/>
</div>

5. **Проверка соединения**:

Внутри контейнера `fire1` прописал `ping fire2`:

<div align="left">
<img src="https://github.com/user-attachments/assets/1085ae34-f8f6-43b0-8b9f-035bab8f4454"/>
</div>

Внутри контейнера `fire2` прописал `ping fire1`:

<div align="left">
<img src="https://github.com/user-attachments/assets/9e0452d9-e339-48a7-b63b-6f735137b04f"/>
</div>

### Ссылки на источники

[stackoverflow.com](https://stackoverflow.com/questions/16242025/term-environment-variable-not-set)

`docker --help`




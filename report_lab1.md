# Отчёт по лабораторной работе №1

В выполнении этой лабораторной работы мне помогли методические указания, и различные онлайн-ресурсы

Для начала, я установил Ubuntu на VirtualBox, после чего я сказал getedit и создал файл с следующим содержимым:

```bash
#!/bin/bash
echo "Welcome to ITMO University"
```

После этого я сохранил файл и запустил bash-скрипт, введя в терминале следующую строчку кода:

```bash
bash script.bash
```

Вывод программы:

<div align="left">
<img width="449" alt="image" src="https://github.com/user-attachments/assets/474a1573-967c-4dc5-b9c5-d650bc79600c">
</div>

Следующей задачей было написание скрипта, приветствующего пользователя. Изучив материалы, я понял, что для этого достаточно использовать аргументы командной строки. Решение оказалось простым:

```bash
#!/bin/bash
echo "Welcome, $*"
```

Здесь `$*` позволяет получить все переданные скрипту аргументы в виде одной строки.

Пример работы:

<div align="left">
<img width="449" alt="image" src="https://github.com/user-attachments/assets/3aa5cb08-c11d-4566-b671-9bb4c25bb5e9">
</div>


Работа в Linux оказалась интересным опытом. Мне понравилось <3

# Отчёт по лабораторной работе №5


## Задание 1 - Автоматизация проверки формата файлов при коммите

Предположим, у вас есть задача автоматизировать проверку формата файлов при коммите с использованием *Git Hooks*.

Перед каждым коммитом необходимо автоматически проверять, чтобы все `.txt` файлы в репозитории соответствовали определенному формату.

Создайте *bash-скрипт*, который будет выполнять проверку того, что коммитится файл формата `.txt` и в файле присутствует какой-то текст (например, в конце файла есть подпись автора, неважно как она выглядит, главное чтобы была какая-нибудь проверка содержимого файла). Далее поместите этот скрипт в нужную папку и проверьте, что перед каждым коммитом проходит проверка, например, добавьте вывод о результате проверки в консоль. За этот функционал отвечает *Git Hooks*, там сказано как автоматизировать такую проверку.

***

## Решение Задания 1

1. Сначала нужно клонировать нужный нам репозиторий. Для этого в терминал вводим команду `git clone https://github.com/ArseniySAF/lrb5.git`

2. Затем переходим в каталог с репозиторием

3. Далее редактируем файл `.git/hooks/pre-commit.sample` и переименовываем его в `.git/hooks/pre-commit`

4. Файл `.git/hooks/pre-commit` содержит следующий код:

```bash
#!/bin/sh


if git rev-parse --verify HEAD >/dev/null 2>&1; then
	against=HEAD
else
	# Initial commit: diff against an empty tree object
	against=$(git hash-object -t tree /dev/null)
fi

files_to_check=$(git diff --staged --name-only --diff-filter=ACMR "$against" | grep -e "\b.*\.txt\b")
regex='^Hello, \w+!?$'
has_errors=false

if [[ -z "$files_to_check" ]]; then
    echo "Info: No .txt files to check."
    exit 0
fi

for file in $files_to_check; do
    last_line=$(tail -n 1 "$file")
    if ! [[ -n "$last_line" && "$last_line" =~ $regex ]]; then
        echo "Error: File: $file - syntax validation failed." >&2;
        has_errors=true
    fi
done

if $has_errors; then
    echo "Commit aborted due to errors in .txt files."
    exit 1
else
    # Success exit if all files have been validated.
    echo "Info: All files have been validated."
    exit 0
fi

```


**Проверка работоспособности**
1. В директории с репозиторием создаем файл `dir/b.txt` со следующим содержимым:

```
.  Hello, world!
```

2. Если  мы пропишем в консоли `git commit -m "First commit"`, то получим следующую ошибку:

<div align="left">
<img src="https://github.com/user-attachments/assets/7c3abe76-5e9a-4c12-9f73-455ae7cce6c8"/>
</div>

3. Это связано с тем, что последняя строка файла не проходит проверку с регулярным выражением. Исправим ошибку в файле и снова пропишем `git commit -m "First commit"`. Получаем такой вывод в консоль:

<div align="left">
<img src="https://github.com/user-attachments/assets/a447a7a3-14e8-44b2-b6af-11e9331a0442"/>
</div>

4. Вывод: мы написали работоспособный *Git Hook*, который проверяет файлы перед коммитом

***

## Задание 2 - Использование Git Flow в проекте

Предположим, у вас есть задача на создание новой функциональности для вашего проекта с использованием Git Flow. Давайте рассмотрим конкретный пример. В примере важен не сам проект и его код (его тут вообще как такового нет), а принцип работы Git Flow.

***

## Решение Задания 2

1. Скачиваем *Git Glow* на локальную машину:

2. В корне репозитория выполняем инициализацию *Git Flow.*

```bash
git flow init
```

3. Создаем ветку для новой функциональности, пусть она будет называться *"notification_management"*:

```bash
git flow feature start notification-management
```

<div align="left">
<img src="https://github.com/user-attachments/assets/c3bd4e32-9d7e-4c28-b26b-0bab87bd6e6f"/>
</div>

4. Внесем изменения в код для добавления функционала управления уведомлениями в файл `notification_manager.py`:

```python
class NotificationManager:
    def create_notification(self, date, title) -> None:
        # TODO: Defeat the Void here (code still in negotiations with the abyss)
        print(f"Notification: {title} - created")

```

Выполним коммит изменения по мере разработки:

```bash
git add notification_manager.py
git commit -m "Added: notification_manager class"
```

5. После завершения разработки функции завершаем фичу и объединяем ее с основной веткой:

<div align="left">
<img src="https://github.com/user-attachments/assets/684561e8-2794-4a2e-969b-38119c302cdc"/>
</div>

*Git Flow* автоматически переключится на ветку *develop* и выполнил слияние.

6. В ветке *develop* начинаем создание релиза:

<div align="left">
<img src="https://github.com/user-attachments/assets/1efea9d4-8037-4792-80b5-2094818d113e"/>
</div>

7. После этого создаем текстовый файл `version.txt`, в который внесем изменения с помощью команды `echo "v1.0.0" > version.txt`. После этого добавим его в индекс и закоммитим:

```bash
echo "v1.0.0" > version.txt
git add version.txt
git commit -m "upd: version for release v1.0.0"
```

8. Завершаем релиз и объединяем его с ветками *develop* и *master*

<div align="left">
<img src="images/Pasted image 20250413014424.png"/>
</div>

9. Создадим *hotfix*.

```bash
git flow hotfix start hotfix-1.0.1
```

<div align="left">
<img src="images/Pasted image 20250413014537.png"/>
</div>

10. (Создадим и) исправим критическую ошибку и закоммитим:

<div align="left">
<img src="images/Pasted image 20250413014908.png"/>
</div>

11. Завершаем *hotfix* и объединяем с ветками *develop* и *master*:

<div align="left">
<img src="images/Pasted image 20250413015258.png"/>
</div>

12. Завершаем работу и отправляем изменения на удаленный репозиторий:

```bash
git push origin develop
git push origin main
```

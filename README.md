№ Инструкция по решению merge-конфликтов в Git

При работе в совместных проектах с использованием системы контроля версий Git неизбежно возникают ситуации, когда несколько участников одновременно вносят изменения в одни и те же файлы. Эти изменения могут конфликтовать между собой, и Git сообщит вам о необходимости разрешения таких конфликтов. В этой инструкции мы рассмотрим, как правильно и эффективно разрешать merge конфликты в Git.

## Что такое merge-конфликт?

Merge конфликт - это ситуация, когда Git не способен автоматически объединить изменения из разных веток, потому что они затрагивают одни и те же строки кода или текстовые файлы. В результате Git останавливает процесс слияния (merge) и оставляет решение конфликта на вас.

Merge-конфликт выглядит следующим образом:

<pre>
<<<<<<< HEAD
Код из текущей ветки
=======
Код из другой ветки
>>>>>>> branch-name
  </pre>
<<<<<<< HEAD: маркер начала изменений из текущей ветки (вашей локальной).
=======: разделяющий маркер.
>>>>>>> branch-name: маркер начала изменений из другой ветки (например, ветки branch-name).
 
## Почему так бывает?

Сбой в процессе слияния говорит о наличии конфликта между текущей локальной веткой и веткой, с которой выполняется слияние. Это бывает в следующих случаях:

- Двое или больше людей изменили одну и ту же строку в файле и пытаются смержить изменения в одну и ту же ветку
- Один разработчик удалил файл, а другой его отредактировал, и оба пытаются смержить свои изменения в одну ветку
- Один разработчик удалил строку, а другой ее отредактировал, и оба пытаются смержить свои изменения в одну ветку

## Как разрешить merge-конфликт?

Рассмотрим как решить merge-конфликт на конкретном примере.

Исходный текст в файле руководство_по_настройке.txt:


<pre> Для настройки приложения необходимо выполнить следующие шаги:
1. Откройте меню "Настройки".
2. Выберите раздел "Сеть".
3. Введите IP-адрес сервера. </pre>

Первый писатель внес изменения:

<pre>Для настройки приложения выполните следующие шаги:
1. Откройте меню "Настройки".
2. Выберите раздел "Сеть".
3. Введите IP-адрес сервера.</pre>>

Второй писатель внес изменения:

<pre>
Для настройки приложения выполните следующие действия:
1. Откройте меню "Настройки".
2. Выберите раздел "Сеть".
3. Укажите IP-адрес сервера.</pre>

Направив merge-request, второй писатель получает сообщение об ошибке:

<pre>Для настройки приложения <<<<<<< HEAD
выполните следующие шаги:
=======
выполните следующие действия:
>>>>>>> branch2
1. Откройте меню "Настройки".
2. Выберите раздел "Сеть".
3. <<<<<<< HEAD
Введите IP-адрес сервера.
=======
Укажите IP-адрес сервера.
>>>>>>> branch2
</pre>

Чтобы разрешить конфликт, следуйте шагам ниже:

1. Запустите команду `git status`, чтобы просмотреть подробную информацию о текущем состоянии репозитория.
Вывод команды git status говорит о том, что из-за конфликта не удалось слить пути. 

2. Запустите `$ cat merge.txt` и просмотрите, что изменилось.
3. Запустите команду `git merge-base new_branch`, для каждого файла будут сравнены три версии base ours и theirs
4. Для просмотра изменений в ветке выполните команду `git diff branch`. Команда `diff` помогает найти различия между состояниями репозитория/файлов.
> Выполните команду `git diff-U0`, чтобы отображались только изменения.

На этом этапе необходимо вручную анализировать изменения и внести изменения, которые соответствуют логике и стилю документации.

Но бывают случаи, когда невозможно определить, какие изменения внести, выполните команду `git reset –merge`. Она оставляет незакоммиченные изменения в файлах, которые не участвовали в слиянии, которые в обоих ветках одинаковы. Эта команда позволяет откатиться к определенному коммиту и удалить все коммиты, которые были сделаны после указанного коммита.

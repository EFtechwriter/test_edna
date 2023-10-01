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

решить конфликт можно несколькими способами
### Создание временной ветки. 
Этот способ подходит для управления сложными конфликтами, особенно если вы не хотите портить текущую ветку или если необходимо провести более детальное тестирование изменений после разрешения конфликтов. Вот как создать и использовать временную ветку для разрешения merge конфликтов:

  1. Создайте новую ветку с помощью команды `git checkout -b` и укажите имя для новой ветки.

<pre> git checkout -b temporary_branch </pre>
Замените temporary_branch на желаемое имя для вашей временной ветки.

2. Выполните слияние или перебазирование в вашей временной ветке.

<pre>
git merge <temporary_branch> </pre>

> Этот метод может вызвать конфликты. Разрешите конфликты в вашей временной ветке, используя текстовый редактор: удалите метки конфликтов (<<<<<<<, =======, >>>>>>>) и сохраните изменения.

3. После успешного разрешения конфликтов добавьте изменения в индекс с помощью команды `git add` и создайте коммит: `git commit -m "Merge_conflict_solved"`

4. Завершите процесс слияния с помощью команды `git merge --continue`. Если вы выполнили перебазирование, продолжайте перебазирование командой git rebase --continue, пока не завершите все коммиты из другой ветки.

5. Если ваши изменения были успешно включены в основную ветку и временная ветка больше не нужна, вы можете ее удалить.

<pre>
git branch -d temporary_branch </pre>




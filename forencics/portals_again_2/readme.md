# И снова порталы #2

1. Также как и в предыдущей задаче, нам доступен файл образа диска, только в этот раз монтирование даст только скрытый файл hint.txt, в котором содержится запись: "Try to walk with binaries..."

2. При извлечении всех доступных файлов утилитой binwalk, получаем архив, в котором находятся docx файл и подсказка (The pass has already given to you).

3. Паролем от docx файла является комментарий к архиву: This is the pass. Открываем док, получаем флаг: VrnCTF{Alternat1ve_p4th} 

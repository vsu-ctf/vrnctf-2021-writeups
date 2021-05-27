Классическая задача на переполнение буфера.

1. Декомпилируем файл, изучаем логику его работы и восстанавливаем высокоуровневый код.


```c
    char data[64];
    char placeholder[30] = {0};
// ...

    printf("Welcome! Do you think that you can pass all the witcher's tests?\n");

    if (!is_positive_answer(data, sizeof(data)))
    {
        printf("Okay, you will not be a real witcher\nGo away!\n");
        return 0;
    }

    printf("What is your age?\n");
    read_answer(data, sizeof(data));

    printf("Where are you from?\n");
    read_answer(data, sizeof(data));

    printf("Do you know Geralt of Rivia?\n");

    if (!is_positive_answer(data, sizeof(data)))
    {
        printf("Ohh, you should know history of our world...\nGo away!\n");
        return 0;
    }

    printf("Should a witcher use some magic?\n");

    if (!is_positive_answer(data, sizeof(data)))
    {
        printf("Ohh, witchers should learn magic too...\nGo away!\n");
        return 0;
    }

    if (!strcmp(placeholder, "my_magic_is_strong"))
    {
        printf("You will become a good witcher. VrnCTF{...}");
    }
    else
    {
        printf("Ohh, you do not see hidden things...\nGo away!\n");
        return 0;
    }
```

```c
void read_answer(char* data, size_t data_len)
{
    memset(data, '\0', data_len);
    gets(data);
}

bool is_positive_answer(char* data, size_t data_len)
{
    read_answer(data, data_len);

    return strcmp(data, "yes") == 0;
}
```

2. Обращаем внимание на использование небезопасной функции `gets`, которая не проверяет границы переданного буфера и допускает переполнения.

3. При ответе на вопрос `Where are you from?` переполняем буфер `data` и захватываем `placeholder`, записывая туда строку `my_magic_is_strong` следом за байтами буфера. Вносим ответы на оставшиеся вопросы и забираем флаг.


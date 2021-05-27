Ещё одна задача на переполнения.

1. Декомпилируем файл и восстанавливаем логику его работы:


```c
const char* args[] = {
    "haha, wrong",
    "use another one",
    "/bin/sh",
    "wrong arg again"
};

void give_me_the_flag(int arg)
{
    printf("Welcome here, young witcher! Your arg is %d", arg);
    system(args[arg]);
}

int main()
{
    char data[64];

    setvbuf(stdout, NULL, _IONBF, 0);

    gid_t gid = getegid();
    setresgid(gid, gid, gid);

    printf("Hello, stranger! Your magic number is %p. Give me some magic too...", give_me_the_flag);

    gets(data);

    printf("Your return address is %p\n", __builtin_return_address(0));

    return 0;
}
```

2. Замечаем, что нам нужно, во-первых, подменить адрес возврата функции `main` на адрес `give_me_the_flag`, а затем передать в эту функцию индекс нужной команды, то есть 3.

3. Смотрим адрес функции в отладчике или любым другим способом.

4. Переполняем буфер `data`, записывая туда нужные значения и получаем флаг (не забываем отключить ASLR при локальной отладке).


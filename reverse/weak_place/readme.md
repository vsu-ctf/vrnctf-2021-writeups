В данной задаче необходимо декомпилировать jar-файл, понять логику его работы, а затем воспользоваться локальным включением файла на удалённом сервере.

1. Декомилируем файл, например, в Java Decompiler.

```java
public class Main {
  public static String getHTML(String urlString) throws Exception {
    StringBuilder result = new StringBuilder();
    URL url = new URL("http://vrn2021-demo.vsu-ctf.ru:50005" + urlString);
    HttpURLConnection conn = (HttpURLConnection)url.openConnection();
    conn.setRequestMethod("GET");
    try (BufferedReader reader = new BufferedReader(new InputStreamReader(conn
            .getInputStream()))) {
      String line;
      while ((line = reader.readLine()) != null)
        result.append(line);
    }
    return result.toString();
  }

  public static String startSessionAndGetToken() throws Exception {
    return getHTML("/api/start_session?salt=" + (System.currentTimeMillis() / 1000L));
  }

  public static String requestNotifications(String token) throws Exception {
    return getHTML("/api/read_notifications?token=" + token + "&storage=notifications.txt");
  }

  public static String requestCompletion(String token, String text) throws Exception {
    return getHTML("/api/complete?token=" + token + "&text=" + URLEncoder.encode(text, String.valueOf(StandardCharsets.UTF_8)));
  }

  public static void main(String[] args) throws Exception {
    String token = startSessionAndGetToken();
    System.out.println("Welcome! Have a good day with my ouija! You notifications from the other side: " + requestNotifications(token));
    System.out.println();
    Scanner scanner = new Scanner(System.in);
    while (true) {
      System.out.println("Enter your message to the other side, and some random creature will complete it: ");
      String messageStart = scanner.nextLine();
      System.out.println("Wait for the answer...");
      String story = requestCompletion(token, messageStart);
      System.out.println("Full story from the other side: ");
      System.out.println(messageStart + story);
      System.out.println();
      System.out.println("Wait for 15 seconds before new attempt... Creatures should have a rest.");
      TimeUnit.SECONDS.sleep(15L);
      System.out.println();
    }
  }
}

```

2. Видим, что приложение работает с удалённым сервером путём отправки ему HTTP-запросов. Взаимодействие начинается с посылки запроса на /api/start_session?salt={SALT}. Вместо соли можно подставить текущее время. В ответе сервер выдаст токен для отправки других запросов. Запрос можно выполнить как вручную в браузере, так и с помощью скрипта.

3. Отправляем запрос на `/api/read_notifications?token={TOKEN}&storage=notifications.txt`, подставляем название файла flag.txt, получаем флаг.
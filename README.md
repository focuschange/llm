# OpenAI API 챗봇 간단 호출 예제

다음 예제로 OpenAI API를 간단히 호출해 "Hello!" 메시지에 대한 응답을 받아볼 수 있습니다. macOS 환경을 기준으로 하며, API 키는 환경 변수 `OPENAI_API_KEY`로 주입합니다.

## Python 예제
1. 가상환경 생성 및 라이브러리 설치:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   pip install openai python-dotenv
   ```
2. `.env` 파일에 API 키 설정:
   ```env
   OPENAI_API_KEY=sk-...
   ```
3. `chat.py` 작성 후 실행:
   ```python
   import os
   from openai import OpenAI
   from dotenv import load_dotenv

   load_dotenv()
   client = OpenAI(api_key=os.environ["OPENAI_API_KEY"])

   resp = client.chat.completions.create(
       model="gpt-4o-mini",
       messages=[{"role": "user", "content": "Hello!"}],
   )

   print(resp.choices[0].message.content)
   ```
   ```bash
   python3 chat.py
   ```

## Java 예제 (단일 파일)
1. 환경 변수로 API 키 설정:
   ```bash
   export OPENAI_API_KEY="sk-..."
   ```
2. `src/main/java/ChatExample.java` 내용을 작성:
   ```java
   import java.io.IOException;
   import java.net.URI;
   import java.net.http.HttpClient;
   import java.net.http.HttpRequest;
   import java.net.http.HttpResponse;
   import java.nio.charset.StandardCharsets;

   public class ChatExample {
       public static void main(String[] args) throws IOException, InterruptedException {
           String apiKey = System.getenv("OPENAI_API_KEY");
           if (apiKey == null || apiKey.isBlank()) {
               System.err.println("OPENAI_API_KEY 환경 변수가 설정되어 있지 않습니다.");
               return;
           }

           String body = "{\"model\":\"gpt-4o-mini\",\"messages\":[{\"role\":\"user\",\"content\":\"Hello!\"}]}";

           HttpRequest request = HttpRequest.newBuilder()
                   .uri(URI.create("https://api.openai.com/v1/chat/completions"))
                   .header("Authorization", "Bearer " + apiKey)
                   .header("Content-Type", "application/json")
                   .POST(HttpRequest.BodyPublishers.ofString(body, StandardCharsets.UTF_8))
                   .build();

           HttpClient client = HttpClient.newHttpClient();
           HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
           System.out.println(response.body());
       }
   }
   ```
3. 실행: Java 11+이 설치되어 있으면 루트에서 다음으로 실행:
   ```bash
   javac src/main/java/ChatExample.java
   java -cp src/main/java ChatExample
   ```

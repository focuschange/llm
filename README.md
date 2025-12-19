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

## Java 예제 (Gradle)
1. `build.gradle` 의존성 추가:
   ```groovy
dependencies {
    implementation 'com.squareup.okhttp3:okhttp:4.12.0'
    implementation 'com.fasterxml.jackson.core:jackson-databind:2.17.1'
}
   ```
2. 환경 변수로 API 키 설정:
   ```bash
   export OPENAI_API_KEY="sk-..."
   ```
3. 간단한 호출 코드:
   ```java
   import okhttp3.*;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import java.io.IOException;
   import java.util.Map;

   public class ChatExample {
       public static void main(String[] args) throws IOException {
           String apiKey = System.getenv("OPENAI_API_KEY");
           OkHttpClient client = new OkHttpClient();
           ObjectMapper mapper = new ObjectMapper();

           String body = mapper.writeValueAsString(Map.of(
               "model", "gpt-4o-mini",
               "messages", new Object[]{Map.of("role", "user", "content", "Hello!")}
           ));

           Request request = new Request.Builder()
               .url("https://api.openai.com/v1/chat/completions")
               .post(RequestBody.create(body, MediaType.get("application/json")))
               .addHeader("Authorization", "Bearer " + apiKey)
               .build();

           try (Response response = client.newCall(request).execute()) {
               System.out.println(response.body().string());
           }
       }
   }
   ```
4. 실행: IntelliJ 또는 `./gradlew run`으로 실행하면 응답 JSON이 출력됩니다.

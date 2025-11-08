```text
SpringBoot中如何读取"E:\DRINK\imitate\01-redis\tempData\phones.txt"改文件，并将每行数据存放到ArrayList中
```

在Spring Boot应用程序中读取文件并将每行数据存放到`ArrayList`中，可以使用Java的文件IO操作。以下是一个简单的示例代码，展示如何实现这一功能：

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

public class FileReaderExample {

    public static void main(String[] args) {
        String filePath = "E:\\DRINK\\imitate\\01-redis\\tempData\\phones.txt";
        List<String> phoneNumbers = new ArrayList<>();
        
        try (BufferedReader br = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = br.readLine()) != null) {
                phoneNumbers.add(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        // 输出读取到的电话号码
        for (String phoneNumber : phoneNumbers) {
            System.out.println(phoneNumber);
        }
    }
}
```

### 代码解释：
1. **导入必要的类**：
   - `BufferedReader` 和 `FileReader` 用于读取文件。
   - `ArrayList` 和 `List` 用于存储读取到的数据。

2. **定义文件路径**：
   - 将文件路径定义为字符串变量 `filePath`。

3. **创建一个`ArrayList`**：
   - 用于存储读取到的每一行数据。

4. **使用`try-with-resources`语句**：
   - 确保在读取文件后自动关闭资源。
   - `BufferedReader` 包装 `FileReader` 以逐行读取文件。

5. **逐行读取文件内容**：
   - 使用 `readLine()` 方法逐行读取文件内容，并将每一行添加到 `ArrayList` 中。

6. **捕获异常**：
   - 捕获并处理可能发生的 `IOException`。

7. **输出读取到的数据**：
   - 遍历 `ArrayList` 并打印每一行数据。

### 在Spring Boot中使用
如果你需要在Spring Boot应用程序中读取文件，可以将上述代码放入一个服务类中，并在需要时调用该方法。例如：

```java
import org.springframework.stereotype.Service;
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

@Service
public class FileService {

    public List<String> readFile(String filePath) {
        List<String> phoneNumbers = new ArrayList<>();
        
        try (BufferedReader br = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = br.readLine()) != null) {
                phoneNumbers.add(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        return phoneNumbers;
    }
}
```

然后在你的控制器或其他地方调用这个服务：

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;

@RestController
public class FileController {

    @Autowired
    private FileService fileService;

    @GetMapping("/readFile")
    public List<String> readFile() {
        String filePath = "E:\\DRINK\\imitate\\01-redis\\tempData\\phones.txt";
        return fileService.readFile(filePath);
    }
}
```

这样，你就可以在Spring Boot应用程序中读取文件并将每行数据存放到`ArrayList`中了。

---
---

```text
继续上述问题如何将，上述每个号进行hash之后保存到新的List中，最后统一在保存到E:\\DRINK\\imitate\\01-redis\\tempData\\tokens.txt中（每行一个hash值）
```

要将每个电话号码进行哈希处理并将结果保存到新的`List`中，然后再将这些哈希值写入到指定的文件中，你可以使用Java的`MessageDigest`类来生成哈希值。以下是一个完整的示例代码：

```java
import java.io.*;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.ArrayList;
import java.util.List;

public class HashFileExample {

    public static void main(String[] args) {
        String inputFilePath = "E:\\DRINK\\imitate\\01-redis\\tempData\\phones.txt";
        String outputFilePath = "E:\\DRINK\\imitate\\01-redis\\tempData\\tokens.txt";
        List<String> phoneNumbers = new ArrayList<>();
        List<String> hashedTokens = new ArrayList<>();

        try {
            // 读取文件并将每行数据存放到ArrayList中
            phoneNumbers = readFile(inputFilePath);

            // 对每个电话号码进行哈希处理
            hashedTokens = hashPhoneNumbers(phoneNumbers);

            // 将哈希值写入到新的文件中
            writeFile(outputFilePath, hashedTokens);
        } catch (IOException | NoSuchAlgorithmException e) {
            e.printStackTrace();
        }
    }

    private static List<String> readFile(String filePath) throws IOException {
        List<String> phoneNumbers = new ArrayList<>();
        try (BufferedReader br = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = br.readLine()) != null) {
                phoneNumbers.add(line);
            }
        }
        return phoneNumbers;
    }

    private static List<String> hashPhoneNumbers(List<String> phoneNumbers) throws NoSuchAlgorithmException {
        List<String> hashedTokens = new ArrayList<>();
        MessageDigest md = MessageDigest.getInstance("SHA-256");

        for (String phoneNumber : phoneNumbers) {
            md.update(phoneNumber.getBytes());
            byte[] digest = md.digest();
            StringBuilder hexString = new StringBuilder(2 * digest.length);
            for (byte b : digest) {
                String hex = Integer.toHexString(0xff & b);
                if (hex.length() == 1) {
                    hexString.append('0');
                }
                hexString.append(hex);
            }
            hashedTokens.add(hexString.toString());
        }

        return hashedTokens;
    }

    private static void writeFile(String filePath, List<String> hashedTokens) throws IOException {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(filePath))) {
            for (String token : hashedTokens) {
                bw.write(token);
                bw.newLine();
            }
        }
    }
}
```

### 代码解释：
1. **读取文件**：
   - 使用`readFile`方法读取输入文件并将每行数据存放到`ArrayList`中。

2. **哈希处理**：
   - 使用`hashPhoneNumbers`方法对每个电话号码进行哈希处理。这里使用了SHA-256算法。
   - `MessageDigest`类用于生成哈希值，并将每个电话号码转换为字节数组，然后进行哈希处理。
   - 将哈希值转换为十六进制字符串并添加到`hashedTokens`列表中。

3. **写入文件**：
   - 使用`writeFile`方法将哈希值写入到指定的输出文件中，每行一个哈希值。

### 在Spring Boot中使用
如果你需要在Spring Boot应用程序中实现这一功能，可以将上述代码放入一个服务类中，并在需要时调用该方法。例如：

```java
import org.springframework.stereotype.Service;
import java.io.*;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.ArrayList;
import java.util.List;

@Service
public class HashService {

    public void processFile(String inputFilePath, String outputFilePath) {
        List<String> phoneNumbers = new ArrayList<>();
        List<String> hashedTokens = new ArrayList<>();

        try {
            // 读取文件并将每行数据存放到ArrayList中
            phoneNumbers = readFile(inputFilePath);

            // 对每个电话号码进行哈希处理
            hashedTokens = hashPhoneNumbers(phoneNumbers);

            // 将哈希值写入到新的文件中
            writeFile(outputFilePath, hashedTokens);
        } catch (IOException | NoSuchAlgorithmException e) {
            e.printStackTrace();
        }
    }

    private List<String> readFile(String filePath) throws IOException {
        List<String> phoneNumbers = new ArrayList<>();
        try (BufferedReader br = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = br.readLine()) != null) {
                phoneNumbers.add(line);
            }
        }
        return phoneNumbers;
    }

    private List<String> hashPhoneNumbers(List<String> phoneNumbers) throws NoSuchAlgorithmException {
        List<String> hashedTokens = new ArrayList<>();
        MessageDigest md = MessageDigest.getInstance("SHA-256");

        for (String phoneNumber : phoneNumbers) {
            md.update(phoneNumber.getBytes());
            byte[] digest = md.digest();
            StringBuilder hexString = new StringBuilder(2 * digest.length);
            for (byte b : digest) {
                String hex = Integer.toHexString(0xff & b);
                if (hex.length() == 1) {
                    hexString.append('0');
                }
                hexString.append(hex);
            }
            hashedTokens.add(hexString.toString());
        }

        return hashedTokens;
    }

    private void writeFile(String filePath, List<String> hashedTokens) throws IOException {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(filePath))) {
            for (String token : hashedTokens) {
                bw.write(token);
                bw.newLine();
            }
        }
    }
}
```

然后在你的控制器或其他地方调用这个服务：

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HashController {

    @Autowired
    private HashService hashService;

    @GetMapping("/processFile")
    public void processFile() {
        String inputFilePath = "E:\\DRINK\\imitate\\01-redis\\tempData\\phones.txt";
        String outputFilePath = "E:\\DRINK\\imitate\\01-redis\\tempData\\tokens.txt";
        hashService.processFile(inputFilePath, outputFilePath);
    }
}
```

这样，你就可以在Spring Boot应用程序中读取文件，对每个电话号码进行哈希处理，并将结果写入到指定的文件中了。


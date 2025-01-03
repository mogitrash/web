# фреймворки для автоматизированной сборки и управления проектами

## Теория

### Основные фреймворки для автоматизации сборки:

- **Maven**: Управление зависимостями, создание исполняемых файлов, управление жизненным циклом сборки.
- **Gradle**: Мощный инструмент с гибкостью в написании сборочных скриптов (использует Groovy или Kotlin).
- **Ant**: Старейший инструмент, основанный на XML-скриптах, гибкий, но требует больше ручной настройки.
- **SBT (Scala Build Tool)**: Специализирован для Scala-проектов.
- **Make**: Классический инструмент для сборки, особенно популярен в C/C++.

### Жизненный цикл сборки:

1. **Очистка (clean)**: Удаление временных файлов.
2. **Сборка (compile)**: Компиляция исходного кода.
3. **Тестирование (test)**: Автоматическое выполнение тестов.
4. **Упаковка (package)**: Создание исполняемых файлов (JAR, WAR и т. д.).
5. **Установка (install)**: Установка артефактов в локальный репозиторий.
6. **Развертывание (deploy)**: Публикация артефактов в удаленном репозитории.

### Пример кода (Maven):

```xml
<!-- pom.xml -->
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>demo-project</artifactId>
    <version>1.0.0</version>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-clean-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

---

# Maven

## Теория

Maven используется для:

- Управления зависимостями проекта (central repository, local repository).
- Конфигурации и автоматизации сборки ПО.
- Поддержания модульности и интеграции.
- Подключения плагинов для расширения функциональности (например, генерация документации, отчётов, запуск тестов).

### Пример кода:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>3.0.0</version>
    </dependency>
</dependencies>
```

### Команда для сборки:

```bash
mvn clean install
```

### pom.xml (Project Object Model)

Основной файл конфигурации Maven, который содержит:

- **Метаданные проекта**: `groupId`, `artifactId`, `version`.
- **Зависимости**: Список библиотек, необходимых для работы проекта.
- **Плагины**: Инструменты для выполнения задач сборки.

### Основные команды Maven

1. **`mvn clean`**: Удаляет временные файлы сборки.
2. **`mvn compile`**: Компилирует исходный код.
3. **`mvn test`**: Выполняет тесты.
4. **`mvn package`**: Создаёт JAR/WAR.
5. **`mvn install`**: Устанавливает артефакт в локальный репозиторий.
6. **`mvn deploy`**: Загружает артефакт в удалённый репозиторий.

### Пример pom.xml:

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>my-app</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

---

# Архетипы приложений

### Архетипы

Архетипы — это шаблоны для создания новых проектов. Они помогают быстро генерировать структуру проекта с базовой настройкой.  
Пример архетипа: `maven-archetype-quickstart`, который создаёт структуру простого Java-проекта.

### Плагины

Плагины расширяют функциональность Maven. Каждый этап жизненного цикла связан с определённым плагином:

- **`maven-compiler-plugin`**: Компиляция исходного кода.
- **`maven-surefire-plugin`**: Запуск тестов.
- **`maven-assembly-plugin`**: Упаковка проекта в архив.

### Пример использования архетипа:

```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

### Пример настройки плагина:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```

# Понятие персистенции (persistence) в языках программирования. Отличительные свойства персистенции от маршаллинга (marshalling) и сериализации (serialization)

### Персистенция

Сохранение данных в долговременное хранилище, такое как базы данных или файлы.

### Сериализация

Преобразование объекта в последовательность байтов для передачи или сохранения.

### Маршаллинг

Процесс подготовки данных для передачи между процессами или системами, близкий по смыслу к сериализации.

### Отличия:

| Свойство               | Персистенция            | Сериализация        | Маршаллинг         |
| ---------------------- | ----------------------- | ------------------- | ------------------ |
| **Назначение**         | Долговременное хранение | Передача/сохранение | Межсистемный обмен |
| **Уровень абстракции** | Высокий                 | Средний             | Высокий/Средний    |
| **Примеры**            | Hibernate, JPA          | Java Serializable   | RPC, RMI           |

### Пример кода (Hibernate):

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;

    // Getters and setters
}
```

### Для сериализации:

```java
import java.io.*;

public class SerializeExample {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        User user = new User();
        user.setName("John");
        user.setEmail("john@example.com");

        // Сериализация
        try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("user.ser"))) {
            out.writeObject(user);
        }

        // Десериализация
        try (ObjectInputStream in = new ObjectInputStream(new FileInputStream("user.ser"))) {
            User deserializedUser = (User) in.readObject();
            System.out.println(deserializedUser.getName());
        }
    }
}
```

---

# Data Access Object (DAO)

### Шаблон DAO

- Обеспечивает изоляцию логики доступа к данным от бизнес-логики.
- Снижает зависимость приложения от конкретной базы данных.
- Упрощает тестирование и замену источников данных.

### Платформенно независимые стандарты взаимодействия:

- **JDBC (Java Database Connectivity)**: Основной API для работы с реляционными базами данных.
- **JPA (Java Persistence API)**: ORM-решение для работы с объектно-реляционными структурами.
- **Hibernate**: Расширение JPA с дополнительными функциями.
- **Spring Data**: Абстракция для взаимодействия с различными хранилищами данных (SQL, NoSQL).

### Пример кода (DAO):

```java
public interface UserDAO {
    User getUserById(int id);
    void saveUser(User user);
}

public class UserDAOImpl implements UserDAO {
    private Connection connection;

    public UserDAOImpl(Connection connection) {
        this.connection = connection;
    }

    @Override
    public User getUserById(int id) {
        String sql = "SELECT * FROM users WHERE id = ?";
        try (PreparedStatement stmt = connection.prepareStatement(sql)) {
            stmt.setInt(1, id);
            ResultSet rs = stmt.executeQuery();
            if (rs.next()) {
                return new User(rs.getInt("id"), rs.getString("name"), rs.getString("email"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }

    @Override
    public void saveUser(User user) {
        String sql = "INSERT INTO users (name, email) VALUES (?, ?)";
        try (PreparedStatement stmt = connection.prepareStatement(sql)) {
            stmt.setString(1, user.getName());
            stmt.setString(2, user.getEmail());
            stmt.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

# Statement, PreparedStatement, CallableStatement

### Statement

- Выполнение SQL-запросов без параметров.
- Используется для простых запросов, таких как `SELECT`.
- Минус: уязвимость к SQL-инъекциям.

### PreparedStatement

- Предварительно компилирует запросы с параметрами.
- Улучшенная производительность и защита от SQL-инъекций.
- Поддерживает типизированные параметры, такие как `setInt` и `setString`.

### CallableStatement

- Используется для вызова хранимых процедур в базе данных.
- Позволяет работать с входными и выходными параметрами.

### Примеры:

```java
// Statement
Statement stmt = connection.createStatement();
ResultSet rs = stmt.executeQuery("SELECT * FROM users");

// PreparedStatement
String sql = "SELECT * FROM users WHERE id = ?";
PreparedStatement pstmt = connection.prepareStatement(sql);
pstmt.setInt(1, 10);
ResultSet prs = pstmt.executeQuery();

// CallableStatement
CallableStatement cstmt = connection.prepareCall("{call getUserById(?)}");
cstmt.setInt(1, 10);
ResultSet crs = cstmt.executeQuery();
```

---

# Цель и возможности управления персистентностью. Назначение и способы управления транзакциями в JDBC

### Цель управления персистентностью

- Обеспечить сохранение объектов в хранилище (БД, файл) с минимальной потерей данных.
- Упростить взаимодействие между объектами приложения и реляционными структурами.

### Управление транзакциями в JDBC

- Позволяет контролировать выполнение набора операций как единого атомарного действия.
- Основные операции:
  - `commit`: Сохранение изменений.
  - `rollback`: Откат изменений.

### Пример:

```java
try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASSWORD)) {
    conn.setAutoCommit(false);

    PreparedStatement updateStmt = conn.prepareStatement("UPDATE accounts SET balance = balance - ? WHERE id = ?");
    updateStmt.setInt(1, 100);
    updateStmt.setInt(2, 1);
    updateStmt.executeUpdate();

    PreparedStatement insertStmt = conn.prepareStatement("INSERT INTO transactions (account_id, amount) VALUES (?, ?)");
    insertStmt.setInt(1, 1);
    insertStmt.setInt(2, -100);
    insertStmt.executeUpdate();

    conn.commit(); // Все изменения фиксируются
} catch (SQLException e) {
    conn.rollback(); // Откат изменений в случае ошибки
    e.printStackTrace();
}
```

---

# Современные форматы представления данных

### XML (Extensible Markup Language)

- Гибкость структуры, но громоздкость.
- Хорошо подходит для сложных и иерархических данных.

### JSON (JavaScript Object Notation)

- Простой формат обмена данными, компактный и легко читаемый.
- Используется в веб-приложениях и API.

### Маршаллинг

- Преобразование объектов в формат, пригодный для передачи (например, JSON, XML).

### Пример (JSON с Jackson):

```java
ObjectMapper mapper = new ObjectMapper();
User user = new User(1, "John", "john@example.com");

// Маршаллинг (объект -> JSON)
String json = mapper.writeValueAsString(user);

// Демаршаллинг (JSON -> объект)
User deserializedUser = mapper.readValue(json, User.class);
```

# Основное назначение, форматы и специфика применения JSON формата данных в приложениях

### Назначение JSON:

- Компактный формат обмена данными между клиентом и сервером.
- Простая структура, легко читается человеком и машиной.

### Основные форматы:

- **Объекты**: `{ "key": "value" }`.
- **Массивы**: `[1, 2, 3]`.
- **Примитивы**: строки, числа, логические значения.

### Применение:

- Обмен данными в REST API.
- Конфигурационные файлы.
- Хранение данных в NoSQL (например, MongoDB).

### Пример:

```json
{
  "name": "John",
  "age": 30,
  "skills": ["Java", "Spring", "Hibernate"]
}
```

---

# Возможности и способ применения JAXB парсера документов

### Теория:

- **JAXB** (Java Architecture for XML Binding) позволяет автоматически преобразовывать Java-объекты в XML и обратно.
- Удобен для работы с типизированными данными.

### Пример:

1. **Аннотированная модель:**

```java
import javax.xml.bind.annotation.XmlElement;
import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement
public class User {
    private String name;
    private int age;

    @XmlElement
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @XmlElement
    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

2. **Маршаллинг и демаршаллинг:**

```java
import javax.xml.bind.JAXBContext;
import javax.xml.bind.Marshaller;
import javax.xml.bind.Unmarshaller;
import java.io.File;

public class JAXBExample {
    public static void main(String[] args) throws Exception {
        JAXBContext context = JAXBContext.newInstance(User.class);

        // Маршаллинг
        User user = new User();
        user.setName("John");
        user.setAge(30);
        Marshaller marshaller = context.createMarshaller();
        marshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
        marshaller.marshal(user, new File("user.xml"));

        // Демаршаллинг
        Unmarshaller unmarshaller = context.createUnmarshaller();
        User deserializedUser = (User) unmarshaller.unmarshal(new File("user.xml"));
        System.out.println(deserializedUser.getName());
    }
}
```

---

# Классификация и основные возможности систем контроля версий (СКВ)

### Классификация СКВ:

1. **Локальные СКВ:**
   - Работают на одной машине.
   - Пример: RCS, SCCS.
2. **Централизованные СКВ:**
   - Один центральный сервер, клиенты получают доступ к репозиторию.
   - Пример: Subversion (SVN).
3. **Распределённые СКВ:**
   - У каждого клиента есть копия полного репозитория.
   - Пример: Git, Mercurial.

### Основные возможности:

- Управление версиями кода.
- Совместная работа над проектами.
- Ветвление и слияние изменений.
- Ведение истории изменений.

### Пример команд Git:

```bash
# Инициализация репозитория
git init

# Добавление файлов в индекс
git add .

# Коммит изменений
git commit -m "Initial commit"

# Создание ветки
git branch new-feature

# Слияние ветки
git merge new-feature

# Отправка изменений на удалённый сервер
git push origin main
```

---

# Хостинг репозиториев GitHub и его основные возможности

### GitHub:

- Облачный сервис для хостинга Git-репозиториев.
- Предоставляет возможности:
  - Совместной работы над проектами.
  - Удобного веб-интерфейса для управления репозиториями.
  - Управления задачами (issues) и досками проектов.
  - **GitHub Actions** для автоматизации CI/CD.
  - Управления pull requests.

### Пример:

```bash
# Клонирование репозитория с GitHub
git clone https://github.com/username/repository.git

# Отправка изменений в удалённый репозиторий
git push origin main
```

---

# Применение репозитория Git. Команды системы Git и их применение

### Git:

- **Распределённая система контроля версий** с поддержкой:
  - Управления изменениями в проекте.
  - Создания веток для параллельной разработки.
  - Поддержки совместной работы через удалённые репозитории.

### Основные команды:

```bash
# Инициализация локального репозитория
git init

# Клонирование удалённого репозитория
git clone <url>

# Отслеживание новых файлов
git add <file>

# Коммит изменений
git commit -m "Message"

# Создание новой ветки
git branch <branch_name>

# Переключение между ветками
git checkout <branch_name>

# Слияние веток
git merge <branch_name>
```

# Понятие ветвей Git. Слияние ветвей и хранение копий разрабатываемого ПО.

### Теория:

- **Ветвь (branch):**
  - Механизм для работы над отдельной функциональностью или исправлениями без влияния на основную версию кода.
  - Основная ветвь по умолчанию — `main` (или `master`).
- **Слияние (merge):**
  - Объединение изменений из одной ветки в другую.
  - Возможен автоматический или ручной (при конфликтах) процесс слияния.

### Пример:

```bash
# Создание ветки
git branch feature-branch

# Переключение на новую ветку
git checkout feature-branch

# Слияние изменений в основную ветвь
git checkout main
git merge feature-branch
```

---

# Назначение и возможности Java Persistence API (JPA).

### Теория:

- **JPA (Java Persistence API)** — стандартный API для ORM в Java:
  - Управляет персистенцией объектов в реляционных базах данных.
  - Упрощает взаимодействие с БД через аннотированные классы.
  - Поддерживает транзакции, кэширование, связи между объектами (например, `@OneToMany`, `@ManyToMany`).

### Пример:

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToMany(mappedBy = "user")
    private List<Order> orders;

    // Getters и Setters
}
```

---

# Персистенция объектов в Java. Понятие технологии Object-Relational Mapping (ORM). Сравнение основных возможностей JPA и JDBC.

### Теория:

- **Персистенция:**
  - Процесс сохранения объектов Java в базу данных.
  - Обеспечивает долгосрочное хранение и восстановление объектов.
- **Object-Relational Mapping (ORM):**
  - Связывание объектов Java с реляционными таблицами.
  - Автоматизация создания SQL-запросов для операций CRUD.

### Сравнение JPA и JDBC:

| Функция              | JPA                               | JDBC                         |
| -------------------- | --------------------------------- | ---------------------------- |
| Простота             | Высокая (ORM, аннотации)          | Низкая (SQL-запросы вручную) |
| Производительность   | Зависит от настроек (кэширование) | Выше для сложных операций    |
| Гибкость             | ORM ограничивает сложные запросы  | Полный контроль через SQL    |
| Поддержка транзакций | Встроенная (через EntityManager)  | Реализация вручную           |

### Пример JPA:

```java
@Entity
public class Product {
    @Id
    @GeneratedValue
    private Long id;
    private String name;
    private Double price;

    // Getters и Setters
}

EntityManagerFactory emf = Persistence.createEntityManagerFactory("example-unit");
EntityManager em = emf.createEntityManager();

em.getTransaction().begin();
Product product = new Product();
product.setName("Laptop");
product.setPrice(1200.0);
em.persist(product);
em.getTransaction().commit();
```

### Пример JDBC:

```java
Connection conn = DriverManager.getConnection(DB_URL, USER, PASSWORD);
String sql = "INSERT INTO products (name, price) VALUES (?, ?)";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, "Laptop");
pstmt.setDouble(2, 1200.0);
pstmt.executeUpdate();
```

---

# Применение классов EntityManager и SessionFactory при взаимодействии ПО с источником данных.

### Теория:

- **EntityManager (JPA):**
  - Управляет сущностями (Entity) и их состоянием в базе данных.
  - Отвечает за операции CRUD, запросы и управление транзакциями.
- **SessionFactory (Hibernate):**
  - Создаёт объекты Session для работы с базой данных.
  - Это фабрика для объектов Session, обеспечивающая управление состояниями объектов.

### Пример с EntityManager:

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("example-unit");
EntityManager em = emf.createEntityManager();
em.getTransaction().begin();
User user = new User();
user.setName("Alice");
em.persist(user);
em.getTransaction().commit();
```

### Пример с SessionFactory:

```java
SessionFactory factory = new Configuration().configure("hibernate.cfg.xml").buildSessionFactory();
Session session = factory.openSession();
session.beginTransaction();
User user = new User();
user.setName("Bob");
session.save(user);
session.getTransaction().commit();
session.close();
```

---

# Транзакции и управление состоянием объекта персистенции. Создание Plain Old Java Object (POJO) классов и возможности преобразования их в Entity классы.

### Теория:

- **Транзакции:**
  - Гарантируют выполнение группы операций как одной целостной единицы.
  - Управление через `EntityTransaction` (JPA) или `Transaction` (Hibernate).
- **Состояние объектов:**

  - **Transient:** Объект ещё не сохранён.
  - **Persistent:** Объект привязан к базе данных.
  - **Detached:** Объект отсоединён от контекста персистенции.

- **POJO:**
  - Класс без зависимости от каких-либо библиотек.
  - Становится сущностью через аннотацию `@Entity`.

### Пример POJO → Entity:

```java
@Entity
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private Double price;

    // Getters и Setters
}
```

---

# Hibernate как основная реализация ORM технологии в спецификации JPA. Компоненты Hibernate. Выполнение запросов на основе SQL-подобного языка Hibernate Query Language (HQL).

### Теория:

- **Hibernate:**
  - Популярная реализация JPA, предоставляет расширенные возможности ORM.
  - Основные компоненты:
    - `SessionFactory` и `Session` для работы с базой данных.
    - `Configuration` для настройки.
    - Кэширование для повышения производительности.
- **HQL:**
  - Язык запросов, похожий на SQL, но работающий с сущностями, а не таблицами.

### Пример HQL:

```java
Session session = factory.openSession();
Query query = session.createQuery("FROM Product WHERE price > :price");
query.setParameter("price", 1000.0);
List<Product> products = query.list();
for (Product product : products) {
    System.out.println(product.getName());
}
```

---

# Отладка и тестирование приложения средствами среды разработки, основные плюсы и минусы подхода. Точки Останова при разработке ПО.

### Теория:

- **Плюсы отладки в IDE:**
  - Визуализация выполнения программы.
  - Возможность пошагового выполнения.
  - Быстрое определение ошибок.
- **Минусы:**

  - Зависимость от IDE.
  - Ограниченные возможности для многопоточных приложений.

- **Точки останова:**
  - Позволяют приостановить выполнение программы для анализа состояния.
  - Типы: условные, логические, обычные.

### Пример работы с точками останова:

- Установить точку останова на строке:

```java
System.out.println("Debug here");
```

---

# Модульное тестирование на основе Reflection API и JUnit. Основные возможности фреймворка JUnit.

### Теория:

- **JUnit:**

  - Популярный фреймворк для тестирования Java-кода.
  - Позволяет писать и запускать тесты, обеспечивает интеграцию с CI/CD.

- **Reflection API:**
  - Используется для тестирования приватных методов и полей.

### Пример теста JUnit:

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class CalculatorTest {
    @Test
    void testAddition() {
        Calculator calc = new Calculator();
        assertEquals(5, calc.add(2, 3));
    }
}
```

### Пример Reflection API:

```java
import java.lang.reflect.Method;

public class ReflectionTest {
    public static void main(String[] args) throws Exception {
        Class<Calculator> clazz = Calculator.class;
        Method method = clazz.getDeclaredMethod("privateMethod");
        method.setAccessible(true);
        Calculator calc = new Calculator();
        method.invoke(calc);
    }
}
```

---

# Основные аннотации, применяемые при тестировании ПО с помощью JUnit.

### Теория:

JUnit предоставляет аннотации для управления тестами:

- **`@Test:`** обозначает метод как тест.
- **`@BeforeEach:`** выполняется перед каждым тестом.
- **`@AfterEach:`** выполняется после каждого теста.
- **`@BeforeAll:`** выполняется один раз перед всеми тестами.
- **`@AfterAll:`** выполняется один раз после всех тестов.
- **`@Disabled:`** отключает тест.
- **`@ParameterizedTest:`** для параметризованных тестов.

### Пример:

```java
import org.junit.jupiter.api.*;

public class ExampleTest {
    @BeforeEach
    void setUp() {
        System.out.println("Before each test");
    }

    @Test
    void testExample() {
        Assertions.assertEquals(2, 1 + 1);
    }

    @AfterEach
    void tearDown() {
        System.out.println("After each test");
    }
}
```

---

# Параметризированное и групповое тестирование. Мутационные тесты и их назначение. Техника test-driven development (TDD).

### Теория:

- **Параметризированное тестирование:**

  - Позволяет запускать тесты с различными наборами данных.

- **Групповое тестирование:**

  - Объединение тестов в группы для управления их выполнением.

- **Мутационные тесты:**

  - Изменяют код программы для проверки, насколько хорошо тесты обнаруживают ошибки.

- **TDD (Test-Driven Development):**
  - Подход, при котором сначала пишутся тесты, а затем реализация функциональности.

### Пример параметризованного теста:

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

public class ParameterizedExample {
    @ParameterizedTest
    @ValueSource(ints = {1, 2, 3})
    void testWithParams(int number) {
        Assertions.assertTrue(number > 0);
    }
}
```

---

# Основные уровни OSI и изучаемые протоколы. Прикладной уровень OSI и протокол HTTP. Основные методы протокола HTTP.

### Теория:

- **Уровни OSI:**
  1. Физический
  2. Канальный
  3. Сетевой
  4. Транспортный
  5. Сеансовый
  6. Представительский
  7. Прикладной
- **Протокол HTTP:**
  - Протокол прикладного уровня для передачи данных.
- **Методы HTTP:**
  - **GET:** получение данных.
  - **POST:** отправка данных.
  - **PUT:** обновление данных.
  - **DELETE:** удаление данных.

### Пример:

```java
import java.net.HttpURLConnection;
import java.net.URL;

public class HttpExample {
    public static void main(String[] args) throws Exception {
        URL url = new URL("http://example.com");
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        connection.setRequestMethod("GET");
        int responseCode = connection.getResponseCode();
        System.out.println("Response Code: " + responseCode);
    }
}
```

---

# Java EE сервера и основные возможности предоставляемые такими системами.

### Теория:

- **Java EE сервера (например, WildFly, Tomcat, GlassFish):**
  - Поддержка сервлетов и JSP.
  - Управление транзакциями.
  - JPA для работы с базой данных.
  - Поддержка REST и SOAP веб-сервисов.
  - Реализация CDI (Context and Dependency Injection).

### Пример сервлета:

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class HelloServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        resp.setContentType("text/html");
        resp.getWriter().write("<h1>Hello, World!</h1>");
    }
}
```

---

# Классификация, цели и задачи паттернов проектирования. Порождающие, структурные и поведенческие паттерны.

### Теория:

- **Классификация паттернов:**
  - **Порождающие:**
    - Цель: управление созданием объектов.
    - Примеры: Singleton, Factory, Builder.
  - **Структурные:**
    - Цель: упрощение структуры классов.
    - Примеры: Adapter, Composite, Decorator.
  - **Поведенческие:**
    - Цель: управление взаимодействием между объектами.
    - Примеры: Observer, Strategy, Command.

### Пример Singleton:

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

---

# Понятие web-приложения и его назначение.

### Теория:

- **Web-приложение** — это программа, работающая на сервере и предоставляющая функциональность через веб-интерфейс.
- **Основное назначение:**
  - Доступ к данным через браузер.
  - Взаимодействие с пользователями через интернет.
  - Обеспечение функциональности, такой как интернет-магазины, социальные сети, SaaS.

### Пример технологии:

- **Frontend:** HTML, CSS, JavaScript.
- **Backend:** Java сервлеты, Spring Boot.

## Создание сервлетов (servlet) и обеспечения доступа к ним. Жизненный цикл сервлета.

### Теория:

- **Сервлет** — Java-класс для обработки HTTP-запросов.
- **Жизненный цикл сервлета**:
  1. Инициализация (`init()`).
  2. Обработка запросов (`service()` или `doGet()`, `doPost()`).
  3. Уничтожение (`destroy()`).

### Пример сервлета:

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        resp.setContentType("text/html");
        resp.getWriter().write("<h1>Hello, Servlet!</h1>");
    }
}
```

- Доступ к сервлету обеспечивается через URL-мэппинг в `web.xml`:

```xml
<servlet>
    <servlet-name>HelloServlet</servlet-name>
    <servlet-class>HelloServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>HelloServlet</servlet-name>
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
```

---

## Основные подходы к созданию web-архитектуры приложения. Request и Response обращений.

### Теория:

- **Web-архитектура**:

  - Монолитная архитектура: вся логика в одном приложении.
  - Микросервисы: логика разделена на независимые сервисы.
  - Serverless: функции запускаются по требованию.

- **Request и Response**:
  - **Request**: запрос клиента к серверу.
  - **Response**: ответ сервера.

### Пример обработки запроса и ответа:

```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException {
    String username = req.getParameter("username");
    resp.setContentType("text/html");
    resp.getWriter().write("<h1>Welcome, " + username + "!</h1>");
}
```

---

## Дискриптор развёртывания приложения и возможность маппинга классов. Назначение cookies и cache.

### Теория:

- **Дискриптор развёртывания (`web.xml`)**:

  - XML-файл для настройки веб-приложения.
  - Используется для маппинга URL на классы, настройки фильтров и параметров.

- **Cookies**:
  - Маленькие файлы, хранящие данные о пользователе (например, сессии).
- **Cache**:
  - Хранение временных данных для ускорения повторных запросов.

### Пример `web.xml`:

```xml
<web-app>
    <servlet>
        <servlet-name>MyServlet</servlet-name>
        <servlet-class>com.example.MyServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>MyServlet</servlet-name>
        <url-pattern>/myServlet</url-pattern>
    </servlet-mapping>
</web-app>
```

---

## Паттерн проектирования MVC и его реализация в web-приложении.

### Теория:

- **MVC (Model-View-Controller)**:

  - **Model**: отвечает за данные и бизнес-логику.
  - **View**: отвечает за представление данных (UI).
  - **Controller**: обрабатывает запросы и взаимодействует с Model и View.

- **Реализация MVC**:
  - **Model**: Java-классы (JPA, DAO).
  - **View**: JSP, HTML.
  - **Controller**: Сервлеты, Spring Controllers.

### Пример:

1. **Model**:

```java
public class User {
    private String username;
    private String email;

    // Getters и Setters
}
```

2. **View (JSP)**:

```html
<html>
  <body>
    <h1>Welcome, ${username}!</h1>
  </body>
</html>
```

3. **Controller (Сервлет)**:

```java
public class UserController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setAttribute("username", "Alice");
        req.getRequestDispatcher("/welcome.jsp").forward(req, resp);
    }
}
```

---

## Взаимодействие сервлетов с представлением. Понятие, назначение и цели Java Server Page (JSP). Применение Bootstrap и Thymeleaf для обеспечения представления. Принципы применения JSP Standard Tag Library (JSTL).

### Теория:

- **Взаимодействие сервлетов с представлением**:

  - Сервлеты обрабатывают запросы, бизнес-логику и передают данные представлению (JSP).
  - JSP используется для отображения данных, полученных из сервлета, с помощью атрибутов.

- **Java Server Pages (JSP)**:

  - Расширение HTML с возможностью встраивания Java-кода.
  - **Назначение**:
    - Создание динамических веб-страниц.
    - Упрощение отображения данных, переданных сервлетами.
  - **Цели**:
    - Отделение логики (сервлетов) от представления.

- **Bootstrap**:

  - CSS-фреймворк для адаптивного дизайна и стилизации интерфейсов.
  - Позволяет быстро создавать красивые веб-приложения.

- **Thymeleaf**:

  - Шаблонизатор для Java-приложений, позволяющий связывать данные с HTML-страницами.

- **JSP Standard Tag Library (JSTL)**:
  - Набор стандартных тегов для работы с циклами, условиями, выводом данных.
  - **Принципы**:
    - Избегать Java-кода в JSP.
    - Использовать теги для улучшения читаемости.

### Пример JSP и JSTL:

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<body>
    <h1>Welcome, ${username}!</h1>
    <ul>
        <c:forEach items="${items}" var="item">
            <li>${item}</li>
        </c:forEach>
    </ul>
</body>
</html>
```

---

## Понятие REST и ключевые особенности построения приложения на его основе. Сходства и отличия понятий REST и RESTful. Плюсы построения приложений на основе REST архитектуры. Назначение и обзор принципов SOLID.

### Теория:

- **REST (Representational State Transfer)**:

  - Архитектурный стиль для построения распределённых систем.
  - **Особенности**:
    - Использование HTTP методов (GET, POST, PUT, DELETE).
    - Stateless (отсутствие состояния между запросами).
    - Идентификация ресурсов через URI.

- **REST vs RESTful**:

  - **REST**: общий подход.
  - **RESTful**: корректная реализация принципов REST.

- **Плюсы REST**:

  - Простота и масштабируемость.
  - Кроссплатформенность.
  - Использование стандартов HTTP.

- **Принципы SOLID**:
  - **S**: Single Responsibility Principle (Принцип единственной ответственности).
  - **O**: Open/Closed Principle (Принцип открытости/закрытости).
  - **L**: Liskov Substitution Principle (Принцип подстановки Барбары Лисков).
  - **I**: Interface Segregation Principle (Принцип разделения интерфейсов).
  - **D**: Dependency Inversion Principle (Принцип инверсии зависимостей).

### Пример REST-контроллера:

```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class RestControllerExample {
    @GetMapping("/users")
    public List<String> getUsers() {
        return List.of("Alice", "Bob");
    }
}
```

**Назначение и возможности интеграции фреймворка Spring. Основной контекст (Context) приложения и его роль в системе.**

- **Spring Framework**:
  - Платформа для разработки Java-приложений.
  - Назначение:
    - Упрощение создания приложений.
    - Инверсия управления (IoC).
    - Управление зависимостями.
- **Возможности Spring**:
  - Контейнер IoC.
  - Поддержка AOP (Aspect-Oriented Programming).
  - Интеграция с базами данных, REST API, JPA.
  - Модульная структура (Spring Boot, Spring Data, Spring MVC).
- **Контекст приложения**:
  - Контейнер Spring, управляющий объектами (beans).
  - Роль:
    - Хранение объектов и их зависимостей.
    - Конфигурирование приложения.

Пример контекста:

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class SpringExample {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        MyBean bean = context.getBean(MyBean.class);
        bean.doSomething();
    }
}
```

---

**Понятие и назначение бина (Bean) приложения.**

- **Bean**:
  - Объект, управляемый Spring IoC контейнером.
  - Назначение:
    - Управление зависимостями.
    - Реализация бизнес-логики.

Пример объявления бина:

```java
import org.springframework.stereotype.Component;

@Component
public class MyBean {
    public void doSomething() {
        System.out.println("Doing something...");
    }
}
```

---

**Spring Core и базовые механизмы предоставляемые фреймворком.**

- **Spring Core**:
  - Основа Spring Framework, предоставляющая функции IoC и DI.
- **Базовые механизмы**:
  - **IoC (Inversion of Control)**:
    - Контейнер управляет созданием и жизненным циклом объектов.
  - **DI (Dependency Injection)**:
    - Автоматическое связывание зависимостей.
  - **AOP (Aspect-Oriented Programming)**:
    - Разделение кросс-результатной логики (например, логирование).

Пример DI:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class Service {
    private final Repository repository;

    @Autowired
    public Service(Repository repository) {
        this.repository = repository;
    }

    public void perform() {
        repository.save();
    }
}

@Component
class Repository {
    public void save() {
        System.out.println("Data saved");
    }
}
```

---

**Назначение и цели Inversion of Control (IoC) в Spring. Реализация Dependency Injection (DI).**

- **Inversion of Control (IoC)**:
  - Концепция, в которой управление объектами и их зависимостями передаётся контейнеру (в данном случае Spring IoC).
  - Назначение:
    - Упрощение управления зависимостями.
    - Снижение связности между компонентами.
  - Цели:
    - Улучшение тестируемости и расширяемости приложения.
- **Dependency Injection (DI)**:
  - Способ передачи зависимостей объекту.
  - Реализация в Spring:
    - Конструкторная DI: Зависимости передаются через конструктор.
    - Сеттерная DI: Зависимости передаются через сеттеры.

Пример DI:

1. Конструкторная DI:

```java
@Component
public class Service {
    private final Repository repository;

    @Autowired
    public Service(Repository repository) {
        this.repository = repository;
    }
}
```

2. Сеттерная DI:

```java
@Component
public class Service {
    private Repository repository;

    @Autowired
    public void setRepository(Repository repository) {
        this.repository = repository;
    }
}
```

---

**IoC контейнер и управление жизненным циклом бина.**

- **IoC контейнер**:
  - Сердце Spring, ответственное за создание, управление и уничтожение бинов.
  - Типы контейнеров:
    - BeanFactory: базовый контейнер.
    - ApplicationContext: расширенный контейнер с дополнительными возможностями (события, международная поддержка).
- **Жизненный цикл бина**:
  - Создание бина.
  - Связывание зависимостей.
  - Вызов методов инициализации.
  - Использование бина.
  - Вызов методов уничтожения.

Пример жизненного цикла:

```java
@Component
public class MyBean {
    @PostConstruct
    public void init() {
        System.out.println("Bean is initialized");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("Bean is being destroyed");
    }
}
```

---

**Основные концептуальные решения создания бина приложения.**

- **Способы создания бинов**:
  1. **Аннотации**:
     - @Component, @Service, @Repository, @Controller.
  2. **Конфигурационные классы**:
     - Использование аннотации @Configuration и метода с @Bean.
  3. **XML-конфигурация**:
     - Определение бинов в файле applicationContext.xml.

Пример с аннотацией:

```java
@Component
public class Service {
    public void execute() {
        System.out.println("Service is executing...");
    }
}
```

Пример с Java-конфигурацией:

```java
@Configuration
public class AppConfig {
    @Bean
    public Service service() {
        return new Service();
    }
}
```

---

**Конфигурация бина приложения на основе XML файла.**

- XML-конфигурация позволяет задавать бины и их зависимости.
- Поддерживается в Spring для обеспечения совместимости и гибкости.

Пример XML-конфигурации:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="service" class="com.example.Service">
        <property name="repository" ref="repository"/>
    </bean>
    <bean id="repository" class="com.example.Repository"/>
</beans>
```

Пример класса:

```java
public class Service {
    private Repository repository;

    public void setRepository(Repository repository) {
        this.repository = repository;
    }
}
```

---

**Надстройка Spring Boot и её основные возможности. Конфигурация бина через основные аннотации Spring Boot.**

- **Spring Boot**:
  - Надстройка Spring, упрощающая создание приложений.
  - Основные возможности:
    - Встроенные серверы (Tomcat, Jetty).
    - Автоконфигурация.
    - Поддержка сторонних библиотек.
    - Управление зависимостями через starter-пакеты.
- **Аннотации для конфигурации бинов**:
  - @SpringBootApplication: запускает приложение и сканирует компоненты.
  - @Component, @Service, @Repository, @Controller: автоматическое создание бинов.
  - @Bean: ручная регистрация бинов.

Пример Spring Boot:

```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}

@Service
public class MyService {
    public void process() {
        System.out.println("Processing...");
    }
}
```

---

**Реализация приложений на основе Spring Boot и Spring MVC.**

- **Spring Boot**:
  - Упрощает создание приложений на основе Spring, включая MVC.
  - Автоконфигурация компонентов для быстрого старта.
  - Встроенные серверы (Tomcat, Jetty).
- **Spring MVC**:
  - Модель-Вид-Контроллер (MVC) для разделения логики, данных и представления.
  - Включает DispatcherServlet, маршрутизацию запросов и интеграцию с представлением (Thymeleaf, JSP).

Пример приложения Spring Boot + MVC:

1. **Контроллер**:

```java
@RestController
@RequestMapping("/api")
public class MyController {
    @GetMapping("/greet")
    public String greet() {
        return "Hello, Spring MVC!";
    }
}
```

2. **Запуск приложения**:

```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```

---

**Создание классов Controller и привязка к контексту ПО.**

- **Контроллеры**:
  - Управляют обработкой HTTP-запросов и передачей данных представлению.
  - Привязываются к контексту через аннотацию @Controller или @RestController.
- **Аннотации**:
  - @RequestMapping: задаёт базовый путь для всех методов.
  - @GetMapping, @PostMapping: маршруты для HTTP-методов.

Пример контроллера:

```java
@Controller
@RequestMapping("/web")
public class WebController {
    @GetMapping("/home")
    public String homePage(Model model) {
        model.addAttribute("message", "Welcome to the Home Page!");
        return "home";
    }
}
```

---

**Особенности интеграции Spring Data в ПО и сравнение с Hibernate. Ключевые аспекты реализации классов CRUDRepository и JPARepository.**

- **Spring Data**:
  - Абстракция для работы с базами данных.
  - Основана на JPA, упрощает реализацию CRUD-операций.
- **Hibernate**:
  - ORM-реализация JPA.
  - Уровень ниже, чем Spring Data (прямая работа с сущностями и HQL).
- **CRUDRepository**:
  - Базовый интерфейс Spring Data для операций CRUD.
  - Методы: save, findById, delete.
- **JpaRepository**:
  - Расширяет CRUDRepository дополнительными методами:
    - Пагинация.
    - Сортировка.

Пример CRUDRepository:

```java
@Repository
public interface UserRepository extends CrudRepository<User, Long> {
    List<User> findByLastName(String lastName);
}
```

Пример JpaRepository:

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
    List<Product> findByCategory(String category, Pageable pageable);
}
```

---

**Возможности применения Spring Security при разграничении прав доступа пользователей системы.**

- **Spring Security**:
  - Фреймворк для обеспечения безопасности приложений.
  - Возможности:
    - Аутентификация (логин).
    - Авторизация (разграничение прав).
    - Поддержка OAuth2, JWT.
- **Применение**:
  - Конфигурация через аннотации:
    - @EnableWebSecurity: включает безопасность.
    - @PreAuthorize: ограничивает доступ к методам.
  - Интеграция с базами данных для хранения пользователей и ролей.

Пример конфигурации безопасности:

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/admin/**").hasRole("ADMIN")
                .antMatchers("/user/**").hasRole("USER")
                .anyRequest().authenticated()
            .and()
            .formLogin()
            .and()
            .logout();
    }
}
```

Пример использования роли в методах:

```java
@Service
public class AdminService {
    @PreAuthorize("hasRole('ADMIN')")
    public void performAdminTask() {
        System.out.println("Admin task performed");
    }
}
```

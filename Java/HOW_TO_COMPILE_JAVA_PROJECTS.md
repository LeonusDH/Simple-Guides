# Как компилировать Java проекты

### Как определить тип проекта?

Если в корне проекта есть файл **pom.xml**, то это **Maven** проект.

Если в корне проекта есть файл **gradlew**, то это **Gradle** проект.

# Компилируем Maven проект

Для компиляции **Maven** проекта, надо установить **Maven** или **Maven-Wrapper**

## Если **Maven**

Скачиваем **Maven**: https://maven.apache.org/index.html
Открываем в папке проекта, где лежит `pom.xml` CMD и вводим:

```bash
mvn package
```

## Если **Maven-Wrapper** (лень ставить **Maven**)

Скачиваем **Maven-Wrapper**: https://github.com/takari/maven-wrapper
Копируем файлы `mvnw`, `mvnw.cmd` и папку `.mvn` в корень проекта.

Затем выполняем эту команду:

```bash
mvnw.cmd package
```

> Готовый проект будет в папке `target`

# Компилирует Gradle Проект

## Windows

Открываем в папке проекта CMD и вводим:

```bash
gradlew.bat build
```

## Linux

Открываем в папке проекта CMD и вводим:

```bash
gradlew build
```

> Готовый проект будет в папке `build/libs`

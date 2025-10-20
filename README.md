
```markdown
# pyuniq-demo

Це проста реалізація утиліти `uniq` (яка фільтрує або підраховує однакові суміжні рядки) на Python з використанням бібліотеки `click`.

Цей проєкт створено як навчальний приклад для пакування CLI-інструментів, їх публікації на PyPI та збірки Docker-образів.

**❗️ Важливо:** Як і оригінальна утиліта `uniq`, `pyuniq-demo` працює коректно лише з **відсортованими** даними. Перед використанням `pyuniq-demo` завжди пропускайте ваші дані через системну команду `sort`.

## Встановлення

Ви можете встановити цей пакет безпосередньо з PyPI:

```bash
# Замініть 'pyuniq-demo' на вашу унікальну назву, опубліковану на PyPI
pip install pyuniq-demo
````

## Використання

Для прикладів створимо тестовий файл:

```bash
echo -e "Apple\nBanana\napple\nBanana\nOrange\nApple\nBanana" > fruits.txt
```

### 1\. Базове використання (потрібен `sort`)

Якщо запустити `pyuniq-demo` на невідсортованому файлі, результат буде некоректним. Завжди сортуйте дані спочатку.

```bash
# Завжди сортуйте дані перед передачею в pyuniq-demo!
cat fruits.txt | sort
```

**Вивід `sort`:**

```
Apple
Apple
apple
Banana
Banana
Banana
Orange
```

Тепер передамо цей результат у `pyuniq-demo`:

```bash
cat fruits.txt | sort | pyuniq-demo
```

**Фінальний вивід:**

```
Apple
apple
Banana
Orange
```

### 2\. Опція `-c` (підрахунок)

Прапор `-c` або `--count` додає префікс з кількістю повторень кожного рядка.

```bash
cat fruits.txt | sort | pyuniq-demo -c
```

**Вивід:**

```
      2 Apple
      1 apple
      3 Banana
      1 Orange
```

### 3\. Опція `-i` (ігнорувати регістр)

Прапор `-i` або `--ignore-case` вважає `Apple` та `apple` одним і тим же.

```bash
# Ми також використовуємо 'sort -f', щоб сортувати, ігноруючи регістр
cat fruits.txt | sort -f | pyuniq-demo -i
```

**Вивід:**

```
Apple
Banana
Orange
```

### 4\. Комбінація `-c` та `-i`

Найпотужніший варіант — підрахувати рядки, ігноруючи регістр.

```bash
cat fruits.txt | sort -f | pyuniq-demo -c -i
```

**Вивід:**

```
      3 Apple
      3 Banana
      1 Orange
```

## Використання з Docker

Ви також можете зібрати та запустити цей інструмент як Docker-контейнер.

```bash
# 1. Зберіть образ
docker build -t pyuniq-demo .

# 2. Запустіть контейнер (покаже довідку)
docker run --rm pyuniq-demo

# 3. Передайте дані в контейнер через stdin
# Не забувайте сортувати дані ПЕРЕД передачею в контейнер!
cat fruits.txt | sort -f | docker run --rm -i pyuniq-demo -c -i
```

**Вивід:**

```
      3 Apple
      3 Banana
      1 Orange
```

```
```
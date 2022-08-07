# Всё про компонент

## Файловая структура компонента

1. tests - папка для тестов. В одном файле не должна собираться портянка из тестов. Один файл - один тест.
2. helpers - папка для вспомогательных функций, которые используются в текущем компоненте.
3. components - папка для компонентов, использующихся внутри текущего компонента.
4. types - папка для описания типов текущего компонента.
5. index.tsx - основной файл компонента.
6. styles.ts, styles.module.css, styles.module.scss, ... - основной файл стилей
7. constants - папка для констант.

Пример файловой структуры компонента для контента роутера:

```
routes-content
  route-name
    contentFirst
      __tests__
        testName.test.ts|tsx
      components
      helpers
        index.ts
      types
        index.ts
      index.tsx
      style.ts
    contentSecond
      __tests__
        testName.test.ts|tsx
      components
      helpers
        index.ts
      types
        index.ts
      index.ts
      style.ts

```

## Порядок импортов

1. Внешние библиотеки (node_modules);
2. Сторы, селекторы;
3. Файлы с путями алиасов (@components/... ; @constants/... ; @shared/... ; @typesTS/...);
4. Файлы с путями от текущего файла (./types ; ./helpers, ./styles.css);

## Порядок сущностей в функциональной части

1. Стейты;
2. Cелекторы;
3. Константы;
4. Функции;
5. Эффекты;
6. Return;

Перед return всегда ставим пробел для читаемости.

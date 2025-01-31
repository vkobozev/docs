# Действия (actions)

## DEFINE ACTION {#define-action}

Позволяет объявить действие (action), которое представляет собой параметризуемый блок из нескольких выражений верхнего уровня (statements), и затем многократно его использовать с помощью [DO](#do).

После `DEFINE ACTION` указывается:

1. [Именованное выражение](expressions.md#named-nodes), по которому объявляемое действие будет доступно далее в запросе.
2. В круглых скобках — список именованных выражений, по которым внутри действия можно обращаться к параметрам.
3. Ключевое слово `AS`.
4. Сам список выражений верхнего верхнего уровня.
5. `END DEFINE` выступает в роли маркера, обозначающего последнее выражение внутри действия.

## DO {#do}

Выполняет действие (action) с указанными параметрами. После `DO` указывается:

1. Именованное выражение, по которому объявлено действие.
2. В круглых скобках — список значения для использования в роли параметров.

`EMPTY_ACTION` — ключевое слово, обозначающее ничего не выполняющее действие.

{% note info %}

В больших запросах объявление действий можно выносить в отдельные файлы и подключать их в основной запрос с помощью EXPORT + IMPORT, чтобы вместо одного длинного текста получилось несколько логических частей, в которых проще ориентироваться. Важный нюанс: для объявления работающих с таблицами действий в отдельной библиотеке, в ней обязательно должно присутствовать `USE my_cluster;`, т.к. их компиляция зависит от типа кластера.

{% endnote %}

Пример:

```sql
DEFINE ACTION $hello_world($name) AS
    $name = $name ?? "world";
    SELECT "Hello, " || $name || "!";
END DEFINE;

DO EMPTY_ACTION();
DO $hello_world(NULL);
DO $hello_world("John");
```

# Условное и циклическое выполнение действий

## EVALUATE IF {#evaluate-if}

Выполняет действие (action) в зависимости от выполнения условия. После `EVALUATE IF` указывается:

1. Условие.
2. [DO](#do) с именем и параметрами действия.
3. Опционально `ELSE` и следом второе `DO` для ситуации, когда условие не выполнено.

## EVALUATE FOR {#evaluate-for}

Выполняет действие (action) для каждого элемента в списке. После `EVALUATE FOR` указывается:

1. [Именованное выражение](expressions.md#named-nodes), в которое будет подставляться каждый очередной элемент списка.
2. Ключевое слово `IN`.
3. Объявленное выше именованное выражение со списком, по которому будет выполняться действие.
4. [DO](#do) с именем и параметрами действия, в параметрах можно использовать как текущий элемент из первого пункта, так и любые объявленные выше именованные выражения, в том числе сам список.
5. Опционально `ELSE` и следом второе `DO` для ситуации, когда список пуст.

Примеры:

```sql
DEFINE ACTION $hello() AS
    SELECT "Hello!";
END DEFINE;

DEFINE ACTION $bye() AS
    SELECT "Bye!";
END DEFINE;

EVALUATE IF RANDOM(0) > 0.5
    DO $hello()
ELSE
    DO $bye();
```

```sql
-- скопировать таблицу $input в $count новых таблиц
$count = 3;
$input = "my_input";
$inputs = ListReplicate($input, $count);
$outputs = ListMap(
    ListFromRange(0, $count),
    ($i) -> {
        RETURN "tmp/out_" || CAST($i as String)
    }
);
$pairs = ListZip($inputs, $outputs);

DEFINE ACTION $copy_table($pair) as
   $input = $pair.0;
   $output = $pair.1;
   INSERT INTO $output WITH TRUNCATE
   SELECT * FROM $input;
END DEFINE;

EVALUATE FOR $pair IN $pairs
    DO $copy_table($pair)
ELSE
    DO EMPTY_ACTION(); -- такой ELSE можно было не указывать,
                       -- ничего не делать подразумевается по умолчанию
```

{% note info %}

Стоит учитывать, что `EVALUATE` выполняется до начала работы операции. Также в рамках `EVALUATE` невозможно использование анонимных таблиц.

{% endnote %}

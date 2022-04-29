# Привязки

Следующие переменные и функции доступны только в коде JavaScript.

## Переменные

### HOST

имя хоста (ранее доступное как переменная `GAME`).  Возможные значения включают `gta3`, `vc`, `re3`, `reVC`, `sa`, `gta3_unreal`, `vc_unreal`, `sa_unreal`, `unknown`. 

Плагины CLEO могут использовать SDK для настройки имени под свои нужды.

```js
if (HOST === "gta3") {
  showTextBox("This is GTA III");
}
if (HOST === "sa") {
  showTextBox("This is San Andreas");
}
if (HOST === "unknown") {
  showTextBox("This host is not natively supported");
}
```

### ONMISSION 

глобальный флаг, определяющий, находится ли игрок в данный момент на задании.  Недоступно на «неизвестном» хосте.

```js
if (!ONMISSION) {
  showTextBox("Not on a mission. Setting ONMISSION to true");
  ONMISSION = true;
}
```

### TIMERA, TIMERB

два автоматически увеличивающихся таймера, полезных для измерения временных интервалов.

```js
while (true) {
  TIMERA = 0;
  // wait 1000 ms
  while (TIMERA < 1000) {
    wait(0);
  }
  showTextBox("1 second passed");
}
```

### __dirname 

абсолютный путь к каталогу с текущим файлом

### __filename 

(начиная с 0.9.4) абсолютный путь к текущему файлу

## Функции

### log

`log(...значения)` печатает разделенные запятыми `{значения}` в `cleo_redux.log`

```js
var x = 1;
log("value of x is ", x);
```

### wait 

`wait(время в миллисекундах)` приостанавливает выполнение скрипта как минимум на `{время в миллисекундах}` миллисекунд

```js
while (true) {
  wait(1000);
  log("1 second passed");
}
```

### showTextBox

`showTextBox(текст)` отображает `{текст}` в черном прямоугольном поле.  Недоступно на `неизвестном` хосте.

```js
showTextBox("Hello, world!");
```

### exit

`exit(причина?)` немедленно завершает скрипт.  Функция `exit` принимает необязательный строковый аргумент, который будет добавлен в `cleo_redux.log`.

```js
exit("Script ended");
```

### native

`native(имя команды, ...входные аргументы)` это низкоуровневая функция для выполнения команды с использованием ее имени `{имя команды}`. Имя команды соответствует свойству `name` в файле JSON, предоставленном библиотекой Sanny Builder.

```js
native("SET_TIME_OF_DAY", 12, 30); // устанавливает время суток на 12:30
```

Для команд, возвращающих одно значение, результатом является это значение.

```js
const progress = native("GET_PROGRESS_PERCENTAGE");
showTextBox(`Progress is ${progress}`);
```

Для команд, возвращающих несколько значений, результатом является объект, в котором каждый ключ соответствует возвращаемому значению. Имена ключей соответствуют именам выходов, указанным в определении команды.

```js
var pos = native("GET_CHAR_COORDINATES", char); // возвращает вектор координат символа {x, y, z}
showTextBox(`Character pos: x ${pos.x} y ${pos.y} z ${pos.z}`);
```

Для условных команд результатом является логическое значение `true` или `false`.

```js
if (native("HAS_MODEL_LOADED", 101)) {
  // проверяет условие
  showTextBox("Model with id 101 has been loaded");
}
```

## Статические объекты 

### Memory

- Объект `Memory` позволяет манипулировать памятью процесса. См. [Руководство по памяти](using-memory.md) для получения дополнительной информации.

### Math

- Объект `Math` — это стандартный объект, доступный в среде выполнения JS, который обеспечивает общие математические операции. CLEO Redux расширяет его некоторыми дополнительными командами.  См. [Объект Math](using-math.md) для получения дополнительной информации.

### FxtStore

- `FxtStore` позволяет обновлять содержимое внутриигровых текстов.  Подробнее см. в руководстве [Пользовательский текст](./using-fxt.md).

### CLEO

- (начиная с 0.9.4) Объект `CLEO` предоставляет доступ к информации и утилитам во время выполнения:

  - `CLEO.debug.trace(флаг)` включает и выключает трассировку команд в текущем скрипте. Когда {флаг} имеет значение true, все выполненные команды добавляются в `cleo_redux.log`:
  ```js
    CLEO.debug.trace(true)
    wait(50);
    const p = new Player(0);
    CLEO.debug.trace(false)
  ```

  - `CLEO.version` - сложное свойство, предоставляющее информацию о текущей версии библиотеки

  ```js
    log(CLEO.version)       // "0.9.4-dev.20220427"
    log(CLEO.version.major) // "0"
    log(CLEO.version.minor) // "9"
    log(CLEO.version.patch) // "4"
    log(CLEO.version.pre)   // "dev"
    log(CLEO.version.build) // "20220427"
  ```
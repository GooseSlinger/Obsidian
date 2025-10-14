# Краткий гайд по `errors.Is` и `errors.As` (Go)

`errors.Is` и `errors.As` работают с **цепочками ошибок**, созданными через `fmt.Errorf("...: %w", err)` или `errors.Join`.

---

## TL;DR
- **`errors.Is(err, targetErr)`** — проверяет наличие **конкретной (sentinel) ошибки** в цепочке (например, `io.EOF`, `context.DeadlineExceeded`, ваши `var Err...`).
- **`errors.As(err, &target)`** — ищет в цепочке **ошибку определённого типа** и записывает её в `target` (например, `*json.SyntaxError`, `*json.UnmarshalTypeError`, `*os.PathError`) — чтобы прочитать **поля**.

---

## 1) Формируем цепочку (важно: `%w` только в `fmt.Errorf`)
```go
package main

import (
	"errors"
	"fmt"
)

var ErrNotFound = errors.New("not found")

func repoFind() error {
	return ErrNotFound
}

func svcGet() error {
	if err := repoFind(); err != nil {
		return fmt.Errorf("svc get user: %w", err) // добавляем контекст, сохраняем первопричину
	}
	return nil
}
```

---

## 2) `errors.Is`: сравнение с sentinel
```go
err := svcGet()
if errors.Is(err, ErrNotFound) {
	// true — в цепочке есть именно ErrNotFound
	// решение ветки: например, вернуть 404
}
```

---

## 3) `errors.As`: сопоставление по типу + доступ к полям
```go
package main

import (
	"errors"
	"fmt"
	"os"
)

func openFile() error {
	_, err := os.Open("no-such.txt")
	return fmt.Errorf("wrap: %w", err)
}

func main() {
	var pathErr *os.PathError
	if err := openFile(); err != nil {
		if errors.As(err, &pathErr) {
			fmt.Println("op:", pathErr.Op, "path:", pathErr.Path) // доступ к полям типовой ошибки
		}
	}
}
```

---

## 4) Когда `Is`, а когда `As` — шпаргалка
- Есть **константная «маячковая» ошибка** (`var ErrX`) → **`Is`**.
- Нужны **поля типовой ошибки** (`Offset`, `Field`, `Path`, `Op` и т.п.) → **`As`**.
- Можно комбинировать: если нужны поля — сначала `As`, иначе `Is`.

---

## 5) Частые кейсы в веб-хендлерах
```go
var syn *json.SyntaxError
var typ *json.UnmarshalTypeError

switch {
case errors.As(err, &syn):
	jsonResponder.BadRequest(fmt.Sprintf("Невалидный JSON (байт %d)", syn.Offset))
case errors.As(err, &typ):
	jsonResponder.BadRequest(fmt.Sprintf("Поле %q: ожидается %s", typ.Field, typ.Type))
case errors.Is(err, io.EOF):
	jsonResponder.BadRequest("Пустое тело запроса")
case errors.Is(err, context.DeadlineExceeded):
	jsonResponder.GatewayTimeout("Превышен таймаут")
default:
	jsonResponder.InternalError()
}
```

---

## 6) Быстрые заметки
- `Is/As` сами проходят всю цепочку через `Unwrap()`.
- Не теряй первопричину: оборачивай через `%w` в `fmt.Errorf("...: %w", err)`.
- `errors.Join(a, b)` агрегирует несколько причин; `Is` вернёт `true`, если совпала **любая**.

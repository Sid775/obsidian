[[Java]]
Exceptions structure in Java:
```plantuml
class Throwable
class Error
class Exception
class RuntimeException

Throwable <-- Error
Throwable <-- Exception
Exception <-- RuntimeException
```

Ошибки делятся на checked и unchecked, первые должны быть обязательно обработаны либо при помощи try-catch-finally либо прокинуты выше при помощи throws на методе. Вторые могут быть проигнорированы и прокинутся на верх автоматически.
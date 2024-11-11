[[Patterns]]

Синглтон это порождающий паттерн смысл которого гарантированно предоставить единственный экземпляр класса и глобальную точку доступа к нему.

Есть несколько способов реализации:
1. Классическая реализация - закрываем конструктор модификатором private и сразу же создаем экземпляр в статическом поле
```
	class Singleton {
		private static Singleton instance = new Singleton();
		
		private Singleton() {
		}
		
		public static Singleton getInstance() {
			return instance;
		}
	}
```
Имеет недостатки связанные с тем что экземпляр создается не лениво а создание может стоит дорого при том что он не пригодится никогда.

2. Ленивая реализация - смысл тот же но в методе getInstance мы проверяем создан ли  экземпляр, если он null то создаем.
```
class Singleton {
		private static Singleton instance;
		
		private Singleton() {
		}
		
		public static Singleton getInstance() {
			if(instance == null){
				instance = new Singleton();
			}
			return instance;
		}
	}
```
Основные недостатки связанны с многопоточностью, такая реализация не является потокобезопасной.
3. Внутренний класс
```
	class Singleton {
		private static class SingletonHolder {
			private static final Singleton instance = new Singleton();
		}
		
		private Singleton() {
		}
		
		public static Singleton getInstance() {
			return SingletonHolder.instance;
		}
	}
```
Проблема ленивой инициализации решена но осталась проблема с обработкой исключений в конструкторе.
4. Синхронизация - ультимативный но очень геморройный способ
```
class Singleton {
		private static volatile Singleton instance;
		
		private Singleton() {
		}
		
		public static Singleton getInstance() {
			if(instance == null){
				synchronized (Singletone.class) {
					if(instance == null){
						instance = new Singleton();
					}
				}
			}
			return instance;
		}
	}
```
Volitile создает проблемы с производительностью а без него написанный здесь double-checked locking работать не будет из за dead code elimination который применяет javac к этому коду.
6. ENUM
```
enum Singleton {
	Instance;
```
Не очень понятно зачем :)

Глобальные проблемы:
1. В распределенных системах типа [[JINI]], [[RMI]] EJB? могут быть созданы несколько экземпляров, когда распределение кода по различным джава машинам скрыта от пользователя и может быть неочевидна.
2. При загрузке разными класслоадерами может быть создано несколько инстансов, например некоторые контейнеры сервлетов грузят каждый сервлет отдельным класслоадером.
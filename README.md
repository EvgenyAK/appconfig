# appconfig

## Описание

* Чтение/Редактирование опций из конфигурационных фалов разных форматов
* Объектное представление конфига
* Обновление однотивных опций для всех конфигов (например, **log_level**)

## Примеры использования

###Связывание опций 2 разных конфигов

```python
import appconfig

config = appconfig.Config("config_path")

@appconfig.option
def log_level(value=None):
    appconfig.service_a.log_level = value
    appconfig.service_b.common.logger.log_level = value

config.log_level = 'debug'
# or
config.log_level('debug')
```




# appconfig

## Описание

* Чтение/Редактирование опций из конфигурационных фалов разных форматов
* Объектное представление конфига
* Обновление однотивных опций для всех конфигов (например, **log_level**)

## Примеры использования

### Загрузка разных типов файлов

```python
import appconfig

json_conf = appconfig.Config("/etc/some/some.conf")
yaml_conf = appconfig.Config("/etc/some/some.yaml")
ini_conf = appconfig.Config("/etc/some/some.ini")
py_conf = appconfig.Config("/etc/some/some.py")

# свой формат
def lala(file):
    return {'option': 'lala'}

any_conf = appconfig.Config("/etc/some/some.lala", loader=lala)
```

### Объектное представление 

```python
import appconfig

config = appconfig.Config("/etc/some/some.conf")

config.logger.level = 'info'
config['logger']['level'] = 'info'
config.save()
```

### Связывание опций разных конфигов

```python
import appconfig

config = appconfig.Config(
    service_a="/etc/service_a/config.conf",
    service_b="/etc/service_b/config.conf"
)

@config.option
def log_level(value=None):
    config.service_a.log_level = value
    config.service_b.common.logger.log_level = value

config.log_level = 'debug'
# or
config.log_level('debug')
```

### Сохранение промежуточных версий

```python

config.save()
# возвращение к первоначальным настройкам
config.reset()

config.log_level = 'trace'
config.save(version='v1')
config.log_level = 'info'
config.reset(version='vi')
config.log_level
>>> 'debug'
```

### Изменение опций

```python
# измненить все опции подпадающие под запрос
config.log_level = appconfig.as_xpath('.//logger/*/data', config.service_a)
config.log_level = appconfig.as_re('info', config.service_b)

# изменение опций в нескольких конфигах
config.service_a.log_level = 'info'
config.service_a.log_level = 'info'

# or
config.log_level = appconfig.as_xpath('.//logger/*/data', config.service_a, config.service_b)

# or
config.group(config.service_a, config.service_b).log_level = 'info'
goup_level = config.group(config.service_a, config.service_b)
goup_level.log_level = 'info'
```


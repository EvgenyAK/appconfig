## Примеры использования

### Загрузка разных типов файлов

```python
import appconfig

manger = appconfig.ConfigManager().init(
  config1="path/to/config1",
  config2="path/to/config2",
  configN="path/to/configN"
)
```

### Объектное представление

```python
import appconfig

config = appconfig.Config("/etc/some/some.conf")

config.logger.level = 'info'
config['logger']['level'] = 'info'
config.save()

# or

import appconfig

manger = ConfigManager().init(
  config1="path/to/config1",
  config2="path/to/config2",
  configN="path/to/configN"
)

manager.config1.logger.level = 'info'

manager.config("config2").section("logger").level = 'info'

config3 = manager.config("config3")
config3.logger.level = 'info'

configN = manager.config("configN")
confign_logger = configN.section("logger")
configN_logger.level = 'info'
```
### Связывание опций разных конфигов

```python
import appconfig

manger = ConfigManager().init(
  config1="path/to/config1",
  config2="path/to/config2",
  configN="path/to/configN"
)

@manager.group
def log_level(value=None):
    config.service_a.log_level = value
    config.service_b.common.logger.log_level = value

config.log_level('debug')
```

### Сохранение промежуточных версий

```python
manager.config.save()
# or
manager.save_all()

# возвращение к первоначальным настройкам
manager.config.reset()
config = manager.config("config)

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
config.log_level = appconfig.as_dpath('/*/logger/*/data', "error", config.service_a)
```

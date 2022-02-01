# Postagres

При первом запуске контейнера инициализируется хранилище БД и запускаются скрипты(sql и sh) из каталога `/docker-entrypoint-initdb.d` С помощью конфигмапа создаем там скрипт инициализации `postgres_init.sh` который подготавливает sql-файл(вписывает туда имя создаваемой БД, пользователя и пароль), потом загружает этот файл.

Параметру `PGDATA` указываем не стандартный путь `/var/lib/postgresql/data/pgdata` потому что постгрес будет менять владельца этого каталога, а этот какталог у нас монтируется как внешний том и с этим могут быть проблемы. Таким образом том монтируем по стандартному пути `/var/lib/postgresql/data`, а постгрес свои базы инициализирует в подкаталоге `pgdata`

# Mattermost

```
 HEALTHCHECK &{["CMD-SHELL" "curl -f http://localhost:8065/api/v4/system/ping || exit 1"] "30s" "10s" "0s" '\x00'}
```



Starting from Mattermost v3.8, you can use environment variables to manage the configuration. Environment variables override settings in `config.json`. If a change to a setting in `config.json` requires a restart for it to take effect, then changes to the corresponding environment variable also require a server restart.

The name of the environment variable for any setting can be derived from the name of that setting in `config.json`. For example, to derive the name of the Site URL setting:

1. Find the setting in `config.json`. In this case, *ServiceSettings.SiteURL*.
2. Add `MM_` to the beginning and convert all characters to uppercase and replace the `.` with `_`. For example, *MM_SERVICESETTINGS_SITEURL*.
3. The setting becomes `export MM_SERVICESETTINGS_SITEURL="http://example.com"`.

```
docker run --rm -p 8065:8065 --env DB_HOST=1.1.1.1 --env MM_USERNAME=mmuser --env MM_PASSWORD=123 --env MM_DBNAME=mattermost mattermost/mattermost-enterprise-edition:6.1 mattermost
```

Создать секрет с сертификатом:

```
kubectl create secret tls mattermost-tls --cert=tls\demo-tls.crt --key=tls\demo-tls.key
```


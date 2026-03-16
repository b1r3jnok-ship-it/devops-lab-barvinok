University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Введение в веб технологии](https://itmo-ict-faculty.github.io/introduction-in-web-tech/)
Year: 2025/2026
Group: U4125
Author: Barvinok Vsevolod Vladimirovich
Lab: Lab2
Date of create: 13.03.2026
Date of finished: 16.03.2026

Ход работы:
Настройка мониторинга с Prometheus и Grafana:

Создание конфигурации Prometheus:

Создать папку prometheus для конфигурации
Создать файл prometheus/prometheus.yml со следующим содержимым: global: scrape_interval: 15s
scrape_configs:
      - job_name: 'prometheus'
      static_configs:
      - targets: ['localhost:9090']

      - job_name: 'node-exporter'
      static_configs:
      - targets: ['node-exporter:9100']
Запуск Node Exporter:

Запустить контейнер Node Exporter для сбора системных метрик:

docker run -d \
      --name node-exporter \
      --restart=unless-stopped \
      -p 9100:9100 \
      -v "/proc:/host/proc:ro" \
      -v "/sys:/host/sys:ro" \
      -v "/:/rootfs:ro" \
      prom/node-exporter \
      --path.procfs=/host/proc \
      --path.rootfs=/rootfs \
      --path.sysfs=/host/sys \
      --collector.filesystem.mount-points-exclude="^/(sys|proc|dev|host|etc)($$|/)"
Проверить работу: curl http://localhost:9100/metrics

Запуск Prometheus:

Создать том для данных Prometheus:

docker volume create prometheus-data
Prometheus - приложение, которое отвечает за сбор метрик. Для их визуализации позже будет запущено другое приложение - Grafana. Чтобы они могли работать вместе, нужно создать для них общую сеть:

docker network create monitoring
Выйти на папку выше в консоли, убедиться что вы находитесь над уровнем папки prometheus. Запустить контейнер Prometheus:

docker run -d \
      --name prometheus \
      --network monitoring \
      --restart=unless-stopped \
      -p 9090:9090 \
      -v prometheus-data:/prometheus \
      -v $(pwd)/prometheus:/etc/prometheus \
      prom/prometheus \
      --config.file=/etc/prometheus/prometheus.yml \
      --storage.tsdb.path=/prometheus \
      --web.console.libraries=/etc/prometheus/console_libraries \
      --web.console.templates=/etc/prometheus/consoles \
      --storage.tsdb.retention.time=200h \
      --web.enable-lifecycle
Проверить работу: открыть http://localhost:9090 в браузере

В случае неполадок можно запустить команду docker logs prometheus. Там будет выведена ошибка, указывающая на причину неработающего контейнера. В случае обнаружения подобной ошибки рекомендуется попробовать починить ее самостоятельно.
Запуск Grafana:

Создать том для данных Grafana:

docker volume create grafana-data
Запустить контейнер Grafana:

docker run -d \
      --name grafana \
      --network monitoring \
      --restart=unless-stopped \
      -p 3000:3000 \
      -v grafana-data:/var/lib/grafana \
      -e "GF_SECURITY_ADMIN_PASSWORD=admin" \
      grafana/grafana
Проверить работу: открыть http://localhost:3000 в браузере (логин: admin, пароль: admin)

Настройка Grafana:

Войти в Grafana (admin/admin)
Добавить источник данных Prometheus:
Configuration → Data Sources → Add data source
Выбрать Prometheus
URL: http://prometheus:9090
Save & Test
Создать дашборд:
Create → Dashboard → Add visualization
Выбрать источник данных Prometheus
Добавить метрику: node_cpu_seconds_total
Сохранить дашборд
Тестирование системы:

Проверить все контейнеры: docker ps
Открыть Prometheus и убедиться, что метрики собираются
Открыть Grafana и проверить отображение графиков
Создать несколько графиков для разных метрик (CPU, память, диск)

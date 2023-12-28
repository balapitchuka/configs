# Apache Airflow

## References
- [DATA ENGINEERING WITH AIRFLOW AND SINGER](https://francescao.com/2021/06/data-engineering-with-airflow-singer/)
- [Airflow on AWS using ECS](https://medium.com/@abhishekranjandev/deploying-airflow-on-aws-for-large-scale-c0e28ab31abf)


## Scheduling Airflow to run automatically (Linux only)

- create airflow-scheduler.service in  /etc/systemd/system
  
  ```bash
  # airflow-scheduler.service

  [Unit]
  Description=Airflow scheduler daemon
  After=network.target postgresql.service mysql.service redis.service rabbitmq-server.service
  Wants=postgresql.service mysql.service redis.service rabbitmq-server.service

  [Service]
  EnvironmentFile=/etc/sysconfig/airflow
  User=airflow
  Group=airflow
  Type=simple
  ExecStart=/bin/airflow scheduler
  Restart=always
  RestartSec=5s

  [Install]
  WantedBy=multi-user.target
  ```
- create airflow-webserver.service file in /etc/systemd/system
  
  ```bash
  # airflow-webserver.service

  [Unit]
  Description=Airflow webserver daemon
  After=network.target postgresql.service mysql.service redis.service rabbitmq-server.service
  Wants=postgresql.service mysql.service redis.service rabbitmq-server.service

  [Service]
  EnvironmentFile=”PATH=/home/ubuntu/python/envs/airflow/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
  User=airflow
  Group=airflow
  Type=simple
  ExecStart=/bin/airflow webserver --pid /run/airflow/webserver.pid
  Restart=on-failure
  RestartSec=5s
  PrivateTmp=true

  [Install]
  WantedBy=multi-user.target
  ```

# Домашнее задание к занятию "`Курсовая работа на профессии "DevOps-инженер с нуля"`" - `Кудряшов Андрей`

---

Оснавная задача - разработать отказоустойчивую инфраструктуру для сайта, включающую мониторинг, сбор логов и резервное копирование основных данных.
Согласно заданию: https://github.com/netology-code/fops-sysadm-diplom/blob/main/README.md

Условия:
* Инфраструктура должна размещаться в Yandex Cloud. 
* Мониторинг будет осуществляться при помощи Prometheus.
* Развёртывание инфраструктуры будет производиться при помощи Terraform и Ansible.
* Сбор и визуализация логов - ELK.
* Должена быть ВМ Bastion для ssh управления.

PS: Это вторая версия проекта, первая вот тут: https://github.com/Andrew-Kud/netology_hw_coursework_1
Перенёс по причине переписывания проекта и ошибки в gitlab push которую я не смогу побороть (да и сил особо уже небыло после такого проекта очередную багу лечить).
---

## Этап 1: Terraform

Этапы создания инфраструктуры:
https://docs.google.com/document/d/1P5oxvzPCOwivDtUC_eOXqr6bg8z3JbDDaJ_FwJa9pSo/edit?usp=sharing

Схема:
<img width="970" height="1064" alt="terra_3" src="https://github.com/user-attachments/assets/d2deed61-74fe-4fc5-ae2d-422171c9ca20" />


Результаты:
До terraform apply.
<img width="2560" height="865" alt="terra_1" src="https://github.com/user-attachments/assets/d775f9c8-004b-4767-a614-349c65c7f526" />

После terraform apply.
<img width="2554" height="1434" alt="terra_2" src="https://github.com/user-attachments/assets/60d75543-1388-41a1-a489-652ba27ecd70" />

PS: Часть комментраиев и объяснений оставил прямо в коде Terrafor.

---

## Этап 2: Ansible
Фазы создания инфраструктуы:
https://docs.google.com/document/d/1H_H4-0hFsnig3ZOqEAFB9HAY8Y-OmewdWCG0BEIc67o/edit?usp=sharing


Ping:
<img width="629" height="878" alt="1_ping-all" src="https://github.com/user-attachments/assets/93c7faec-934a-4714-9388-79a3d05aeff0" />


Node and Nginx exporter:
<img width="1050" height="469" alt="2_web_node_and_nginx-exporter" src="https://github.com/user-attachments/assets/20d25d78-b092-45f9-a3fc-13f9a428b542" />


Prometheus:
<img width="2553" height="572" alt="3_prometheus" src="https://github.com/user-attachments/assets/15129096-c0b0-4540-8ddc-d272869bf7c7" />

Prometheus metric:
<img width="1917" height="1036" alt="4_prometheus mecrics" src="https://github.com/user-attachments/assets/e899b0b5-ea83-4e64-8576-424d1685f021" />
<img width="2559" height="1439" alt="4_prometheus mecrics2" src="https://github.com/user-attachments/assets/5a1cbdfa-5235-491a-8680-3127e2b84a03" />


Grafana:
<img width="1926" height="1089" alt="5_grafana install and web" src="https://github.com/user-attachments/assets/9581f59d-d291-4434-a071-917be1877a82" />

Grafana metric:
<img width="1921" height="1080" alt="6_grafana web nginx metric" src="https://github.com/user-attachments/assets/3bff7379-9c61-4853-be7b-6cb5346bd69c" />
<img width="1925" height="1079" alt="6_grafana web node exporter" src="https://github.com/user-attachments/assets/a893938c-c38a-471b-aac5-f0f768d90dbb" />

Grafana stress:
<img width="2547" height="1439" alt="7_grafana metrics" src="https://github.com/user-attachments/assets/190de042-3f4d-41e5-9b19-0b9eb77ef265" />


Elasticsearch:
<img width="1946" height="1090" alt="8_elasticsearch_health" src="https://github.com/user-attachments/assets/be9d97d3-82b6-400f-9ab2-6127c0c80702" />


Kibana:
<img width="1932" height="1088" alt="8_kibana" src="https://github.com/user-attachments/assets/5d55cf70-89cd-4b62-a0a0-9d99dfa51967" />

Kibana auto-log:
<img width="1920" height="1079" alt="8_kibana_auto-log" src="https://github.com/user-attachments/assets/ec77b4aa-7926-430b-8dc0-d37c84ff739a" />


---
## Итоги:
<img width="2559" height="1439" alt="2025-11-24_15-47" src="https://github.com/user-attachments/assets/49741480-b85d-46da-9f8b-81d2dda03ecb" />

<img width="1278" height="879" alt="2025-11-24_15-47_1" src="https://github.com/user-attachments/assets/359d5913-f6f5-4f23-8b2f-42d1aadb2a35" />


Требования:                                  | Статус: | Комментарий:
1 - Две ВМ в разных зонах + Nginx            | ✅      | web-b (ru-central1-b) + web-d (ru-central1-d), идентичные, все настроено
2 - ALB + Target/Backend Group + HTTP Router | ✅      | Полностью реализовано, healthcheck настроен, балансинг работает
3 - Prometheus + Node/Nginx Exporters        | ✅      | 4 типа exporters (node, log, stub), 5 jobs, все targets в статусе UP
4 - Grafana + Dashboards + Thresholds        | ✅      | 4 pre-provisioned dashboards, все метрики включены (CPU, RAM, Disk, Network, HTTP)
5 - Elasticsearch + Kibana + Filebeat        | ✅      | Полная цепь: access.log + error.log → Filebeat → ES → Kibana
6 - VPC + Security Groups + Bastion Host     | ✅      | Правильная сегментация сети, публичные/приватные подсети, ProxyCommand SSH
7 - Snapshots на 7 дней                      | ✅      | Ежедневное расписание, 7-дневный retention, все 7 дисков включены

---
Инфраструктура:
Terraform:
VPC сеть (10.0.0.0/8)
3 подсети в разных зонах
7 ВМ с их IP адресами и портами
ALB с Target Group, Backend Group, HTTP Router
NAT Gateway для приватных ВМ
Security Groups правила доступа
Snapshot расписание

Ansible:
4 Playbooks (base, webservers, monitoring, logging)
9 Roles с их структурой (tasks, templates, handlers, files)
Инвентарь (hosts.ini)

Взаимодействие:
User → ALB → Web servers (HTTP:80)
User → Grafana (HTTP:3000)
User → Kibana (HTTP:5601)
User → Bastion (SSH:22)
Prometheus → Web exporters (scrape каждые 15 сек)
Grafana → Prometheus (query каждые 30 сек)
Filebeat → Elasticsearch (send logs realtime)
Kibana → Elasticsearch (query logs)
Snapshots → Daily backup (2:00 UTC)



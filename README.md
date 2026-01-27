# EN

### Environment & Application Profile

Our application operates within a multi-zone Kubernetes cluster spanning three availability zones, comprising a total of five worker nodes.

### Key Application Characteristics:

- **Initialization:** Requires a 5-10 second startup period.
- **Performance & Scaling:** Load testing confirms that four pod replicas are sufficient to handle the projected peak traffic load.
- **Resource Utilization Pattern:**
	- **CPU:** Exhibits a significant spike during the initial request processing, subsequently stabilizing at a consistent baseline of approximately 0.1 CPU cores.
	- **Memory:** Consumption is stable and predictable, consistently around 128 MiB.
- **Traffic Pattern:** Features a distinct diurnal cycle, with daytime peak traffic exceeding nighttime load by an order of magnitude.

### Deployment Objectives:
- **Maximize Resilience:** Achieve the highest possible level of fault tolerance and availability within the given cluster topology.
- **Optimize Resource Efficiency:** Minimize the total resource footprint of the deployment while fully meeting performance and scalability requirements.

# RUS

## Описание проекта:

### Описание среды выполнения и характеристик приложения

Приложение развернуто в мультизональном Kubernetes-кластере, распределенном по трём зонам доступности на инфраструктуре из пяти узлов.

### Профиль приложения:

- **Время инициализации:** 5–10 секунд.
- **Масштабируемость:** Результаты нагрузочного тестирования показывают, что для обработки пиковой нагрузки достаточно четырёх реплик Pod.
- **Паттерн потребления ресурсов:**
  - **CPU:** Наблюдается выраженный всплеск потребления при обработке первых запросов с последующей стабилизацией на постоянном уровне около 0.1 ядра.
  - **Память:** Потребление стабильно и предсказуемо, находится в районе 128 MiB.
- **Профиль нагрузки:** Нагрузка имеет ярко выраженный суточный цикл: дневной пик превышает ночную минимальную нагрузку на порядок.

### Цели конфигурации Deployment:
- **Максимизировать отказоустойчивость:** Обеспечить бесперебойную работу приложения, используя возможности мультизональной архитектуры кластера для достижения высокой доступности.
- **Оптимизировать эффективность ресурсов:** Спроектировать конфигурацию, которая гарантирует выполнение SLA при минимальном потреблении вычислительных ресурсов, адаптируясь к естественным циклам нагрузки.

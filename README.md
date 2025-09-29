EN

Project Description:

We have a multi-zone cluster (three zones) with five nodes.

The application requires about 5-10 seconds for initialization.

Based on the results of load testing, it is known that 4 pods can handle the peak load.

The application requires significantly more CPU resources for the initial requests; subsequent consumption is stable at around 0.1 CPU. Memory usage is always "steady" at around 128M.

The application has a daily load cycle – the number of requests is orders of magnitude lower at night, with a peak during the day.

We want a highly failure-tolerant deployment.

We want minimal resource consumption from this deployment.


RUS

Описание проекта: 


у нас мультизональный кластер (три зоны), в котором пять нод
приложение требует около 5-10 секунд для инициализации
по результатам нагрузочного теста известно, что 4 пода справляются с пиковой нагрузкой
на первые запросы приложению требуется значительно больше ресурсов CPU, в дальнейшем потребление ровное в районе 0.1 CPU. По памяти всегда “ровно” в районе 128M memory
приложение имеет дневной цикл по нагрузке – ночью запросов на порядки меньше, пик – днём
хотим максимально отказоустойчивый deployment
хотим минимального потребления ресурсов от этого deployment’а

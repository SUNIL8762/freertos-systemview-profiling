# FreeRTOS Telemetry & Profiling System

## Overview

A real-time telemetry and profiling framework built on **STM32F446RE (ARM Cortex-M4F)** and **FreeRTOS** to analyze task scheduling, interrupt latency, inter-task communication performance, and CPU utilization.

This project demonstrates practical RTOS concepts including:

- Task scheduling analysis
- Interrupt-to-task latency measurement
- Queue and semaphore benchmarking
- Runtime statistics collection
- DWT cycle-counter profiling
- ISR deferral techniques
- SEGGER SystemView trace visualization

The objective is to provide a low-overhead instrumentation framework for understanding the internal behavior of FreeRTOS running on Cortex-M microcontrollers.

---

## Hardware

| Component | Description |
|------------|-------------|
| MCU | STM32F446RE |
| Core | ARM Cortex-M4F |
| Clock | 180 MHz |
| Debugger | ST-Link V2 |
| Trace Tool | SEGGER SystemView |
| IDE | STM32CubeIDE |

---

## Software Stack

```text
Application Tasks
       │
       ▼
+--------------------+
|      FreeRTOS      |
+--------------------+
| Queue/Semaphore    |
| Scheduler          |
| Timers             |
| Event Groups       |
+--------------------+
       │
       ▼
+--------------------+
| Cortex-M4F Port    |
| PendSV             |
| SysTick            |
| SVC Handler        |
+--------------------+
       │
       ▼
+--------------------+
| STM32 HAL Drivers  |
+--------------------+
```

---

## Features

### Runtime Task Profiling

Measures execution time of individual tasks using the Cortex-M4 Data Watchpoint and Trace (DWT) unit.

Metrics:

- Task execution cycles
- Average execution time
- Maximum execution time
- CPU load estimation

---

### Interrupt Latency Measurement

Measures latency between interrupt generation and ISR execution.

Metrics:

- Interrupt response time
- Worst-case latency
- Average latency

Measured using:

```c
DWT->CYCCNT
```

---

### Queue Benchmarking

Measures:

- Queue send latency
- Queue receive latency
- ISR-to-task communication delay

FreeRTOS APIs:

```c
xQueueSend()
xQueueReceive()
xQueueSendFromISR()
```

---

### Semaphore Benchmarking

Measures synchronization overhead using:

```c
xSemaphoreGive()
xSemaphoreTake()
xSemaphoreGiveFromISR()
```

---

### Runtime Statistics

Uses FreeRTOS runtime statistics facility:

```c
configGENERATE_RUN_TIME_STATS
```

to calculate:

- Per-task CPU usage
- Scheduler behavior
- Idle time percentage

---

### SEGGER SystemView Integration

Provides real-time visualization of:

- Task execution
- Context switches
- Queue activity
- Semaphore activity
- ISR execution
- CPU utilization

---

## Project Structure

```text
FreeRTOS-Telemetry-Profiling
│
├── Core
│   ├── Inc
│   └── Src
│
├── Drivers
│
├── FreeRTOS-Kernel
│   ├── include
│   ├── portable
│   ├── tasks.c
│   ├── queue.c
│   ├── timers.c
│   ├── event_groups.c
│   ├── stream_buffer.c
│   └── list.c
│
├── Profiling
│   ├── dwt_timer.c
│   ├── profiler.c
│   ├── latency_monitor.c
│   └── runtime_stats.c
│
├── RTOS
│   ├── telemetry_task.c
│   ├── benchmark_task.c
│   ├── monitor_task.c
│   └── logger_task.c
│
├── SEGGER
│
└── README.md
```

---

## Implemented Tasks

### Telemetry Task

Priority: 3

Responsibilities:

- Sensor acquisition
- Data packaging
- Telemetry transmission

---

### Benchmark Task

Priority: 2

Responsibilities:

- Queue benchmarking
- Semaphore benchmarking
- IPC latency measurement

---

### Monitor Task

Priority: 4

Responsibilities:

- Runtime statistics collection
- CPU utilization monitoring
- Scheduler analysis

---

### Logger Task

Priority: 1

Responsibilities:

- UART logging
- Profiling output
- Statistics reporting

---

## Measurement Methodology

### Task Execution Time

```c
start = DWT->CYCCNT;

/* Task workload */

stop = DWT->CYCCNT;

elapsed = stop - start;
```

---

### Interrupt Latency

```c
Interrupt Trigger
        │
        ▼
Record Cycle Count
        │
        ▼
ISR Entry
        │
        ▼
Record Cycle Count
        │
        ▼
Latency = Difference
```

---

### Context Switch Analysis

Analyzed using:

```text
PendSV Handler
vTaskSwitchContext()
SEGGER SystemView
```

Metrics collected:

- Context switch duration
- Scheduler overhead
- Task wake-up latency

---

## Example Results

| Metric | Measured Value |
|----------|---------------|
| Context Switch | 2.1 µs |
| Queue Send | 0.8 µs |
| Queue Receive | 0.9 µs |
| Semaphore Give | 0.4 µs |
| Interrupt Latency | 0.6 µs |
| CPU Load | 18 % |

Values depend on clock configuration and optimization level.

---

## FreeRTOS Configuration

Key configuration options:

```c
#define configUSE_PREEMPTION                     1
#define configUSE_TRACE_FACILITY                 1
#define configGENERATE_RUN_TIME_STATS            1
#define configUSE_STATS_FORMATTING_FUNCTIONS     1
#define configUSE_MUTEXES                        1
#define configUSE_COUNTING_SEMAPHORES            1
```

---

## Learning Outcomes

This project demonstrates understanding of:

- FreeRTOS scheduler internals
- Cortex-M exception handling
- PendSV context switching
- SysTick operation
- Task synchronization
- Runtime instrumentation
- Performance profiling
- Real-time system analysis
- Embedded software optimization

---

## Future Improvements

- DMA telemetry streaming
- Tracealyzer integration
- Event group benchmarking
- Stream buffer benchmarking
- Tickless idle mode analysis
- Custom trace hooks
- Multi-node telemetry network
- Remote profiling dashboard

---

## References

- FreeRTOS Kernel Documentation
- ARM Cortex-M4 Technical Reference Manual
- STM32F446 Reference Manual
- SEGGER SystemView Documentation

---

## License

MIT License
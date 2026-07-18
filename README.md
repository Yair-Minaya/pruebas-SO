# TP Laboratorio - Plug & Pray

Guía de pasos para levantar el TP en la VM del laboratorio.
link de git list : https://tinyurl.com/tp-so

---

## 1. Requisitos previos

- Abrir **PuTTY** — si en 2 PCs salta la misma IP, reiniciar PuTTY
- **VM encendida y con internet** — en `Configuración → Red`, tener "Conectar a" en **Adaptador puente**
- **Token de GitHub:** `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2`
- **Doc de pruebas:** [Google Docs](https://docs.google.com/document/d/1RXiJoIydnxEMyeMWZWnnxbLWWjYCIl3kHoA0mw322bA/edit?tab=t.0)

---

## 2. Clonar los repositorios

**Repos de referencia:**

| Repo | Link |
|---|---|
| TP | `https://github.com/sisoputnfrba/tp-2026-1c-Error-220` |
| Commons | `https://github.com/sisoputnfrba/so-commons-library` |
| Pruebas | `https://github.com/sisoputnfrba/plug-n-pray-pruebas` |
| so-deploy | `https://github.com/sisoputnfrba/so-deploy` |uebas |

---

## 3. Compilar

```bash
./compilar_tp.sh
```
**si usas so-deploy :**
```bash
./so-deploy/deploy.sh \
  -l=sisoputnfrba/so-commons-library \
  -l=sisoputnfrba/plug-n-pray-pruebas \
  -c=LISTEN_IP=0.0.0.0 \
  -c=KM_IP=192.168.x.xxx \
  -c=KS_IP=192.168.x.xxx \
  -c=MS_IP=192.168.x.xxx \
  -p=utils -p=cpu -p=io -p=kernel_memory -p=kernel_scheduler -p=memory_stick -p=swap \
  tp-2026-1c-Error-220
```
---

## 4. Pruebas

| Prueba | Algoritmo | Sticks | Pseudocódigo |
|---|---|---|---|
| 4.1 Base | CMN `[FIFO,RR,FIFO,RR]` | 1 × 256 | `PLANI_PRE_0.prc` · `MEMORIA_PRE_0.prc` |
| 4.2 Corto Plazo | CMN `[FIFO,RR,RR,RR]` | 1 × 256 | `PCP.prc` |
| 4.3 Memoria | RR | 16, 32, 64, 128 | `PLANI_MEM.prc` |
| 4.4 Herencia Prioridades | CMN `[FIFO × 6]` | 16, 16 | `PHP.prc` |
| 4.5 Mediano Plazo | CMN `[FIFO × 4]` | 16, 16, 32, 64 | `PMP.prc` |

### 4.1 Prueba Base

**Config — `kernel_scheduler/ks.config`**

```ini
PLANIFICATION_ALGORITHM=CMN
QUEUES_ALGORITHMS=[FIFO,RR,FIFO,RR]
RR_QUANTUM=600
QUEUE_PREEMPTION=TRUE
SUSPENSION_TIMEOUT=60000
```

**Config — `kernel_memory/km.config`**

```ini
SEGMENT_MAX_SIZE=128
ALLOCATION_STRATEGY=BEST
INSTRUCTION_DELAY=250
COMPACTION_DELAY=30000
SCRIPTS_BASEPATH=/home/utnso/plug-n-pray-pruebas
```

**Memory Stick:** 1 stick — tamaño 256 (`MEMORY_DELAY=1500`)

**Pseudocódigo:**
- Parte 1 → `PLANI_PRE_0.prc`
- Parte 2 → `MEMORIA_PRE_0.prc`

### 4.2 Prueba de Corto Plazo (PCP)

**Config — `kernel_scheduler/ks.config`**

```ini
PLANIFICATION_ALGORITHM=CMN
QUEUES_ALGORITHMS=[FIFO,RR,RR,RR]
RR_QUANTUM=1500
QUEUE_PREEMPTION=TRUE
SUSPENSION_TIMEOUT=35000
```

**Config — `kernel_memory/km.config`**

```ini
SEGMENT_MAX_SIZE=256
ALLOCATION_STRATEGY=BEST
INSTRUCTION_DELAY=500
COMPACTION_DELAY=30000
SCRIPTS_BASEPATH=/home/utnso/plug-n-pray-pruebas
```

**Memory Stick:** 1 stick — tamaño 256

**Pseudocódigo:** `PCP.prc`

### 4.3 Prueba de Memoria

**Config — `kernel_scheduler/ks.config`**

```ini
PLANIFICATION_ALGORITHM=RR
QUEUES_ALGORITHMS=No interesa
RR_QUANTUM=1500
QUEUE_PREEMPTION=TRUE
SUSPENSION_TIMEOUT=35000
```

**Config — `kernel_memory/km.config`**

```ini
SEGMENT_MAX_SIZE=128
ALLOCATION_STRATEGY=BEST
INSTRUCTION_DELAY=500
COMPACTION_DELAY=30000
SCRIPTS_BASEPATH=/home/utnso/plug-n-pray-pruebas
```

> **Dos corridas:**
> - Parte 1 → `ALLOCATION_STRATEGY=BEST`
> - Parte 2 → `ALLOCATION_STRATEGY=WORST`

**Memory Stick:** 4 sticks — tamaños 16, 32, 64, 128

**Pseudocódigo:** `PLANI_MEM.prc`

### 4.4 Prueba Herencia de Prioridades (PHP)

**Config — `kernel_scheduler/ks.config`**

```ini
PLANIFICATION_ALGORITHM=CMN
QUEUES_ALGORITHMS=[FIFO,FIFO,FIFO,FIFO,FIFO,FIFO]
RR_QUANTUM=1500
QUEUE_PREEMPTION=TRUE
SUSPENSION_TIMEOUT=1000000
```

> **Ojo:** `SUSPENSION_TIMEOUT` altísimo — en esta prueba no queremos que nadie se suspenda.

**Config — `kernel_memory/km.config`**

```ini
SEGMENT_MAX_SIZE=128
ALLOCATION_STRATEGY=BEST
INSTRUCTION_DELAY=500
COMPACTION_DELAY=30000
SCRIPTS_BASEPATH=/home/utnso/plug-n-pray-pruebas
```

**Memory Stick:** 2 sticks — tamaños 16 y 16

**Pseudocódigo:** `PHP.prc`

### 4.5 Prueba de Mediano Plazo (PMP)

**Config — `kernel_scheduler/ks.config`**

```ini
PLANIFICATION_ALGORITHM=CMN
QUEUES_ALGORITHMS=[FIFO,FIFO,FIFO,FIFO]
RR_QUANTUM=1500
QUEUE_PREEMPTION=TRUE
SUSPENSION_TIMEOUT=10000
```

**Config — `kernel_memory/km.config`**

```ini
SEGMENT_MAX_SIZE=128
ALLOCATION_STRATEGY=BEST
INSTRUCTION_DELAY=500
COMPACTION_DELAY=30000
SCRIPTS_BASEPATH=/home/utnso/plug-n-pray-pruebas
```

**Memory Stick:** 4 sticks — tamaños 16, 16, 32, 64

> **Atención al STDIN:** cuando el IO STDIN pida "Ingrese 16 caracteres", escribí 16 caracteres + Enter en esa terminal. Pasa varias veces (PID 1 pide 4 veces; PID 2 y 3, dos cada uno). **No demores: el timing es ajustado.**

**Pseudocódigo:** `PMP.prc`

**Mirar el archivo binario del Swap:**

```bash
find /tmp ~/tmp -name "*.bin" 2>/dev/null
xxd file.bin
```

---

## 5. Levantar los módulos

> **Importante:** el orden importa. Una terminal por módulo.

**5.1 — Kernel Memory**

```bash
./bin/kernel_memory km.config
```

**5.2 — Swap**

```bash
./bin/swap swap.config
```

**5.3 — Kernel Scheduler**

```bash
./bin/kernel_scheduler ks.config [archivo pseudocódigo]
```

**5.4 — Memory Sticks**

```bash
./bin/memory_stick ms.config [tamaño] [id]
```

**5.5 — IO (las 3)**

```bash
./bin/io io.config [tipo de io]
```

**5.6 — CPU**

```bash
./bin/cpu cpu.config 1
```

---

## 6. Ver logs

```bash
grep "log buscado" modulo.log
```

> Usar `|` para separar los logs que vas a buscar.

---

## 7. Limpiar logs después de cada prueba

```bash
cd ~/tp-2026-1c-Error-220
rm -f */*.log */bin/*.log /tmp/tp_so_swapfile.bin
```

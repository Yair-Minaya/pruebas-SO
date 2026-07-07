repo del tp : https://github.com/sisoputnfrba/tp-2026-1c-Error-220.git
usuario : Yair-Minaya

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2

repo de pruebas : https://github.com/sisoputnfrba/plug-n-pray-pruebas.git
link de doc de pruebas : https://faq.utnso.com.ar/plug-n-pray-pruebas

________________________________________________________________________________

comandos de ejecucion:

Consola 1:
cd kernel_memory
./bin/kernel_memory km.config 

Consola 2:
cd swap
./bin/swap swap.config

Consola 3:
cd kernel_scheduler
./bin/kernel_scheduler ks.config proceso_inicial.txt

Consola 4:
cd memory_stick
./bin/memory_stick ms.config 1024 1

Consola 5:
cd memory_stick
./bin/memory_stick ms.config 1024 2

Consola 6 — IO tipo SLEEP (este debe ejecutarse antes que CPU porque la CPU quiere usar la operación SLEEP en su archivo de pseudocódigo):
cd io
./bin/io io.config SLEEP

Consola 7 — IO tipo STDIN:
cd io
./bin/io io.config STDIN

Consola 8 — IO tipo STDOUT:
cd io
./bin/io io.config STDOUT

Consola 9:
cd cpu
./bin/cpu cpu.config 1

________________________________________________________________________________

comandos de valgrind: 

Terminal 1 — Kernel Memory (siempre primero, es el servidor base):
bashcd kernel_memory
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes \
  --log-file=valgrind_km.log ./bin/kernel_memory km.config

Terminal 2 — Kernel Scheduler:
bashcd kernel_scheduler
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes \
  --log-file=valgrind_ks.log ./bin/kernel_scheduler ks.config proceso_inicial.txt

Terminal 3 — Swap:
bashcd swap
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes \
  --log-file=valgrind_swap.log ./bin/swap swap.config

Terminal 4 — Memory Stick:
bashcd memory_stick
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes \
  --log-file=valgrind_ms.log ./bin/memory_stick ms.config 1024

Terminal 5 — CPU:
bashcd cpu
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes \
  --log-file=valgrind_cpu.log ./bin/cpu cpu.config 1

Terminal 6 — IO (una por dispositivo, ej. SLEEP):
bashcd io
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes \
  --log-file=valgrind_io.log ./bin/io io.config SLEEP

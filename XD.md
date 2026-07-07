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

comando de valgrind: 

valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes --log-file=valgrind_xx.log 



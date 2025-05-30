# Tarea 3
#!/bin/bash

# --- INSTALACIÓN AUTOMÁTICA DE DEPENDENCIAS ---
echo "Actualizando sistema e instalando herramientas necesarias..."
apt update && apt install -y procps coreutils grep gawk

# --- INICIO DEL MONITOREO ---
echo "Iniciando monitoreo de recursos por 5 minutos (cada 60s)..."
echo "Tiempo(s) %CPU_Usado %Memoria_Libre %Disco_Libre" > monitoreo.txt

for ((i=60; i<=300; i+=60))
do
    cpu=$(top -bn1 | grep "Cpu(s)" | awk '{print 100 - $8}')
    mem=$(free | grep Mem | awk '{print ($4/$2)*100}')
    disk=$(df / | grep / | awk '{print 100 - $5}' | tr -d '%')

    echo "$i $cpu $mem $disk" >> monitoreo.txt
    sleep 60
done

echo "Monitoreo completado. Ver archivo monitoreo.txt"

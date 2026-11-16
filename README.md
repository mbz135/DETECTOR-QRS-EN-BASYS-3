# DETECTOR-QRS-EN-BASYS-3

Este repositorio contiene los códigos del proyecto para un detector de complejo QRS implementado en la tarjeta Basys 3, utilizando el lenguaje de descripción de hardware **Verilog** y el software **Vivado**.

El proyecto procesa señales electrocardiográficas (ECG) para detectar los picos R y calcular la frecuencia cardíaca (BPM), mostrando los resultados en la pantalla de 7 segmentos de la FPGA.

## 📁 Descripción de los archivos

A continuación se detalla la función de cada archivo dentro del repositorio:

*   **`QRS1.txt`**: Archivo de diseño en Verilog. Está pensado para realizar pruebas funcionales simuladas junto con el archivo `SIMULACION`. **Nota:** Esta versión detecta el complejo QRS, pero *no* realiza el cálculo del BPM.
*   **`QRS-FINAL.txt`**: Archivo de diseño principal en Verilog. Es la versión definitiva que se carga en la tarjeta **Basys 3**. A diferencia de `QRS1`, esta versión **sí calcula el BPM** y lo muestra en la pantalla de 7 segmentos de la FPGA.
*   **`SIMULACION.txt`**: Archivo de simulación (Testbench). Se utiliza junto con `QRS1.txt` para probar el diseño antes de sintetizarlo en la tarjeta.
*   **`Datos_Basys1.txt`**: Código en Python. Se debe ejecutar para remuestrear los datos descargados de la base de datos PhysioNet. Los registros de ECG con los que se trabajó en este proyecto fueron: **100, 106, 119 y 200**.
*   **`rom_ecg_4seg.mem`**: Archivo de memoria que contiene los datos del ECG ya remuestreados (correspondientes a 4 segundos de señal). Son los datos reales con los que trabajará la tarjeta Basys 3.
*   **`Constraints-QRS-FINAL.txt`**: Archivo de restricciones (XDC). Contiene el mapeo de los pines para conectar los puertos del diseño Verilog con los periféricos físicos de la tarjeta Basys 3.

## ⚙️ Flujo de trabajo del proyecto

1.  **Procesamiento de datos:** Se utiliza `Datos_Basys1.txt` (Python) para tomar las señales de PhysioNet y generar el archivo `rom_ecg_4seg.mem`.
2.  **Pruebas:** Se simula el diseño `QRS1.txt` con el archivo `SIMULACION.txt` para verificar que la lógica de detección del pico R funcione correctamente.
3.  **Implementación final:** Se sintetiza el archivo `QRS-FINAL.txt` en Vivado, se configuran los pines con el archivo de *Constraints* y se carga en la Basys 3. La FPGA leerá la memoria `rom_ecg_4seg.mem` y mostrará el BPM calculado en los displays de 7 segmentos.

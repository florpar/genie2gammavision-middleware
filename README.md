# genie2gammavision-middleware
A middleware adapter for data acquisition and calibration in Genie 2000, allowing cross compatibility of generated spectrum files in Gammavision using Cambio.

# 🔄 Conversión Genie 2000 → GammaVision  
### Scripts y Jobs para integración de espectros (.CNF → .SPC)

Este repositorio contiene los **scripts batch** y **archivos .JOB** utilizados para automatizar la conversión de espectros adquiridos con **Genie 2000 (Canberra)** al formato **.SPC** utilizado por **GammaVision (Ortec)**.

El objetivo es permitir que los espectros generados por sistemas basados en **Inspector/Genie 2000** puedan ser procesados sin intervención manual dentro del flujo de trabajo de GammaVision.

---

# 🎯 Funcionalidad

Los componentes incluidos permiten:

- Convertir espectros **.CNF → .SPC** automáticamente.
- Ejecutar secuencias de procesamiento encadenadas.
- Aplicar o restaurar calibraciones energéticas.
- Ejecutar análisis desde .JOB dentro de GammaVision.
- Controlar el flujo de archivos y borrado/creación temporal.
- Estandarizar la operación entre ambos softwares.
---

# 📁 Archivos incluidos

## 🟦 Scripts Batch (`*.bat`)

### `cambiar.bat`
Realiza la conversión principal del archivo .CNF usando **Cambio**.

### `genieconcambio.bat`
Coordina la secuencia:
1. lectura del .CNF  
2. conversión  
3. guardado del .SPC resultante  

### `cambiarcali.bat`
Aplica o intercambia archivos de calibración energética previos a la conversión.

### `borracalib.bat`
Elimina archivos de calibración temporales para evitar contaminación de corridas previas.

### `geniecalib2.bat`
Ejecuta un ciclo de calibración seguido por conversión.

---

## 🟧 Archivos JOB (`*.job`)

### `jobgenieconcambio.job`
Job ejecutado desde GammaVision que:
- recibe el archivo convertido
- restaura calibración o aplica una nueva
- carga automáticamente el .SPC para análisis

### `CalG2k.job`
Job específico para corridas que **requieren recalibración energética**.  
Incluye pasos de:
- ECAL
- carga de archivo .CTF
- guardado del resultado

---

## 📗 Archivos de calibración

### `Eu-152.CTF`
Archivo de referencia para **calibración gamma** utilizado por Genie 2000.  
Contiene las energías certificadas del Eu-152 para ajuste automático.

---

# 🧬 Flujo de trabajo

El sistema opera así:
```bash
Genie 2000 adquiere → genera .CNF

Script batch se ejecuta automáticamente

Cambio convierte .CNF → .SPC

Scripts aplican / restauran calibración

GammaVision abre el .SPC usando un archivo .JOB

Se continúa el análisis dentro de GammaVision

```

Todo el proceso puede ejecutarse sin intervención del usuario.

---

# ▶ Uso dentro de GammaVision

1. Abrir:
```bash
Services → Job Control
```
2. Seleccionar el job correspondiente:
- `jobgenieconcambio.job`  
- `CalG2k.job` (si se requiere recalibración)
3. Ejecutar el job.  
4. El espectro convertido aparecerá cargado automáticamente.

---

# 📌 Requisitos

- GammaVision  
- Genie 2000  
- Herramienta **Cambio** instalada  
- Acceso a los archivos del repositorio en:
```bash
C:\ProgramControl\
```

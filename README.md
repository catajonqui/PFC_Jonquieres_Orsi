# VentrIA: Aplicación de apoyo diagnóstico para la clasificación de etiologías de HVI en imágenes de ECG

Proyecto Final de Carrera de Bioingeniería (ITBA, 2026).

<table>
  <tr><td><b>Autoras</b></td><td>Catalina Jonquieres · Victoria Orsi</td></tr>
  <tr><td><b>Tutora</b></td><td>Bioing. Victoria Reppucci</td></tr>
  <tr><td><b>Asesor</b></td><td>Dr. Juan Pablo Costabel</td></tr>
  <tr><td><b>Institución colaboradora</b></td><td>Instituto Cardiovascular de Buenos Aires (ICBA)</td></tr>
</table>


El presente trabajo propone el desarrollo de una herramienta basada en redes neuronales convolucionales para la clasificación automática de cuatro etiologías de hipertrofia ventricular izquierda (HVI) a partir de imágenes de electrocardiogramas de 12 derivaciones: amiloidosis cardíaca (Am), hipertensión arterial (HTA), estenosis aórtica (EAo) y miocardiopatía hipertrófica (MCH). El proyecto busca potenciar el ECG incorporando algoritmos de *deep learning* capaces de identificar patrones sutiles que resultan difíciles de distinguir a simple vista, para así facilitar su interpretación y brindar apoyo al médico en la toma de decisiones clínicas.

Este repositorio contiene:
- **El informe de la tesis** en formato PDF;
- **Los códigos de implementación** de todos los experimentos;
- **El ejecutable de VentrIA**, la aplicación de escritorio que integra los modelos desarrollados.



---

## 1. Informe de la tesis

El informe completo se encuentra en [`Informe Final PFC - version 5.pdf`](Informe%20Final%20PFC%20-%20version%205.pdf).

---

## 2. Códigos de implementación

Todos los códigos utilizados para el desarrollo y la evaluación de los modelos desarrollados se encuentran en la carpeta `Códigos/`, organizados como notebooks de Jupyter numerados según el orden de ejecución:

| Etapa | Notebooks | Descripción |
|---|---|---|
| **Dataset ICBA** | `1` – `2` | Análisis y limpieza de datos, recorte automático de márgenes, redimensionamiento y partición en *folds*. |
| **Dataset sintético** | `3` – `4` | Construcción del dataset de señales públicas (PTB-XL, CPSC-2018, Chapman-Shaoxing, Georgia) y generación de las imágenes sintéticas. |
| **Preentrenamiento** | `5` – `14` | Búsqueda de hiperparámetros con Optuna, entrenamiento y evaluación sobre el dataset sintético de las cinco arquitecturas evaluadas: CNN-A, CNN-B, ResNet50, EfficientNetB0 y ConvNeXtTiny. |
| **Ajuste fino** | `15` – `18` | Búsqueda de hiperparámetros,  entrenamiento y evaluación de los modelos base seleccionados sobre el dataset ICBA. |
| **Ensamble** | `19` | Análisis de complementariedad y evaluación del ensamble de modelos finales. |
| **Experimento de ablación** | `20` – `23` | Entrenamiento y evaluación de los modelos sin preentrenamiento sintético. |
| **Interpretabilidad** | `24` | Generación de los mapas de activación Grad-CAM. |
| **Formatos excluidos** | `25` | Evaluación adicional de los modelos sobre formatos de presentación no utilizados en el entrenamiento. |


**Datos.** El dataset del ICBA no se incluye en este repositorio, ya que contiene información clínica confidencial. Las señales públicas utilizadas para construir el dataset sintético se obtienen desde [PhysioNet](https://physionet.org/content/challenge-2021/) y [PTB-XL](https://physionet.org/content/ptb-xl/).

**Entorno.** Los experimentos se ejecutaron con Python 3.11, TensorFlow 2.21, Keras 3.14, Optuna 4.8 y scikit-learn 1.8, sobre una GPU NVIDIA Quadro RTX 8000.

---

## 3. Aplicación VentrIA

VentrIA es una aplicación de escritorio para Windows que integra el ensamble de los modelos finales. Permite cargar un ECG en formato PNG, JPG o PDF y obtener la etiología predicha, junto con la probabilidad asignada a cada clase y un indicador de confianza. Además, muestra el mapa de activación Grad-CAM, que señala las regiones de la imagen que más contribuyeron a la predicción. Finalmente, si se desea, el análisis puede exportarse como informe en PDF.

La aplicación se ejecuta localmente y no requiere conexión a internet.

### Instalación

1. Descargar el instalador [`VentrIA-Setup.exe`](https://github.com/catajonqui/PFC_Jonquieres_Orsi/releases/download/v1.0/VentrIA-Setup.exe).
2. Ejecutarlo y seguir los pasos del asistente de instalación.
3. Abrir VentrIA desde el menú Inicio o desde el acceso directo del escritorio.

### Uso previsto

VentrIA está destinada a profesionales médicos como **herramienta de apoyo** al diagnóstico diferencial de pacientes con HVI previamente identificada. Sus resultados son orientativos y no reemplazan el juicio clínico ni los estudios confirmatorios.

> **Importante:** los modelos fueron entrenados y evaluados con estudios del ICBA en formato **6×2 horizontal**. Sobre otros formatos de presentación, su desempeño es considerablemente menor.

> **Aviso:** VentrIA es un prototipo de investigación desarrollado en el marco de un Proyecto Final de Carrera. **No es un dispositivo médico aprobado** y no debe utilizarse como herramienta diagnóstica autónoma.

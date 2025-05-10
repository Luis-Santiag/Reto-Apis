# Programa de Detección de Personas con Clarifai

## Integrantes
- Aranza Brigitte Rueda Alvarado, 0905-24-7854 realizacion de  
- Luis Ángel Santiago Palma, 0905-24-9756  
- Edwins Josue Argueta Duarte, 0905-24-6913  

## Objetivo
Crear un programa en Python que muestre una vista previa en vivo de la cámara web, capture una imagen, muestre la imagen en un formulario gráfico, y use la API de Clarifai para detectar personas y, opcionalmente, otros objetos.

## Explicación del Programa
Este programa fue diseñado para cumplir con los requisitos de "El Reto.pdf". Utiliza una cámara web para capturar imágenes y las analiza con una API de visión por computadora para detectar personas y otros objetos. La interfaz gráfica, creada con Tkinter, permite al usuario ver una vista previa en vivo de la cámara, capturar una imagen, visualizarla, y procesarla para obtener resultados de detección.

Inicialmente, se probaron varias APIs de visión por computadora para determinar cuál sería la más adecuada:

- **Google Cloud Vision**: Ofrece capacidades avanzadas de análisis de imágenes, pero requiere una configuración más compleja y una cuenta de Google Cloud, lo que podría generar costos adicionales.
- **Azure Computer Vision**: Similar a Google Cloud Vision, pero también requiere una suscripción a Azure y una configuración más elaborada.
- **Hugging Face**: Ideal para modelos personalizados y experimentales, pero no proporciona una API directa y fácil de usar para este caso.
- **Roboflow**: Más enfocado en modelos personalizados y entrenamiento, lo que lo hace menos práctico para un uso rápido.
- **Clarifai**: Proporciona un modelo gratuito (General Model) que detecta personas y objetos, es fácil de usar, y tiene una API sencilla. Fue elegida por su simplicidad y accesibilidad.

Finalmente, **nos quedamos con Clarifai** porque cumplió con nuestras necesidades: facilidad de uso, un modelo preentrenado gratuito, y buena documentación para integrarlo en Python.

## Investigación
Revisamos varias APIs:
- **Clarifai**: Fácil de usar, con un modelo gratuito que detecta personas y objetos. Elegida por su simplicidad.
- **Roboflow**: Mejor para modelos personalizados, no para uso rápido.
- **Google Cloud Vision**: Buena, pero necesita más configuración.
- **Azure Computer Vision**: Similar a Google, pero más complicada.
- **Hugging Face**: Ideal para modelos avanzados, pero no para API directa.

**Elección**: Clarifai, por su modelo gratuito y facilidad de uso.

## Creación del Enlace de la API
El enlace de la API se creó así:
1. Registrarse en [Clarifai](https://www.clarifai.com/) y crear una aplicación ("DeteccionPersonas").
2. Obtener el **APP_ID** desde el panel de la aplicación.
3. Generar una **API Key** en "My Account" > "Security".
4. Usar el modelo General (ID: `aaa03c23b3724a16a56b629203edc62c`).
5. El enlace de la API es:
   ```
   https://api.clarifai.com/v2/models/aaa03c23b3724a16a56b629203edc62c/outputs
   ```
   - Se envía la imagen en base64 con la API Key y APP_ID.

## Descripción del Programa
El programa (`deteccion_personas_clarifai.py`) hace lo siguiente:
1. Muestra un formulario gráfico (usando Tkinter) con:
   - Vista previa en vivo de la cámara web (solicitado para ver lo que la cámara está capturando en tiempo real).
   - Botón para capturar una imagen.
   - Área para mostrar la imagen capturada (solicitado para visualizar la imagen tomada antes de procesarla).
   - Botón para procesar la imagen con Clarifai.
   - Área de texto para mostrar resultados.
2. Captura la imagen y la guarda como `imagen_capturada.jpg`.
3. Envía la imagen al enlace de Clarifai para:
   - Detectar personas (conceptos como "person", "adult", "man", "woman", "portrait" con confianza > 0.9, ajustado tras errores de detección).
   - Listar otros objetos (opcional).
4. Muestra los resultados en el formulario, incluyendo la confianza de todos los conceptos detectados.
5. Borra la imagen tras procesarla.

## Requisitos
- Python 3.8+
- Librerías: `clarifai-grpc`, `opencv-python`, `pillow`, `tk` (Tkinter, suele venir con Python)
- Credenciales: `API Key` y `APP_ID` de Clarifai
- Cámara web funcional

## Configuración
1. **Instalar Python**:
   - Descarga Python 3.8+ desde [python.org](https://www.python.org/downloads/) si no lo tienes.
   - Verifica en la terminal:
     ```bash
     python --version
     ```

2. **Actualizar pip**:
   - Asegúrate de tener la última versión de pip:
     ```bash
     python -m pip install --upgrade pip
     ```

3. **Instalar librerías**:
   - Ejecuta:
     ```bash
     pip install clarifai-grpc opencv-python pillow
     ```
   - Si Tkinter no está instalado (raro, pero posible en Windows), instala:
     ```bash
     pip install tk
     ```
   - Si hay errores de timeout, prueba:
     ```bash
     pip install clarifai-grpc opencv-python pillow --timeout 100
     ```
   - Si el servidor falla, usa un espejo:
     ```bash
     pip install clarifai-grpc opencv-python pillow -i https://pypi.tuna.tsinghua.edu.cn/simple
     ```

4. **Configurar Clarifai**:
   - Crea una aplicación en [Clarifai](https://www.clarifai.com/).
   - Copia tu `API Key` (desde "My Account" > "Security") y `APP_ID` (desde tu aplicación).
   - En el código, asegúrate de que las credenciales sean correctas.

5. **Ejecutar**:
   ```bash
     python deteccion_personas_clarifai.py
     ```

## Ejemplo de Salida
- Ventana con una vista previa en vivo de la cámara.
- Botón "Capturar Imagen" guarda y muestra la imagen.
- Botón "Procesar Imagen" muestra en el área de texto:
```
Confianza para 'person': 0.00
- portrait (confianza: 0.99)
- adult (confianza: 0.98)
- man (confianza: 0.97)
- indoors (confianza: 0.96)
- window (confianza: 0.96)
Resultados:
Persona detectada: Sí
```

## Prompts Utilizados
A lo largo del desarrollo, se realizaron las siguientes solicitudes:
- "Sé un poco más claro por favor": Pedí una explicación más detallada y clara sobre cómo crear el enlace de la API de Clarifai y usar el programa.
- "Cómo instalo lo que necesito de más": Solicité instrucciones específicas para instalar las librerías necesarias (`clarifai-grpc`, `opencv-python`, `pillow`).
- "Tengo este error [timeout al instalar librerías]": Reporté un error de timeout al instalar las librerías con pip, que se solucionó aumentando el tiempo de espera y usando un espejo alternativo.
- "Ahora solo quiero que el código me despliegue un formulario donde pueda ver la imagen que tomó": Pedí que el programa mostrara un formulario gráfico para visualizar la imagen capturada antes de procesarla.
- "Me da error esto [en Tkinter con `stattk.DISABLED`]": Reporté un error en la sintaxis de Tkinter (`stattk.DISABLED`), que se corrigió a `state=tk.DISABLED`.
- "Ahora solo agrega que salga lo que la cámara está viendo": Solicité una vista previa en vivo de la cámara web en la interfaz gráfica.
- "Me sigue apareciendo que no hay personas pero que sí hay otros objetos": Reporté que el programa no detectaba personas a pesar de que Clarifai devolvía conceptos relacionados (como "portrait"). Esto se corrigió ajustando la lógica para considerar conceptos relacionados con personas.

## Demostración
Las capturas de pantalla estarán en la carpeta `capturas/` (agregar después de ejecutar).

## Desafíos
- **Timeout al instalar librerías**: Se enfrentó un error de timeout al instalar las librerías con pip (`pip install clarifai-grpc opencv-python pillow`). Se solucionó aumentando el tiempo de espera (`--timeout 100`) y usando un espejo alternativo (`-i https://pypi.tuna.tsinghua.edu.cn/simple`).
- **Error en Tkinter (`stattk.DISABLED`)**: Un error tipográfico en la sintaxis de Tkinter (`stattk.DISABLED`) fue corregido a `state=tk.DISABLED` para deshabilitar correctamente el botón "Procesar Imagen".
- **Falta de vista previa de la cámara**: Inicialmente, el programa no mostraba lo que la cámara veía en tiempo real. Se añadió una vista previa en vivo usando OpenCV y Tkinter, actualizando los frames cada 10 ms.
- **Ajustar la lógica de detección para incluir conceptos relacionados como "adult", "man", "woman", y "portrait"**: El programa no detectaba personas porque Clarifai no devolvía el concepto exacto "person". Se ajustó la lógica para considerar conceptos relacionados con confianza > 0.9.
- **Verificar la confianza de los conceptos devueltos por Clarifai para depurar falsos negativos**: Se añadió depuración para mostrar todos los conceptos y sus confidencias, lo que ayudó a identificar y corregir el problema de detección.
- **Asegurar que la cámara web funcione y la interfaz sea fluida**: Se implementó la liberación de la cámara al cerrar la ventana y se optimizó la actualización de la vista previa.

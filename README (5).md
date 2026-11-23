# 🐭 Caza Ratones AR

Juego de realidad aumentada en el navegador: la cámara muestra tu entorno, los ratones corren por encima y los atrapás con la mano en tiempo real. Al atraparlos suman puntos, hay una animación de estallido y suena un *blop*.

Proyecto de **Studio NMFT**. Vanilla HTML/CSS/JS, un solo archivo, sin frameworks ni build.

## Cómo se juega

1. Abrí la página y tocá **Activar cámara y jugar**. Aceptá el permiso de cámara.
2. Los ratones aparecen y corren por la pantalla.
3. **Cerrá el puño** sobre un ratón para atraparlo (modo normal), o simplemente **tocalo** con la mano (modo fácil, botón de abajo).
4. Rondas de **45 segundos**. Atrapar varios seguidos en menos de 1,6 s activa el **combo** (hasta x8): +10 por ratón multiplicado por el combo.
5. Los ratones esquivan la mano abierta y se aceleran con el paso del tiempo.
6. El récord se guarda en el navegador (`localStorage`).

## Tecnología

- **[MediaPipe Tasks Vision](https://ai.google.dev/edge/mediapipe/solutions/vision/hand_landmarker)** (Hand Landmarker, modo VIDEO, hasta 2 manos). Se carga por CDN (jsDelivr) y el modelo desde `storage.googleapis.com`. Usa GPU y cae a CPU si no está disponible.
- **Canvas 2D** para el video espejado, los ratones (dibujados por código), el esqueleto de la mano y los efectos.
- **Web Audio API** para el blop: sin archivos de audio.

## Ejecutar

La cámara requiere **HTTPS** o **localhost**.

```bash
# local
python -m http.server 8000
# luego abrir http://localhost:8000
```

También funciona subiéndolo tal cual a **GitHub Pages**.

Se necesita conexión a internet la primera vez (CDN de MediaPipe y modelo).

## Ajustes rápidos

Todo está en `index.html`:

| Qué | Dónde |
|---|---|
| Duración de la ronda | `GAME_TIME` |
| Sensibilidad del puño | `closed: ratio < 1.45` en `processHands` (más alto = cierra con menos esfuerzo) |
| Tamaño de los ratones | `mouseSize()` |
| Cantidad máxima / frecuencia de aparición | `mice.filter(...) < 7` y `spawnT` en `frame()` |
| Velocidad de los ratones | `speed` en `spawnMouse()` |
| Radio de captura | `checkCatches()` |
| Sonido | `blop()` |

## Problemas comunes

- **No pide cámara / error al iniciar:** revisá que estés en HTTPS o localhost y que el navegador tenga permiso de cámara para el sitio.
- **El puño no se detecta:** mejorá la luz, alejá un poco la mano de la cámara o subí el umbral `1.45`.
- **Va lento:** cerrá otras pestañas; en celulares viejos bajá la resolución en `initCamera()`.
- **Se congela:** abrí la consola (F12) y fijate el error.

## Ideas a futuro

- Ratones especiales (dorado con más puntos, gato que resta).
- Modo cooperativo/versus por PeerJS.
- Empaquetado como APK con WebView (agregar permiso `CAMERA` en el manifest).
- Niveles, dificultad progresiva y ranking.

---

Hecho por NOMORE · Studio NMFT · [nmft.ar](https://nmft.ar)

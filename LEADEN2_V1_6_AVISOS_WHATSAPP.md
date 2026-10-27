# LEADEN2 V1.6 — Avisos de mensajes de WhatsApp

Base de respaldo: `LEADEN2_V1_5_RESPALDO_TECNICO_LLAMADA_EXITOSA.zip`.

## Objetivo
Añadir una función independiente para que, cuando LEADEN2 esté activo mediante el switch, un mensaje visible de WhatsApp pueda generar un aviso por medio del archivo:

`app/src/main/assets/mensaje_whatsapp.mp3`

Después del aviso, LEADEN2 pregunta por voz si el usuario desea escuchar el mensaje. Solo ante una confirmación afirmativa se reproduce por el parlante el texto recibido mediante TextToSpeech.

## Protecciones
- La función solo actúa si `assistant_enabled=true`.
- Se mantienen separados los servicios de accesibilidad y de notificaciones.
- `WhatsAppCallAccessibilityService` no fue reemplazado ni eliminado.
- El flujo existente de llamadas de WhatsApp permanece en su servicio original.
- Los audios existentes (`comienza_el_ensamblaje.mp3`, `importando.mp3`, `orden.mp3`) se conservan.
- Si LEADEN2 ya está reproduciendo un audio propio, el aviso de WhatsApp queda en cola para no interrumpir ese audio.
- Se aplica deduplicación de notificaciones durante una ventana corta para evitar repeticiones de WhatsApp.

## Permiso nuevo
Android requiere que el usuario habilite manualmente el acceso a notificaciones para LEADEN2. La pantalla principal incorpora el botón `CONFIGURAR AVISOS DE WHATSAPP`, que abre la pantalla correspondiente de Android.

## Archivos nuevos
- `app/src/main/java/com/leaden2/app/WhatsAppNotificationListenerService.java`
- `app/src/main/assets/mensaje_whatsapp.mp3`

## Archivos modificados
- `AndroidManifest.xml`: registro del NotificationListenerService.
- `MainActivity.java`: botón de configuración y texto informativo.
- `PlaybackService.java`: reproducción del aviso y continuación hacia la confirmación.
- `VoiceAssistantService.java`: confirmación por voz y lectura del mensaje.
- `app/build.gradle`: versión 1.6 / versionCode 7.

## Mensaje MP3 incluido
El MP3 incluido dice:

`LEADEN2 informa: tienes un nuevo mensaje de WhatsApp.`

## Punto de restauración
El ZIP V1.5 original debe conservarse sin cambios y utilizarse como respaldo si fuera necesario volver a la versión anterior.


## V1.6.1 — ajuste de aviso y confirmación (2026-10-26)

- Se sustituyó `app/src/main/assets/mensaje_whatsapp.mp3` por el audio proporcionado para esta versión.
- El archivo conserva exactamente el nombre `mensaje_whatsapp.mp3`.
- La confirmación larga por TTS fue reducida a: `Mensaje de [remitente]. ¿Deseas escucharlo?`
- La confirmación de WhatsApp usa una velocidad de TTS ligeramente superior (1.12x) para reducir el tiempo antes de escuchar `sí`.
- El resto de respuestas TTS mantiene velocidad normal.
- `PlaybackService` refresca específicamente `mensaje_whatsapp.mp3` al actualizar la aplicación, evitando que Android conserve el audio anterior en `filesDir`.
- `WhatsAppCallAccessibilityService` no fue modificado.
- El botón `CONFIGURAR MENSAJES DE WHATSAPP` se conserva.
- `versionCode`: 8; `versionName`: 1.6.1.

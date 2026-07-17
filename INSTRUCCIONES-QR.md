# Carta pública con QR — Cómo publicarla

La carta pública es una página web de **solo lectura** que muestra tus platos y alérgenos
a los clientes. Lee del mismo Firebase que tu app, así que se actualiza sola: cambias un
plato en la app y la carta pública lo refleja al instante.

Vamos a publicarla en internet con Firebase Hosting (gratis) y a generar el QR para las mesas.

## PASO 1 — Ajustar las reglas de Firestore (importante)

La carta pública necesita que cualquiera pueda LEER (los clientes), pero seguir protegiendo
la escritura. Entra en console.firebase.google.com → tu proyecto → Firestore Database →
pestaña **Reglas**, y pon exactamente esto (luego pulsa **Publicar**):

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /restaurantes/{codigo}/{documento=**} {
      allow read: if true;
      allow write: if true;
    }
  }
}
```

(De momento mantenemos la escritura abierta como estaba; la carta solo lee. Si más adelante
quieres blindar la escritura, se puede añadir autenticación — pídemelo.)

## PASO 2 — Instalar la herramienta de Firebase (una sola vez)

En tu PC, abre una terminal y ejecuta:

```
npm install -g firebase-tools
```

## PASO 3 — Preparar la carpeta

1. Descomprime este proyecto en una carpeta, por ejemplo `carta-publica` en el Escritorio.
2. Dentro, crea una subcarpeta llamada **public** y mueve `carta.html` dentro de ella.
   La estructura debe quedar así:
   ```
   carta-publica/
   ├── firebase.json
   └── public/
       └── carta.html
   ```

## PASO 4 — Publicar

En la terminal, dentro de la carpeta `carta-publica`, ejecuta:

```
firebase login
```
(se abre el navegador, inicia sesión con tu cuenta de Google, la misma de Firebase)

```
firebase use listadiplatos
firebase deploy --only hosting
```

Al terminar te dará una **URL pública**, algo como:
`https://listadiplatos.web.app`

Esa es la dirección de tu carta. Ábrela en el móvil para comprobar que se ve bien.

## PASO 5 — Generar el QR

1. Entra en una web gratuita de códigos QR, por ejemplo: https://www.qr-code-generator.com
   o https://qrcode.tec-it.com
2. Pega tu URL (`https://listadiplatos.web.app`).
3. Descarga el QR en PNG o PDF (en alta resolución si vas a imprimirlo grande).
4. Imprímelo y colócalo en las mesas, en la barra o en la entrada.

Los clientes lo escanean con la cámara del móvil y ven tu carta actualizada al momento.

## Para actualizar la carta pública

**No hace falta volver a publicar** cuando cambias platos: la carta lee de Firebase en
tiempo real, así que cualquier cambio en tu app aparece solo.

Solo tendrías que repetir el `firebase deploy` si en el futuro cambio el diseño de la
página `carta.html` (te avisaría).

## Notas

- La carta pública NO tiene modo edición ni acceso a nada: los clientes solo ven los platos.
- Se ve perfecta en el móvil del cliente (está diseñada para eso).
- Muestra todas las secciones con platos. Si una sección está vacía, no aparece.
- Incluye buscador y filtro por alérgeno, para que el cliente encuentre lo que puede comer.

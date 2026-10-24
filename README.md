# Mokepon – Juego multijugador en el navegador

> **Demo en vivo:** https://mi-primer-proyecto-mokepon.onrender.com
> La demo corre en el plan gratuito de Render: si lleva un rato sin visitas, la primera carga puede tardar cerca de un minuto en despertar el servidor. Para probar el multijugador, ábrela en dos pestañas o dispositivos.

Juego donde varios jugadores eligen un Mokepon, se mueven por un mapa compartido y, al chocar con otro jugador, se enfrentan en un combate por turnos con ataques de agua 💧, fuego 🔥 y tierra 🌱.

## Tecnologías

- **Frontend:** JavaScript puro, HTML5, CSS3 y Canvas 2D (sin frameworks)
- **Backend:** Node.js y Express (API REST), CORS
- **Despliegue:** Render (el mismo servidor Express sirve el frontend y la API)

## Cómo funciona

- **Mapa y movimiento:** el mapa se dibuja en un `<canvas>` que se repinta cada 50 ms. Los Mokepon se mueven con el teclado o con botones en pantalla.
- **Detección de colisiones:** al chocar dos jugadores se inicia el combate.
- **Multijugador con polling:** cada ciclo de dibujo el cliente envía su posición con `fetch` y recibe la de los demás jugadores. Los ataques del rival se consultan con otro polling a su endpoint. No usa WebSockets.
- **Estado en memoria:** el servidor guarda a los jugadores en un arreglo. Si el servidor se reinicia, se pierden.

## API

| Método | Ruta | Descripción |
| --- | --- | --- |
| GET | `/unirse` | Registra un jugador y devuelve su id |
| POST | `/mokepon/:jugadorId` | Asigna el Mokepon elegido |
| POST | `/mokepon/:jugadorId/posicion` | Guarda la posición y devuelve la de los demás jugadores |
| POST | `/mokepon/:jugadorId/ataques` | Guarda la secuencia de ataques del jugador |
| GET | `/mokepon/:jugadorId/ataques` | Devuelve los ataques de ese jugador |

## Ejecutarlo en local

```bash
git clone https://github.com/cristianareiza/mi-primer-proyecto-mokepon.git
cd mi-primer-proyecto-mokepon
npm install
npm start
```

Abre http://localhost:8080 en dos pestañas para probar el multijugador.

## Posibles mejoras

- Reemplazar el polling por WebSockets (por ejemplo, Socket.IO) para reducir peticiones y latencia.
- Guardar jugadores y partidas en una base de datos.
- Eliminar a los jugadores desconectados del servidor.

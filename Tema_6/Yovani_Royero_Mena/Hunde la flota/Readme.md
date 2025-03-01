# API del juego hunde la flota

En este ejercicio vamos a diseñar la API REST que podría usar la App del juego 'Hundir la flota' o 'Juego de los barcos'.
Si no conoces este juego puedes echar un vistazo al PDF de instrucciones que se encuentra en esta misma ruta, o descarga una App existente para jugar una partida. La aplicación gestionará principalmente partidas entre usuarios registrados o invitados (es decir, sin registro, anónimos). Cada partida tiene asociadas dos cuadrículas de 10x10 cuadrados, una por cada jugador, y estos jugadores deben poner sobre dicha cuadrícula las localizaciones de sus barcos. Tal como se indica en las instrucciones, deberá haber:
- 1 barco de 4 cuadrados.
- 2 barcos de 3 cuadrados.
- 3 barcos de 2 cuadrados.
- 4 barcos de 1 cuadrado.

También es necesario que, como dicen las instrucciones, se respeten dos reglas:
- Los barcos se colocan enteramente en horizontal o enteramente en vertical, es decir, no puede haber un barco en forma de L.
- Siempre debe haber un cuadrado de distancia entre cualquier punto de cualquier barco, y se pueden pegar al borde de la cuadrícula.

El objetivo del ejercicio es diseñar una API REST que será implementada (en otros ejercicios) por un microservicio o aplicación que se encargará de tratar todos los datos de las diferentes partidas. En este ejercicio nos centraremos únicamente en el diseño de la API y no trataremos ningún detalle de la implementación.

Las operaciones que la API debe soportar son las siguientes:
- Crear una partida.
- Eliminar una partida.
- Modificar datos de una partida.
- Iniciar una partida.
- Finalizar una partida.
- Consultar todos los datos (jugadores asociados, barcos de cada jugador, disparos realizados, ganador...) de una partida.
- Añadir un barco a la cuadrícula de un jugador en una partida.
- Eliminar un barco de la cuadrícula de un jugador en una partida.
- Consultar todos los barcos de un jugador de una partida.
- Registrar un disparo de un jugador a otro en una partida.
- Crear un usuario.
- Obtener datos de un usuario.
- Eliminar un usuario.

Ten en cuenta que podría no ser necesario definir un endpoint por cada una de las operaciones que se han listado. No obstante, dichas operaciones deben ser satisfechas por la API diseñada. Las primeras preguntas que deberás hacerte son:

- ¿Qué recursos tiene que manejar la API?
      - Coordenada (`{"id":number, "posicionX": string, "posicionY": number}`)<br/>
      - Barco (`{"id":number, "posicion": Coordenada[], "hundido": boolean}`)<br/>
      - Tablero (`{"id":number, "barcos": Barco[], "disparos": Disparos[]}`)<br/>
      - Partida (`{"id":number, "jugador1": Jugador, "jugador2": Jugador, "ganador": string, "estado": string}`)  estado => ['Creada','Iniciada','Finalizada']<br/>
      - Jugador (`{"id":number,"tableroBarcos": Tablero, "tableroDisparos": Tablero, "usuario": string}`)<br/>
      - Usuario (`{"id":number,"nombre": string, "apellidos": string, "usuario": string, "constrasenna": string}`)<br/>
      - Disparo (`{"id":number, objetivo: Coordenada, "resultado": string}`) resultado => ['Agua','Tocado','Hundido']
- ¿Cómo se relacionan entre sí?
- ¿Qué información (atributos) guarda cada recurso?

| Método HTTP | URI            | Query Params | Request Body | Response Body    | Códigos HTTP de respuesta |
|-------------|----------------|--------------|--------------|------------------|-------------------------|
| POST         | /api/v1/partidas | - | `{"jugador1Id": number, "jugador2Id": number}`| `{"id":number,"jugador1": Jugador, "jugador2": Jugador,"ganador": string,"estado": string, "fechaHora": date}` | 201<br/> 400<br/>404<br/> 500|
| DELETE         | /api/v1/partidas/{id} | -  | - | `{"resultado":boolean}` | 200<br/> 404<br/> 500|
| PUT         | /api/v1/partidas/{id} | - | `{"id":number,"jugador1Id": number, "jugador2Id": number,"ganador": string, "estado": string, "fechaHora": date}`| `{"id":number,"jugador1": Jugador, "jugador2": Jugador, "ganador": string, "estado": string, "fechaHora": date}` | 200<br/> 400<br/>404<br/> 500|
| PATCH         | /api/v1/partidas/{id} | - | `{"id":number,"estado": string}`| `{"id":number,"jugador1Id": number, "jugador2Id": number, "ganador": string, "estado": string, "fechaHora": date}` | 200<br/> 400<br/>404<br/> 500|
| GET         | /api/v1/partidas/{id} | - | - | `{"id":number,"jugador1": Jugador, "jugador2": Jugador, "ganador": string, "estado": string, "fechaHora": date}` | 200<br/> 404 |
| POST         | /api/v1/partidas/{id}/jugador/{id}/barcos | - | `{ "posicion": Coordenada[]}`| `{"id":number, "posicion": Coordenada[], "hundido": boolean, "tablero": Tablero}` | 201<br/> 400<br/>404<br/> 500|
| DELETE         | /api/v1/partidas/{id}/jugador/{id}/barcos/{id} | -  | - | `{"resultado":boolean}` | 200<br/> 404<br/> 500|
| GET         | /api/v1/partidas/{id}/jugador/{id}/barcos | -  | - | `{"barcos":Barco[]}` | 200<br/> 404<br/> 500|
|POST         | /api/v1/partidas/{id}/jugador/{id}/disparos | - | `{ "posicion": Coordenada}`| `{"id":number, "objetivo": Coordenada, "resultado": boolean, "tablero": Tablero}` | 201<br/> 400<br/>404<br/> 500|
| POST         | /api/v1/usuarios | - | `{"nombre": string, "apellidos": string, "usuario": string, "constrasenna": string}`| `{"id":number,"nombre": string, "apellidos": string, "usuario": string}` | 201<br/> 400<br/> 500|
| GET         | /api/v1/usuarios/{id} | -  | - | `{"id":number,"nombre": string, "apellidos": string, "usuario": string}` | 200<br/> 404<br/> 500|
| DELETE         | /api/v1/usuarios/{id} | -  | - | `{"resultado":boolean}` | 200<br/> 404<br/> 500|
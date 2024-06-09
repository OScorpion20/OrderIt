# Order It con Integración de Google Calendar

## Introducción

Este proyecto es una aplicación de lista de tareas integrada con Google Calendar. Permite a los usuarios gestionar sus tareas y sincronizarlas con Google Calendar. Los usuarios pueden autorizar su cuenta de Google, crear eventos en su calendario directamente desde la aplicación y recibir notificaciones de éxito cuando se agrega un evento.

## Funcionalidades

- Autenticación de usuarios
- Gestión de tareas (añadir, editar, eliminar, completar)
- Integración con Google Calendar
- Notificaciones para eventos de calendario exitosos

## Tecnologías Utilizadas

- Frontend: React
- Backend: Node.js, Express
- Base de datos: MongoDB
- API de Google Calendar
- Autenticación: JWT
- CSS para estilos

## Requisitos Previos

Antes de comenzar, asegúrate de tener lo siguiente instalado en tu máquina:

- Node.js
- npm (Node Package Manager)
- MongoDB
- Un proyecto en Google Cloud Platform con la API de Google Calendar habilitada

## Instrucciones de Configuración

### 1. Clonar el Repositorio

```sh
git clone https://github.com/tuusuario/todo-list-app.git
cd todo-list-app
````

### 2. Configuración del Backend
Navega al directorio backend:

```sh
cd backend
````

Instala las dependencias:

````sh
npm install
````

Crea un archivo .env en el directorio backend con el siguiente contenido:

````sh
PORT=5000
MONGODB_URI=tu_mongodb_uri
JWT_SECRET=tu_jwt_secret
````
Inicia el servidor del backend:

````sh
npm start
````

### 3. Configuración del Frontend

Navega al directorio frontend:

````sh
cd ../frontend
````

Instala las dependencias:

````sh
npm install
````

En GoogleAuth.js agrega el clientID y la key del api de google:

````sh
import React, { useEffect, useState } from 'react';
import { gapi } from 'gapi-script';
import './GoogleAuth.css'; // Importa el archivo de estilo

const CLIENT_ID = 'tu_cliente_id_de_google';
const API_KEY = 'tu_api_key_de_google';
const SCOPES = 'https://www.googleapis.com/auth/calendar.events';

const GoogleAuth = () => {
  const [successMessage, setSuccessMessage] = useState('');

  useEffect(() => {
    const initClient = () => {
      gapi.client.init({
        apiKey: API_KEY,
        clientId: CLIENT_ID,
        discoveryDocs: ['https://www.googleapis.com/discovery/v1/apis/calendar/v3/rest'],
        scope: SCOPES,
      }).then(() => {
        console.log('Google API initialized');
      }).catch((error) => {
        console.error('Error initializing Google API:', error);
      });
    };
    gapi.load('client:auth2', initClient);
  }, []);

  const handleAuthClick = () => {
    gapi.auth2.getAuthInstance().signIn().then(() => {
      console.log('User signed in');
    }).catch((error) => {
      console.error('Error signing in', error);
    });
  };

  const handleSignoutClick = () => {
    gapi.auth2.getAuthInstance().signOut().then(() => {
      console.log('User signed out');
    }).catch((error) => {
      console.error('Error signing out', error);
    });
  };

  const createEvent = () => {
    const event = {
      summary: 'New Event',
      start: {
        dateTime: '2024-06-18T10:00:00-07:00',
        timeZone: 'America/Los_Angeles',
      },
      end: {
        dateTime: '2024-06-18T10:25:00-07:00',
        timeZone: 'America/Los_Angeles',
      },
    };

    const request = gapi.client.calendar.events.insert({
      calendarId: 'primary',
      resource: event,
    });

    request.execute((event) => {
      console.log('Event created: ', event.htmlLink);
      setSuccessMessage('Evento agregado exitosamente a Google Calendar');
      setTimeout(() => {
        setSuccessMessage('');
      }, 3000); // Mensaje desaparece después de 3 segundos
    });
  };

  return (
    <div>
      <button onClick={handleAuthClick}>Authorize Google Calendar</button>
      <button onClick={handleSignoutClick}>Sign Out</button>
      <button onClick={createEvent}>Create Event</button>
      {successMessage && <div className="success-message">{successMessage}</div>}
    </div>
  );
};

export default GoogleAuth;

````
#### Uso

1. Abre tu navegador y navega a http://localhost:3000.

2. Registra un nuevo usuario o inicia sesión con una cuenta existente.

3. Autoriza la aplicación para acceder a tu Google Calendar haciendo clic en el botón "Authorize Google Calendar".

4. Añade tareas y sincronízalas con Google Calendar utilizando el botón "Create Event".

5. Visualiza el mensaje de éxito cuando un evento se agrega a tu Google Calendar.

### Conclusión
Con esta configuración, deberías poder ejecutar la aplicación de lista de tareas con integración de Google Calendar en cualquier máquina. Si encuentras algún problema o tienes alguna pregunta, por favor abre un issue en GitHub o contáctanos directamente.

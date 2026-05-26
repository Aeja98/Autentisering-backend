# Tankad Tacos API

URL

Detta repository innehåller kod för en webbtjänst byggd med Node.js, Express och MongoDB. API:et är skapat för en fiktiv taco truck, Tankad Tacos, och innehåller funktionalitet för autentisering med registrering, inloggning och skyddade routes.

Vid inloggning skapas en JWT-token som används för att komma åt skyddad admin-data. Lösenord hashats med bcrypt innan de sparas i databasen.

## Datamodeller

### User

Varje användarkonto innehåller följande fält:

| Fält | Datatyp | Beskrivning |
|---|---|---|
| _id | ObjectId | Unikt id som skapas automatiskt av MongoDB |
| username | String | Användarnamn |
| email | String | Användarens e-postadress |
| password | String | Hashat lösenord |
| account_created | Date | Datum då kontot skapades |

### MenuItem

Varje menyobjekt innehåller följande fält:

| Fält | Datatyp | Beskrivning |
|---|---|---|
| _id | ObjectId | Unikt id som skapas automatiskt av MongoDB |
| name | String | Namn på maträtten eller drycken |
| category | String | Kategori, till exempel Mat eller Dryck |
| description | String | Beskrivning av menyobjektet |
| fillings | Array | Lista med fyllningar eller alternativ |
| price | Number | Pris i kronor |

## Användning

Nedan beskrivs hur API:et kan användas:

| Metod | Ändpunkt | Skyddad | Beskrivning |
|---|---|---|---|
| GET | `/` | Nej | Testar att API:et är igång |
| POST | `/api/auth/register` | Nej | Skapar ett nytt användarkonto |
| POST | `/api/auth/login` | Nej | Loggar in användare och returnerar JWT-token |
| GET | `/api/admin/dashboard` | Ja | Hämtar skyddad admin-data och meny från databasen |
| POST | `/api/admin/menu` | Ja | Lägger till ett nytt menyobjekt i databasen |

Skyddade routes kräver att en giltig JWT-token skickas med i anropet:

```bash
Authorization: Bearer DIN_TOKEN_HÄR
```

## JSON-struktur för registrering

Ett nytt användarkonto skickas som JSON med följande struktur:

```json
{
  "username": "admin",
  "email": "admin@tankadtacos.se",
  "password": "password123"
}
```

## JSON-struktur för inloggning

Vid inloggning skickas användarnamn och lösenord:

```json
{
  "username": "admin",
  "password": "password123"
}
```

Vid lyckad inloggning returneras en JWT-token:

```json
{
  "message": "Inloggning lyckades.",
  "token": "JWT_TOKEN_HÄR",
  "user": {
    "id": "user_id",
    "username": "admin",
    "email": "admin@tankadtacos.se"
  }
}
```

## JSON-struktur för menyobjekt

Ett menyobjekt skickas som JSON med följande struktur:

```json
{
  "name": "Tacos",
  "category": "Mat",
  "description": "Mjuka tacos med valfri fyllning.",
  "fillings": ["Chicken", "Asada", "Al Pastor", "Veggie mince", "Fajita veggies"],
  "price": 95
}
```

## Tekniker

Projektet är byggt med:

- Node.js
- Express
- MongoDB Atlas
- Mongoose
- CORS
- dotenv
- bcryptjs
- jsonwebtoken
- nodemon

# BUZIC Golang Restfull Api
[![MIT License](http://img.shields.io/badge/license-MIT-blue.svg?style=flat)](LICENSE) [![Go Report Card](https://goreportcard.com/badge/github.com/mlabouardy/movies-restapi)](https://goreportcard.com/report/github.com/mlabouardy/movies-restapi)

## What is it ?
The api is Buzic's server side connected to the **PostgreSQL** database.

## How to launch it
In a terminal go to the root folder of the project (on server located in `/home/buzicapi/http/api`)
##### Server:
```
./start.sh
```
##### Linux/Mac:
```bash
# Build executable
go build

# Run executable
./api
```
##### Windows:
```bash
# Build executable
go build

# Run executable
api.exe
```

Launching the executable will use port number *3000*, use the option '--port' with the port number to change it.
```bash
# Run executable on port 5000
./api --port 5000
```

## How to use it ?
Here is a GET request to have all musics `https://api.buzic.pw/oauth/musics`.

### Security
You must add your **token** as Bearer Token to authenticates your self at each request.
This authentication is to grant you access and make it easier to handle your requests.

The `https://api.buzic.pw/user/new` POST request is to create a new user. Use this request as OPTIONS instead of POST to see the body expected.

The `https://api.buzic.pw/user/token` GET request is to get your token access from your **username** and **password**.
To send your identifications add `?grant_type=client_credentials&client_id=[username]&client_secret=[password]&scope=read` to the request and replace `[username]` and `[password]`.

An `access_token` field will be available in the response which is your token you need to use as Bearer Token.

![alt text](image/AccessToken.jpg)

### All requests:
All the other requests starts with `https://api.buzic.pw/oauth` and needs the token access.

**GET** requests are used to get information.

**POST** requests are used to create information.

**PUT** requests are used to update information.

**DELETE** requests are used to delete information.

Every Http status detail can be found at
https://httpstatuses.com.

The Golang struct are in the file `archi.go` located in th **archi** folder.

Body is a Golang struct in Json form
#### Get
| Requests                | Description                                                                  | Http status   | Golang struct answer |
| ----------------------- | ---------------------------------------------------------------------------- | ------------- | -------------------- |
|`/musics`                | Get all musics                                                               | 200, 401, 500 | `[]Musics`           |
|`/musics/recommendation` | Get all recommended musics                                                   | 200, 401, 500 | `[]Musics`           |
|`/search/:name`          | Get musics by search of **name**                                             | 200, 401, 500 | `[]Musics`           |
|`/playlists`             | Get all public playlist                                                      | 200, 401, 500 | `[]Playlist`         |
|`/playlist`              | Get musics in your current playlist (use PUT `/playlist/enter/:code` before) | 200, 401, 500 | `[]Musics`           |
#### POST
| Requests    | Description                                                             | Http status             | Body       |
| ----------- | ----------------------------------------------------------------------- | ----------------------- | ---------- |
| `/playlist` | Create playlist (use this request with OPTIONS to see the body required | 201, 400, 401, 406, 500 | `Playlist` |
#### PUT
| Requests           | Description                                                    | Http status                  | Golang struct answer |
| ------------------ | -------------------------------------------------------------- | ---------------------------- | -------------------- |
| `/playlist/enter/:code`     | Enter into **code** playlist                                          | 200, 401, 500      | `[]Music` |
| `/playlist/exit`            | Exit a playlist                                                | 200, 401, 500      | null      |
| `/playlist/add/:music`      | Add **music** to your current playlist                         | 200, 304, 401, 500 | `[]Music` |
| `/playlist/upvote/:music`   | Add or remove upvote of **music** from your current playlist   | 200, 401           | `[]Music` |
| `/playlist/downvote/:music` | Add or remove downvote of **music** from your current playlist | 200, 401           | `[]Music` |
#### DELETE
| Requests           | Description                                               | Http status   | Golang struct answer |
| ------------------ | --------------------------------------------------------- | ------------- | -------------------- |
| `/user/delete`     | Delete your account                                       | 501           | Not Implemented      |
| `/playlist`        | Delete playlist if you created                            | 200, 401, 500 | null                 |
| `/playlist/:music` | Delete **music** from your current playlist if you add it | 200, 401, 500 | `[]Music`            |

#### IMPORTANT
Parameters in path like **music** should not contains these characters: **: / ? # [ ] @ ! $ & ' ( ) * + , ; =**
## ToDo
#### UnitTest
Create unit tests to be aware of changes after changing the code.

#### Websocket
Make it so if an update is happening on one screen it updates on the other ones without refresh.

## Author
**Stanley Stephens**: stanley.stephens@epitech.eu

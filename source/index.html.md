---
title: API Reference

language_tabs:
  - json

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

---

# Introduction

API pour le projet technologique.


Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/tripit/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Requêtes 

Une action correspondent à un type.

## register

`register` permet de s'enregistrer auprés du bus.

> Requête

```json
{
  "type" : "register",
  "sender_class" : "GPS",
  "sender_name" : "GPS 2"
}
```

> Réponse

```json
{
  "type" : "register",
  "ack" : { "resp" : "ok" }
}
```

**Paramètres**

*Requête*

Paramètres | Objet | Description
--------- | ---  |-----------
sender_class | `CLASS` | Nom de la classe 
sender_name | `STRING`  | Nom du capteur

*Réponse*

Paramètres | Objet | Description
--------- | ---  |-----------
ack | `ACK` | Accusé de réception

##deregister

`deregister` permet de se déconnecter du bus.

> Requête

```json
{
  "type" : "deregister",
  "sender_id" : 1
}
```

> Réponse

```json
{
  "type" : "deregister",
  "ack" : { "resp" : "ok" }
}
```
**Paramètres**

*Requête*

Paramètres | Objet | Description
--------- | ---  |-----------
sender_id | `ID` | ID unique du capteur 

*Réponse*

Paramètres | Objet | Description
--------- | ---  |-----------
ack | `ACK` | Accusé de réception

##list

`list` permet de lister les capteurs présents sur le bus.

> Requête

```json
{
  "type" : "list",
  "sender_id" : "GPS"
}
```
ou
```json
{
  "type" : "list",
  "sender_name" : "GPS 2"
}
```
ou
```json
{
  "type" : "list"
}
```


> Réponse

```json
{
  "type" : "send",
  "ack" : { "resp" : "ok" },
  "results" : [
        {
            "sender_id" : 1,
            "sender_class" : "GPS",
            "sender_name" : "GPS 2",
            "last_message_id" : 1025        
        },
        {
            "sender_id" : 2,
            "sender_class" : "GPS",
            "sender_name" : "GPS 3",
            "last_message_id" : 124        
        }
  ]
}
```
**Paramètres**

*Requête*

Paramètres | Objet | Description
--------- | ---  |-----------
sender_id | `ID` | ID unique du capteur 
contents  | `CLASS_CONTENTS` | Payload spécifique à chaque classe [voir ici](#class-class_contents)

*Réponse*

Paramètres | Objet | Description
--------- | ---  |-----------
ack | `ACK` | Accusé de réception


##send

`send` permet d'envoyer un message au bus.

> Requête

```json
{
  "type" : "send",
  "sender_id" : 1,
  "contents" : { "lat" : -49.202458, "lng": 20.687594 }
}
```

> Réponse

```json
{
  "type" : "send",
  "ack" : { "resp" : "ok" }
}
```
**Paramètres**

*Requête*

Paramètres | Objet | Description
--------- | ---  |-----------
sender_id | `ID` | ID unique du capteur 
contents  | `CLASS_CONTENTS` | Payload spécifique à chaque classe [voir ici](#class-class_contents)

*Réponse*

Paramètres | Objet | Description
--------- | ---  |-----------
ack | `ACK` | Accusé de réception


##get

`get` permet de récuperer un message du bus.

> Requête

```json
{
  "type" : "get",
  "sender_id" : 1,
  "msg_id" : 1025
}
```

> Réponse

```json
{
  "type" : "get",
  "ack" : { "resp" : "ok" },
  "msg_id" : 1025,
  "date" : 1488463439214,
  "contents" : { "lat" : -49.202458, "lng": 20.687594 }
}
```
**Paramètres**

*Requête*

Paramètres | Objet | Description
--------- | ---  |-----------
sender_id | `ID` | ID unique du capteur 
msg_id    | `MSG_ID`  | Numéro unique du message

*Réponse*

Paramètres | Objet | Description
--------- | ---  |-----------
ack | `ACK` | Accusé de réception
msg_id    | `MSG_ID`  | Numéro unique du message
date      | `DATE`   | Nombre de milliseconde depuis 1970, timestampé par le bus
contents  | `CLASS_CONTENTS` | Payload spécifique à chaque classe [voir ici](#class-class_contents)

##get_last

`get_last` permet de récuperer un message du bus.

> Requête

```json
{
  "type" : "get_last",
  "sender_id" : 1
}
```

> Réponse

```json
{
  "type" : "get_last",
  "ack" : { "resp" : "ok" },
  "msg_id" : 1125,
  "date" : 1488463439214,
  "contents" : { "lat" : -49.202458, "lng": 20.687594 }
}
```
**Paramètres**

*Requête*

Paramètres | Objet | Description
--------- | ---  |-----------
sender_id | `ID` | ID unique du capteur 
contents  | `CLASS_CONTENTS` | Payload spécifique à chaque classe [voir ici](#class-class_contents)

*Réponse*

Paramètres | Objet | Description
--------- | ---  |-----------
ack | `ACK` | Accusé de réception
msg_id    | `MSG_ID`  | Numéro unique du message
date      | `DATE`   | Nombre de milliseconde depuis 1970, timestampé par le bus
contents  | `CLASS_CONTENTS` | Payload spécifique à chaque classe [voir ici](#class-class_contents)
# Objets

## ACK
L'objet `ACK` représente l'échec ou la réussite d'une action

> Réussite

```json

{
  "resp" : "ok"
}
```

> Echec

```json

{
  "resp" : "error",
  "error_id" : 401
}
```
## STRING
L'objet `STRING` représente une chaine de caractère, entouré de `"`

*Exemple* :
`"Gps 1"`
##MSG_ID
Numéro unique par capteur, représentant un message. *nombre entier*

##DATE
Le nombre de seconde depuis 1970 timestampé par le bus. *nombre entier*

> Exemple

```json
"date" : 1488463439214
```

##CLASS & CLASS_CONTENTS
###GPS
Les capteurs GPS font parties de la classe `"GPS"` .

####CLASS_CONTENTS
> Exemple

```json
{
    "type" : "send",
    "sender_id" : 2,
    "contents" : {
            "lat" : -49.2578645,
            "lng" : 20.2655465
    }
}
```
Le `CLASS_CONTENTS` de la classe `GPS` est un objet JSON avec deux champs:

- `lat` représentant la latitude, *décimal pointé*
- `lng` représentant la longitude, *décimal pointé*




###Accéléromètre
> Exemple

```json
{
    "type" : "send",
    "sender_id" : 2,
    "contents" : {
            "x" : 1.25,
            "y" : 1.47,
            "z" : 0
    }
}
```
Les capteurs de type accéléromètre font parties de la classe `"Accelerometer"` .

####CLASS_CONTENTS

Le `CLASS_CONTENTS` de la classe `Accelerometer` est un objet JSON avec trois champs:

- `x` représentant l'accélération sur l'axe X, *décimal pointé*
- `y` représentant l'accélération sur l'axe Y, *décimal pointé*
- `z` représentant l'accélération sur l'axe Z, *décimal pointé*

###Gyroscope

> Exemple

```json
{
    "type" : "send",
    "sender_id" : 2,
    "contents" : {
            "x" : 120,
            "y" : 300,
            "z" : 0
    }
}
```

Les capteurs de type gyroscope font parties de la classe `"Gyroscope"` .


####CLASS_CONTENTS

Le `CLASS_CONTENTS` de la classe `Gyroscope` est un objet JSON avec trois champs:

- `x` représentant l'orientation de l'axe X, *nombre entier*
- `y` représentant l'orientation de l'axe Y, *nombre entier*
- `z` représentant l'orientation de l'axe Z, *nombre entier*


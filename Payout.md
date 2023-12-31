---
layout: home
title: Payout 
nav_order: 5
---

# Payout

Un payout correspond à l'action de transférer ou retirer de la crypto-monnaie depuis votre portefeuille numérique.

### Initiation d'un paiement
Pour faire un paiement via l'Api, vous devez effectuer une requête HTTP POST à l'endpoint spécifié `/api/payements/payout`. Assurez-vous d'inclure les en-têtes nécessaires tels que `l'x-api-key` pour l'authentification API, et `l'x-signature` pour garantir l'intégrité des données.

**Description des paramètres**

***Liste des cryptomonnaies supportées et leur code***

| Title | Code |
| --- | --- |
| Bitcoin | btc |
| Ethereum | eth |
| Tron | trx |


| variables | type | description |
| --- | --- | --- |
| coin | string | Le code de la cryptomonnaie |
| address | string | Addresse de reception |
| amount | decimal | Montant de la transaction |

**Méthode HTTP :** POST

**Exemple de requête :**

``` http

http

POST /api/payements/payout HTTP/1.1
Host: <base_url>
Content-Type: application/json
x-api-key: <votre_cle_api>
Accept: application/json
x-signature: <la_signature_de_la_requete>
Content-Length: 300
{
  "amount": 100,
  "coin": "trx",
  "address": "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ"
}
```

``` bash
curl --location '<base_url>/api/payements/payout' \
--header 'x-api-key: <votre_cle_api>' \
--header 'x-signature: <la_signature_de_la_requete>' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data '{
  "amount": 100,
  "coin": "trx",
  "address": "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ"
}'

```
**Comment signer les requetes**

Lors de la construction de la donnée à signer pour la génération de la signature HMAC, le processus implique la concaténation du nom des paramètres avec leur valeur associée provenant de la requête. Cela crée une structure spécifique où chaque paramètre est représenté sous la forme "nom=valeur".

```
    dataToString = "coin=" + data.coin + "amount=" + data.amount + "address=" + data.address;
    secret = "VotreSecretDeSignature";
    $signature = SHA256(dataToString, secret);
```

```php

   <?php
   
    $data = [
        "address" => "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ",
        "amount" => 100,
        "coin" => "trx"
    ];
    $dataToString ="coin=".$data['coin']."amount=".$data['amount']."address=".$data['address'];
    $secretKey="votre_secret_defini_a_la_generation_de_la_cle";
    $signature = hash_hmac('sha256',$dataToString, $secretKey, FALSE);

```

**Format des réponses**

```
    {
    "status": true,
    "message": "",
    "data": {
        "id": "9a8e033a-9e2e-494c-a2c9-8641404fd3c1",
        "address": "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ",
        "amount": 100,
        "status": "PENDING",
        "coin": "trx"
    },

    "signature": "7d637a4f666f48be2cd9c118d07508314c42aa59e3354e583994e6a5aa49a773"
}

```

**Verification de la signature**

L'intégrité de nos réponses API est garantie par une signature. Voici la structure typique de notre réponse signée :


```php

    <?php 
    $receviedData = [
        "status" => true,
        "message" => "",
        "data" => [
            "id" => "9a8e033a-9e2e-494c-a2c9-8641404fd3c1",
            "address" => "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ",
            "amount" => 100,
            "status" => "PENDING",
            "coin" => "trx"
        ]
        'signature' => '300e0876809980406cc2e8c485de34a4f486472db4edc3d2a99c39874b782f75',
    ];

    $data=$receviedData['data'];
    $dataToString ="id=".$data['id']."address=".$data['address']."amount=".$data['amount']."coin=".$data['coin']; 
    $secretKey="votre_secret_defini_a_la_generation_de_la_cle";
    $expectedSignature = hash_hmac('sha256',$dataToString, $secretKey, FALSE);

    if(hash_equals($expectedSignature, $receviedData['signature'])){
        echo "signature valide";
    }else{
        echo "signature invalide";
    }

```

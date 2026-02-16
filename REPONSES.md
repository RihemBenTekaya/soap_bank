# TP2 — SOA / SOAP avec Java (Spring-WS)

Nom : Rihem BEN TEKAYA  
Classe : 4Info  
Matière : SOA et Microservices  

# A) Fork, clone et exécution

Le dépôt soap_bank a été cloné depuis GitHub avec la commande :

git clone https://github.com/RihemBenTekaya/soap_bank.git

Le projet a été compilé et exécuté avec Maven :

mvn clean install  
mvn spring-boot:run  

Le service SOAP a été démarré avec succès.

Le fichier WSDL est accessible via l’URL suivante :

http://localhost:8080/ws/bank.wsdl

Cela confirme que le service SOAP est correctement déployé.


# B) Lecture du contrat

## 1) Fichier XSD et son rôle

Le fichier XSD se trouve dans :

src/main/resources/bank.xsd

Le XSD définit le contrat du service SOAP selon l’approche contract-first.

Il décrit :

- les types de données
- les requêtes
- les réponses
- la structure des messages XML

Le WSDL et les classes Java sont générés à partir de ce fichier.


## 2) Éléments requête et réponse

GetAccountRequest
- accountId : string

GetAccountResponse
- account : AccountType
    - accountId
    - owner
    - balance
    - currency

DepositRequest
- accountId
- amount

DepositResponse
- newBalance

WithdrawRequest (ajouté)
- accountId
- amount

WithdrawResponse (ajouté)
- newBalance


## 3) Analyse du WSDL

Namespace :
http://example.com/bank

PortType :
BankPort

Opérations :
- GetAccount
- Deposit
- Withdraw

Endpoint :
http://localhost:8080/ws

Binding :
SOAP over HTTP


# C) Tests Postman

## Test GetAccount

Une requête SOAP a été envoyée pour consulter le compte A100.

Le service a retourné les informations du compte :

- accountId
- owner
- balance
- currency

Résultat : SUCCÈS


## Test Deposit

Une requête SOAP a été envoyée pour déposer 20.00 dans le compte A100.

Le nouveau solde a été retourné correctement.

Résultat : SUCCÈS


## Test Fault

Une requête Withdraw avec un montant supérieur au solde a été envoyée.

Le service a retourné un SOAP Fault :

faultstring : Insufficient balance

Résultat : SUCCÈS


# D) Ajout d’une fonctionnalité — Withdraw

## Fonctionnalité ajoutée

Une nouvelle opération Withdraw a été ajoutée pour retirer un montant depuis un compte.

## Fichiers modifiés

src/main/resources/bank.xsd  
Ajout de :
- WithdrawRequest
- WithdrawResponse

src/main/java/com/example/bank/endpoint/BankEndpoint.java  
Ajout de la méthode withdraw()


## Fonctionnement

L’opération Withdraw :

- vérifie que le compte existe
- vérifie que le montant est valide
- vérifie que le solde est suffisant
- met à jour le solde
- retourne le nouveau solde


## Tests Postman

Test valide :
Withdraw 10.00 depuis A100  
Résultat : nouveau solde retourné

Test erreur :
Withdraw 999999 depuis A100  
Résultat : SOAP Fault

# Conclusion

Le service SOAP bancaire a été déployé avec succès.

Les opérations suivantes fonctionnent correctement :

- GetAccount
- Deposit
- Withdraw

Les tests SOAP avec Postman ont confirmé le bon fonctionnement du service et la gestion des erreurs via SOAP Fault.

# Postman-API-Testing ![postman icon](https://camo.githubusercontent.com/e0aa4b3bb9af7d3610dd65656751f3940ef645e1e3e5ff727abecec2accfb31b/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f506f73746d616e2d4646364333373f7374796c653d666f722d7468652d6261646765266c6f676f3d506f73746d616e266c6f676f436f6c6f723d7768697465)
## Trello boards, lists and cards are managed through the API

## 
```POST``` **Create A Board**

In the query parameters I pass the API key, the token and use Postman's built-in function to generate a random name - ```{{$randomUserName}}```

In the **Tests** tab I check if the response has a correct status and I assign the ID of the created Board to the collection variable. 

```javascript
let response;
pm.test('Verify the status of the response', () => {
    pm.response.to.have.status(200);
    response = pm.response.json();
    pm.collectionVariables.set('BoardID', response[0].id);
});
```
```GET``` **Show Boards**

In the second request as parameters I pass the API key and the token only. The **Tests** tab looks identically as on the previous query. 

```POST``` **Create A List**

In the third query parameters besides the API key and the token I pass the collection variable ```{{ListName}}``` which is created in the **Pre-Tests** tab. The code in the **Pre-Tests** tab is:
```javascript
const listName = 'Testowa Lista ' + Math.floor((Math.random() * (100 - 1) + 1));
pm.collectionVariables.set('ListName', listName);
```
In the **Tests** tab I just assign the ID taken from the response as the collection variable.

```javascript
const response = pm.response.json();
pm.collectionVariables.set('ListID', response.id);
```

```POST``` **Create A Card**

In the fourth query, I do pretty much the same thing as in the previous query, but with another variable. 

```javascript
const cardName = 'Testowa Karta ' + Math.floor((Math.random() * (100 - 1) + 1));
pm.collectionVariables.set('cardName', cardName);
```

```GET``` **Get Cards On Board**

In this query I just display all Cards on the Board:

```
{{baseURL}}/boards/{{BoardID}}/cards?key={{key}}&token={{token}}
```
```DEL``` **Delete A Board**

In this query in the **Tests** tab I go with for loop to pick the last Board ID. 
```javascript
const response = pm.response.json();
const numberOfObjectsInResponse = Object.keys(response);
for (let i = 0; i < numberOfObjectsInResponse; i++) {
    if(i = numberOfObjectsInResponse){
        pm.environment.set('BoardID', response[i].id);
    }
} //setting the ID of the last created Board to delete
```

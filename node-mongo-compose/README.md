```sh
npm i --save express@4.15.3 mongoose@4.11.1 node-restful@0.2.6 body-parser@1.17.2 cors@2.8.3
#remova a pasta node modules

# execute composer da seguinte forma:
docker-compose up

# post para clients
curl --header "Content-Type: application/json" --request GET http://localhost:3000/clients
```
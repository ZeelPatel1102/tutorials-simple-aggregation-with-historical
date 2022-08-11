# SubQuery - Tutorials-simple-aggregation-with-historical

This project demonstrates historical queries where the data returned is dependent on the blockheight provided. This allows queries to retrieve historical data instead of the value at the current blockheight.

## Preparation

```
git clone https://github.com/subquery/tutorials-simple-aggregation-with-historical.git
cd tutorials-simple-aggregation-with-historical
yarn install
yarn codegen
yarn build
```

## Indexing and Query

#### Run required systems in docker

Under the project director run following commands within the docker folder:

```
cd docker
docker-compose pull && docker-compose up
```

#### Query the project

Open your browser and head to `http://localhost:3000`.

Finally, you should see a GraphQL playground is showing in the explorer and the schemas that ready to query.

Wait until the indexer has indexed past block 7013941. This should take a few minutes. Then run the following query. 

````graphql
query{
  sumRewards(blockHeight:"7013941" first:3 orderBy:BLOCKHEIGHT_DESC
  filter:{id:{equalTo:"121FXj85TuKfrQM1Pdcjj4ibbJNnfsqCtMsJ24rSvGEdWDdv"}}
    ){
    nodes{
      blockheight
      id
      totalReward
    }
  }
}

````
Result:

````
{
  "data": {
    "sumRewards": {
      "nodes": [
        {
          "blockheight": 7013941,
          "id": "121FXj85TuKfrQM1Pdcjj4ibbJNnfsqCtMsJ24rSvGEdWDdv",
          "totalReward": "20170219857"
        }
      ]
    }
  }
}
````

Now run the same query but change the blockheight to a value in the past such as 7000064.

````
query{
  sumRewards(blockHeight:"7000064" orderBy:BLOCKHEIGHT_DESC
  filter:{id:{equalTo:"121FXj85TuKfrQM1Pdcjj4ibbJNnfsqCtMsJ24rSvGEdWDdv"}}
    ){
    nodes{
      blockheight
      id
      totalReward
    }
  }
}
````
Results:
````
{
  "data": {
    "sumRewards": {
      "nodes": [
        {
          "blockheight": 7000064,
          "id": "121FXj85TuKfrQM1Pdcjj4ibbJNnfsqCtMsJ24rSvGEdWDdv",
          "totalReward": "10901386603"
        }
      ]
    }
  }
}

````

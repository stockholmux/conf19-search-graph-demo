# conf19-search-graph-demo

## Project setup
```
npm install
```

## Requirements
* RedisGraph w/ Search integration
* RediSearch with the correct branch

Note: you must have RediSearch earlier in your `redis.conf` file modules defintions

## Setup
In the `dataload` directory, you'll need to run the RedisGraph bulk loader Python script as per that directory's readme

After that is complete run the following command from redis-cli to full-text index the nodes:
```
> GRAPH.QUERY MAMMALS "CALL db.idx.fulltext.createNodeIndex('Species','description')"
```

## Run
```
node server.js -p 6379 -a yourverysecurepassword -h localhost
```

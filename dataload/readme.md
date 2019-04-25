Use the [RedisGraph bulk loader](https://github.com/RedisGraph/redisgraph-bulk-loader)
Query example:

```
GRAPH.QUERY MAMMALS "MATCH (s:Species)-[*]->(x)<-[*]-(c:Species) WHERE c.fullname = 'Sorex hoyi' AND s.fullname = 'Sorex haydeni'  RETURN x.name, labels(x)"
```
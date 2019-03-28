Import command line:

python bulk_import.py MAMMALS -n Class.csv -n Family.csv -n Genus.csv -n Order.csv -n Species.csv -r IN_CLASS.csv -r IN_FAMILY.csv -r IN_GENUS.csv -r IN_ORDER.csv

Query example:

GRAPH.QUERY MAMMALS "MATCH (s:Species)-[*]->(x)<-[*]-(c:Species) WHERE c.fullname = 'Sorex hoyi' AND s.fullname = 'Sorex haydeni'  RETURN x.name, labels(x)"
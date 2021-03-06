# bloodhound-disco

Get turnt.

## Cyphers

Bloodhound uses `neo4j` to store data and retrieve paths. `neo4j` uses `Cypher` to construct these queries.

`Cypher` is amazingly fluid and clever and I love it. It's also a huge fucking pain in the arse.

### Get any lateral movement path to high value targets from any owned users

```cypher
MATCH (u:User)
WHERE u.owned = True
MATCH (n {highvalue:true}),p=shortestPath((u)-[r:SQLAdmin|ExecuteDCOM|CanRDP|CanPSRemote|AdminTo*1..]->(n))
RETURN p
```

### Get any lateral movement path from any owned user

```cypher
MATCH (u:User)
WHERE u.owned = True
MATCH (n),p=shortestPath((u)-[r:SQLAdmin|ExecuteDCOM|CanRDP|CanPSRemote|AdminTo*1..]->(n))
RETURN p
```

### Get any user with a description set

```cypher
MATCH (u:User) WHERE u.description =~ ".*" return u
```

### Get any outdated system

```cypher
MATCH (H:Computer) WHERE H.operatingsystem =~ '.*(2000|2003|2008|xp|vista|7|me)*.' RETURN H
```

### Match user accounts which have not logged in in over 90 days

```cypher
MATCH (u:User) WHERE u.lastlogon < (datetime().epochseconds - (90 * 86400)) and NOT u.lastlogon IN [-1.0, 0.0] RETURN u
```

### Match Domain Admins with active sessions

```cypher
MATCH (u:User)
```



## Evidencing

> Screenshotting from the neo4j browser console looks less cool but is more readable for the client and permits export to CSV and JSON for clean tabular magic inside reports. (Who wants to read 50 accounts from a screenshot)

### Get the total number of users which have not logged in in over 90 days

```cypher
MATCH (u:User) WHERE u.lastlogon < (datetime().epochseconds - (90 * 86400)) and NOT u.lastlogon IN [-1.0, 0.0] RETURN count(u)
```

### Get all user with hasSPN set

```cypher
MATCH (u:User) WHERE u.hasspn = true return u.name
```

### Get all computers with hasLAPS enabled

```cypher
MATCH (u:Computer) WHERE u.haslaps = true return u.name
```


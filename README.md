# galera-rolling-update
###### Command line tool that will apply a rolling update to a Galera cluster behind a MaxScale load balancer.

For each node in cluster (master node last)
1. Enter maintenance mode by marking the node for maintenance in MaxScale.
2. Remove the node from the cluster by calling `SET GLOBAL wsrep_OSU_method='RSU'`, then `SET GLOBAL wsrep_cluster_address="gcomm://"`
3. Execute each of the DDL statements
4. Rejoin the cluster by calling `SET GLOBAL wsrep_cluster_address=XXXX`, then `SET GLOBAL wsrep_OSU_method='TOI'`, and then waiting until `wsrep_local_state` = 4.
5. Leave maintenance mode by clearing the maintenance flag in MaxScale.

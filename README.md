# Docker Postgres Testbench

Creates two connected containers on a docker network that can communicate with each other.

`vim` is installed in both master and slave for customization of pg config.

1. Start the containers in the background:
    
    ```
    docker-compose up -d
    ```

2. View the docker network to get the local IPv4 address of each.
    
    ```
    docker network inspect pgreplica_db_network
	=>
	...
	  "Containers": {
		  "ccd3a7e20bb9fe16878554350f7a706d278dc260c8795b26a28c6fa3b84ef455": {
			  "Name": "pgreplica_master_1",
			  "EndpointID": "88a49739d5dd346bd08170fcc0db04e35ecad29d3066dc9ce25850f4fa8bf999",
			  "MacAddress": "02:42:ac:12:00:02",
			  "IPv4Address": "172.18.0.2/16",
			  "IPv6Address": ""
		  },
		  "fc888902592e536d8b407409aab2361b3d1f2b46180b486941b7b0ccc0517278": {
			  "Name": "pgreplica_slave_1",
			  "EndpointID": "6d01a6506b372f5092d205f0661987116d6ee19cb03fc1ddef2a76c538b8250d",
			  "MacAddress": "02:42:ac:12:00:03",
			  "IPv4Address": "172.18.0.3/16",
			  "IPv6Address": ""
		  }
	  },
	...
    ```

3. Run a shell in containers and ping the other container to verify the connection:
    
    ```
    docker exec -it pgreplica_master_1 bash
    docker exec -it pgreplica_slave_1 bash

    /# ping -p 5432 172.18.0.3
    /# ping -p 5432 172.18.0.2
    ```

## Other

Reset the database containers & config. (pg image uses a volume that stores the config and persists between container runs)

```
docker-compose rm -v
```

## Acknowledgments
Docker bidirectional networking setup sourced from [Abdelrahman Hosny's Blog](https://abdelrahmanhosny.com/2015/07/01/3-solutions-to-bi-directional-linking-problem-in-docker-compose/)

# Stateless vs Stateful Connections

## Table of Contents
- [Stateless Connection](#stateless-connection)
- [Stateful Connection](#stateful-connection)

## Stateless Connection
- Stateless means that you **do not need to keep track of the connection status**.
  - There is only **success** or **failure**.
	- If the server needs to go down, the server will stop receiving incoming requests.
	- For example, there is usually a load balancer that distributes incoming requests between multiple servers.
		- If a server needs to go down for a bit, it informs the load balancer of it. Therefore it finishes dealing with only the already received requests and then go down. Other requests can be distributed among the alive servers.

## Stateful Connection
- Stateful means that you **need to know the connection status**.
	- Problem: since the client and server are directly connected, a crash in the connected server leads to the connected clients to crash as well.
		- It is not easy to implement load balancing to WebSocket because the client and server is directly connected (stateful) in real-time.
			- The connection only ends when one disconnects.

- Ref: https://www.youtube.com/watch?v=o-EkdZW4zbA
- No technical limit as based on memory of server it can accept as many connections as possible until it crashes!!!
- From client side there'd be a limit as the port related to a IP address in header field is 16bit that translates to ~64K unique ports. The minimum start port that can be set is 1025. The maximum end port (based on the range being configured) can't exceed 65535.
- With layer 4 proxying there'll be the same limit as above as now proxies will become client while connecting to backend servers. In this case either we add more backends to which proxy can connect to or make server listen on different ports.
- HAProxy tried a way where basically the reverse proxy can act as a router itself, thus keeping track of the clients in a NAT like table and treating each TCP connection from client and forwarding it to backend servers as it is. Rather than terminating the client request and then forwarding the request from proxy to backend.
- Another solution be to add more IPs to the single backend host. Like for example having a NIC with multiple IP addresses and backend is listening to both, while Proxy would have to have some load balancing functionality but the backend host will still be a single machine.
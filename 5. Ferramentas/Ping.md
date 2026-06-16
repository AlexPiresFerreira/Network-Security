- ICMP (Internet Control Massage Protocol)
	- ECHO_REQUEST --> Enviado 
		----------------------> 
	- Resposta --> ECHO_RESPONSE
	    <---------------------- 

- PING 
	- ping [opções]  {nome_do_host | IP_destinio}

	- Linux
		![[Pasted image 20251227154734.png]]
		![[Pasted image 20251227154914.png]]
		![[Pasted image 20251227155141.png]]
	
		- Opções de PING

			- •         -a -> Audible ping
			- •         -A -> Adaptative ping
			- •         -b -> Broadcast ping
			- •         -B -> Bound address
			- •         -c -> Count
			- •         -d -> SO_DEBUG
			- •         -D -> Timestamp
						- timestamp --> unix time
			- •         -f -> Flood 
			- •         -F -> Flow Label 
			- •         -h -> Help 
			- •         -i -> Interval
			- •         -I -> Interface (i)
			- •         -l -> Preload 
			- •         -L -> Loopback
			- •         -m -> Mark 
			- •         -M -> MTU 
			- •         -n -> Numeric Output Only
			- •         -N -> Node Information Query
			- •         -O -> Outstanding Packet
			- •         -p -> Pattern
			- •         -q -> Quiet Output
			- •         -Q -> Quality of Service
			- •         -r -> Bypass Routing Tables
			- •         -R -> Record Route
			- •         -s -> Packet Size
			- •         -S -> Set Socket Sndbuf
			- •         -t -> Packet Time to Live
			- •         -T -> Timestamp Options
			- •         -s -> Packet Size
			- •         -S -> Set Socket Sndbuf
			- •         -t -> Packet Time to Live
			- •         -T -> Timestamp Options
	
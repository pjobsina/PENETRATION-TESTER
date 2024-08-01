![[pivoting.png]]
# Pivoting
- Utilizing multiple hosts to cross `network` boundaries you would not usually have access to. 
- This is more of a targeted objective. 
- The goal here is to allow us to move deeper into a network by compromising targeted hosts or infrastructure.
# Tunneling
- We often find ourselves using various protocols to shuttle traffic in/out of a network where there is a chance of our traffic being detected. 
- For example, using HTTP to mask our Command & Control traffic from a server we own to the victim host. 
- The key here is obfuscation of our actions to avoid detection for as long as possible. 
- We utilize protocols with enhanced security measures such as HTTPS over TLS or SSH over other transport protocols. 
- These types of actions also enable tactics like the exfiltration of data out of a target network or the delivery of more payloads and instructions into the network.

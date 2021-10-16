# GeyserOnEC2

## Use Case

You want to connect to a Java Minecraft server from your tablet, i.e. using a Bedrock client.

## Proposal

Install [Geyser proxy](https://geysermc.org/) in a AWS EC2 instance in a region near you or the server.

## What

You can use the Cloudformation file in this repository:
* To create a [Geyser proxy](https://geysermc.org/) in the destination region that will only accept connections from your public IP and direct the traffic from your Bedrock client to the Java Minecraft server

### Design

![Design](/Images/Design.jpg?raw=true)

### Usage

2 mandatory values to inform:
* Your **public IP** - The proxy will only accept connections coming from this IP
* **Server domain** (or IP) - The domain (or IP) of the Java Minecraft server

4 optional value:
* Instance type - Set to `t2.micro` given that it is usually part of AWS **free tier** but you can use any **64-bit x86** instance
* Subnet CIDR - IP range used in your private AWS network where the proxy is deployed. Default: `10.0.0.0/28` (minimum valid range)
* Server port - Port where the Java Minecraft server is listening. Default: `25565`
* Geyser port - Port where you want to connect you Bedrock client. Default: `19132`

### Outputs

Once the Cloudformation stack is deployed, in the `Output` tab you will find the following values:
* **Direct Geyser IP:port** - IP & port where you can connect your Bedrock client
* **AWS instance ID** - The ID of the EC2 instance executing the proxies. You might need this information in case you want to have a look inside the EC2 instance. Notice that you can easily connect to the EC2 instance with Session Manager

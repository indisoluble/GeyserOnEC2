# AcceleratedGeyser

## Use Case

You want to connect to a Java Minecraft server far from your region, for example you need to connect to a server located in UK from Australia (or vice versa). Also you want to use your PC (i.e. Java client) and tablet (i.e. Bedrock client).

## Proposal

A direct connection over long distances usually routes your TCP/UDP packages over many different networks which produces a high ping and increased lag. However, if there is an AWS region in the country where the server is located, you can use [AWS Global Accelerator](https://aws.amazon.com/global-accelerator) to get rid off most of the intermediate networks.

## What

You can use the Cloudformation file in this repository:
* To create a [HAProxy](http://www.haproxy.org/) in the destination region that will only accept connections from your computer and direct the traffic from your Java client to the Java Minecraft server
* Also, to create a [Geyser proxy](https://geysermc.org/) in the destination region that will only accept connections from your computer and direct the traffic from your Bedrock client to the Java Minecraft server
* And to enable [AWS Global Accelerator](https://aws.amazon.com/global-accelerator) to speed up your traffic to the proxies

### Design

![Design](/Images/Design.jpg?raw=true)

### Usage

2 mandatory values to inform:
* Your **public IP** - The proxies will only accept connections coming from this IP
* **Server domain** (or IP) - The domain (or IP) of the Java Minecraft server

4 optional value:
* Instance type - Set to `t2.micro` given that it is usually part of AWS **free tier** but you can use any **64-bit x86** instance
* Subnet CIDR - IP range used in your private AWS network where the proxies are deployed. Default: `10.0.0.0/28` (minimum valid range)
* Server port - Port where the Java Minecraft server is listening. Default: `25565`
* Geyser port - Port where you want to connect you Bedrock client. Default: `19132`

### Outputs

Once the Cloudformation stack is deployed, in the `Output` tab you will find the following values:
* **Accelerated Geyser domain:port** - URL where you can connect your Bedrock client. Lag on this connection is reduced thanks to [AWS Global Accelerator](https://aws.amazon.com/global-accelerator)
* **Direct Geyser IP:port** - IP & port where you can connect your Bedrock client. This is a direct connection to the proxy and, normally, ping will be higher than if you use the accelerated domain:port
* **Accelerated HAProxy domain:port** - URL where you can connect your Java client. Lag on this connection is reduced thanks to [AWS Global Accelerator](https://aws.amazon.com/global-accelerator)
* **Direct HAProxy IP:port** - IP & port where you can connect your Java client. This is a direct connection to the proxy and, normally, ping will be higher than if you use the accelerated domain:port
* **AWS instance ID** - The ID of the EC2 instance executing the proxies. You might need this information in case you want to have a look inside the EC2 instance. Notice that you can easily connect to the EC2 instance with Session Manager

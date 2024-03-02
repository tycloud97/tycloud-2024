1. 

Its hard to understand CIDR calculation and allocation when we create our first virtual private cloud and subnets. I hope this article would be helpful for beginners.

Let’s first understand why CIDR allocation is so important while creating VPCs. When we have a single VPC then CIDR allocation may not be that important. However, when we need interconnectivity between multiple VPCs in same account or across different accounts, then its very important to plan ahead and define our CIDR blocks accordingly. When we have same CIDR blocks or overlapping CIDR blocks in two different VPCs, then we can’t use features like VPC peering.

So, now we understand that allocating CIDR blocks correctly is a critical task, let’s understand what CIDR is. CIDR, which stands for Classless Inter-Domain Routing, is an IP addressing scheme that improves the allocation of IP addresses. I had written an answer to a stackoverflow question (https://stackoverflow.com/questions/51734945/cidr-address-is-not-within-cidr-address-from-vpc/56051282#56051282), where I have explained how CIDR address range works. I hope this will provide more clarity in CIDR allocation.

Now, since we have understood how CIDR address range works, lets take an example CIDR — 10.97.224.0/20 and create one VPC with 4 subnets (2 subnets may act as public subnets and another 2 subnets may act as private subnets.)

Using 10.97.224.0/20 CIDR block, as explained above, the first 20 bits are fixed and we can use last 12 bits to create a range of IP addresses. So, we can have total 2�� i.e. 4,096 IP addresses. Since, we plan to create 4 subnets, we can divide the IP address range into 4 parts i.e. each subnet can have 1,024 IP address. Since, we are dividing IP address range into 4 (i.e. 2�) parts our CIDR blocks for each subnet should have /22 suffix. (/22 means we can use last 10 bits to form a range of IP addresses and thus we will have total 2�⁰ (1024) addresses).

Hence, we can have following CIDR configuration for our VPC.

VPC name => my-vpc
CIDR Block => 10.97.224.0/20
First IP => 10.97.224.0
Last IP => 10.97.239.255

You can use some CIDR interpretation tools such as https://www.ipaddressguide.com/cidr to check IP address range for your CIDR block.

Accordingly, our first subnet will have following CIDR configuration. This CIDR has only one difference that it has /22 suffix. That means it will use first 1,024 IP addresses starting from 10.97.224.0 to 10.97.227.255.

subnet name => my-public-subnet-a
CIDR Block => 10.97.224.0/22
First IP => 10.97.224.0
Last IP => 10.97.227.255

For second subnet, CIDR block can start immediately after first subnet ends. Hence the CIDR blocks start from 10.97.228.0 and ends at 10.97.231.255.

subnet name => my-public-subnet-b
CIDR Block => 10.97.228.0/22
First IP = 10.97.228.0
Last IP = 10.97.231.255

For third subnet, CIDR block can start immediately after second subnet ends. Hence, the CIDR blocks start from 10.97.232.0 and ends at 10.97.235.255.

subnet name => my-private-subnet-a
CIDR Block => 10.97.232.0/22
First IP = 10.97.232.0
Last IP = 10.97.235.255

For fourth subnet, CIDR block can start immediately after third subnet ends. Hence, the CIDR blocks start from 10.97.236.0 and ends at 10.97.239.255 (As you can see, this is the last IP address in our master CIDR block).

my-private-subnet-b
CIDR Block => 10.97.236.0/22
First IP => 10.97.236.0
Last IP => 10.97.239.255

Now, we can use these CIDR configurations while creating our VPC and subnets.

Neu ban dat ngoai 

IP addressing for your VPCs and subnets
PDF
RSS
IP addresses enable resources in your VPC to communicate with each other, and with resources over the internet.

Classless Inter-Domain Routing (CIDR) notation is a way to represent an IP address and its network mask. The format of these addresses is as follows:

An individual IPv4 address is 32 bits, with 4 groups of up to 3 decimal digits. For example, 10.0.1.0.

An IPv4 CIDR block has four groups of up to three decimal digits, 0-255, separated by periods, followed by a slash and a number from 0 to 32. For example, 10.0.0.0/16.

An individual IPv6 address is 128 bits, with 8 groups of 4 hexadecimal digits. For example, 2001:0db8:85a3:0000:0000:8a2e:0370:7334.

An IPv6 CIDR block has four groups of up to four hexadecimal digits, separated by colons, followed by a double colon, followed by a slash and a number from 1 to 128. For example, 2001:db8:1234:1a00::/56.

For more information, see What is CIDR?

Quan ly va duy tri cho to chuc cua ban
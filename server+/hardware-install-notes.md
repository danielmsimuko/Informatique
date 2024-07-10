````.text
Static RAM:  is used as cache memory for computers. Employs flip-flop tech to store data. 
Dynamic RAM: uses transistors and capacitors to store data. Static RAM is faster, but more expensive

Blade servers take up the least amount of physical space of all server types

When installing cabling, it is important to consider the bend radius. This is the minimum radious that a cable can be bent
without damaging it. The greater the flexibility of material used, the smaller the radius. I.e twisted pair coaxial cable
is more flexible than fibre optic and thus has a smaller bend radius. 

BIOS settings can be configured to include a password that prevents users from accessing it at startup. The Bios resides on a
an EPROM read-only chip
````
````.text
A RAID Controller is device that manages storage devices such as hard drives.

RAID0 - utilises disk stripping across muliple drives
RAID1 - uses disk mirroring, provides data availability in event of another drive crashing. Requires a minimum of 2 hard drives
RAID5 - uses disk strip with parity. Will provide data in event 1 hard drive crashes. Needs 3 drives minimum
RAID10 - uses disk mirror and stripping. Provides data if one drive crashes. Needs 4 hard drives minimum

An IMPI (intelligent platform management interface) can be used to diagnose issues affecting servers. Management modules
on blade servers provide access to single point of control for ALL servers on a chasis, so you can diagnose issues affecting
one via another. 
````
````.text
USB/CD-ROM can be used as removable media capable of being transfered offsite quickly. As such, needs to be safeguarded
more than other media types. 

Cat6 shielded twisted pair cabling is capable of spanning distances of 100yards with max speeds of 10gbps. MultiMode fibre is also capable of doing 
around 30-85yards with max bandwidth of 10gbps. Shielded twisted pair comes out on top however as it provides shielding 
against electromagenetic interfearance. (emi)

Liquids and Switch Mode Power Supplies both regulate heat on a computer server. An SMPS is a power supply unit that converts
electrical power into an efficient way of using a switching regulator. Liquid cooling involves pumping coolant constantly 
to keep the machines temp low. 


````
````.text
Dual Corded Power Distribution Units for redundancy is a good idea for multiserver systems. 
Each PDU imput cord goes into different UPS's. This provided full redundancy and there is no single point of failure
````
````.text
When looking for instanteanous data access and low latency, hard drives with higher RPM's would be more important than 
any other proposed metric. 

Tower servers have fewer heat issues and are more extensible than their corresponding rack server systems. Towers have 
fewer components next to each other reducing heat buildup and can be uprated by adding another tower. Rack servers 
have the issue that once the space within the rack is finished, no more servers can be added. 
````
````.text
Cache memory has the largest effect on procesing efficiency for large volumes of data being sent and recieved simultaneously. 
Bigger cache values allow the CPU to work more effectively. 
````
````.text
In a network where security is a concern and devices must be limited to specific ports and servers, LUN zoning is a great idea. 
It is essentially creating smaller networks within a larger storage networking environment.

KBM (kernal based virtual machine) provides an infrastructure for virtualisation on a linux machine. Allows the system to create 
and run vm's run by the hypervisor. 
````
````.text
When dealing with upgrades on servers, any component changes should always be met with firmware updates/changes too. This can
mean driver downloads, or BIOS changes. Always look for firmware updates before anything else. 

1Gbps network requires cat5e, cat6 or cat6 cabling. These cables require RJ45 connectors. SFP connector are useful for fibre
optic and go upto 10gpbs. Straight tip (ST) connectors are fibre optict aswell but much slower

A host bus adapter (HBA) is a device that provides connectivity between a host device like server and storage or network device.
It takes the processing tasks of data retrieval away from the CPU, relieving it and improving its performance as a result. 
                                                                                                                                              
````
````.text
A PSU converts AC into DC for use by the computer. Mains electricity is usually 230v at 50hz in Europe, Asia, Africa. 

VNC is an open source graphics based softare that allows remote connectivity to computers. It is done via remote frame buffer 
(RFB) protocol.

DDR4/5 is some of the fastest RAM type for data transfer. Allows for 16/32 data transfer cycles per clock making data access
for the computer faster and easier.

CPU's have the fastest access to L1 cache memory. If it cannot find what it needs in l1, it goes to level 2 and so on.
The bigger the size of cache, the faster the CPU.  

Computers need cooling to dissipate the the heat. Fans and liquid cooling are the best most inexpensive solutionss fo this. 

Calculating disk capacity, you multiply number of sectors X sector size x bytes. This number is then converted from bytes to 
megabytes. Each megabyte is 1mill bytes or 1,048,576 bytes in base 2. You should always use base 2 for calculating storage for
software installations instead.
````
````.text
Solid State drives use non volatile memory to store large amounts of data. With no moving parts inside. They are faster,
more reliabe, and consume less power. For energy efficient storage of large files, they are a good choice.  

iDRAC - Intergrated Dell Remote Access is a way to manage a dell poweredge server remotey. The controlling hardware for iDrac Is 
embedded into each dell poweredge server and removes the need for an OS or hypervisor for remote access. You can update, monitor,
deploy and maintain without software agents.
````
````.text
UPS can help computer systems protect against power loss and voltage spikes. A remote agent can also be used to manage these
remotely, giving internal temperature, voltage and load information. 

Rack mounted servers have the advange of being lighter, less noisier and simpler to cable over tower servers. In many cases, 
rack servers are preffered over tower unless you have the equivalent of a server room with dedicated space, cooling  and cabling.

Cable troughs, raceways and ties can be used for placement and routing. Correct installation of fibre optic cables ensure optimum power,
low installation cost, higher reliability and maintenance. Good cabling is essential for that. 

RAID level 1 is also called disk mirroring. It involves data being written to two identical disks so one provides backup to the 
other. 
````



# Physical and Network Access layer


In this phase, we check the network hardware, specifically network cables and ports.

- Is the power on?
- Is the network cable plugged in?
    - In VirtualBox, go to the VM settings, Network, select the active interfaces, click "Advanced" and make sure the checkbox "Cable connected" is checked.
    - In the case of a physical UTP cable, use a cable tester to check whether it is working correctly
- Are the Ethernet port LEDs on, both on the machine and on the switch?
    - If not, test the cable
    - Some switches have a different color for e.g. FastEthernet (100Mbps) and Gigabit Ethernet. Check whether you see the expected color.
- Use the command `ip link`
    - `UP`: ok, the interface is connected
    - `NO-CARRIER`: no signal on this interface



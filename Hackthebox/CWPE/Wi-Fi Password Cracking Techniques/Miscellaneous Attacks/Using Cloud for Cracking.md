## Setting up the VM instance

In this example, we will use `Google Cloud` to create a virtual machine (VM) for password cracking. However, it is also possible to set up GPU-enabled VMs for the same purpose using other cloud providers such as [Amazon AWS](http://aws.amazon.com/) or [Microsoft Azure](https://azure.microsoft.com/en-us/).

![](Using%20Cloud%20for%20Cracking-20261006-084827.png)

This will navigate us to the Compute Engine API page, where we need to click `enable`.

![](Using%20Cloud%20for%20Cracking-20261006-084836.png)

Then, we can click the hamburger icon on the top `left` of the screen and click `vm instances`.

![](Using%20Cloud%20for%20Cracking-20261006-084848.png)

Once we are here, we can click `create instance`.

![](Using%20Cloud%20for%20Cracking-20261006-084917.png)

![](Using%20Cloud%20for%20Cracking-20261006-084921.png)

![](Using%20Cloud%20for%20Cracking-20261006-084947.png)

Once the VM is created, it will appear in the dashboard. To connect to it, simply click the `SSH` button associated with the instance.

![](Using%20Cloud%20for%20Cracking-20261006-084955.png)

This should open a new tab that will automatically connect to our instance.

![](Using%20Cloud%20for%20Cracking-20261006-085000.png)

Once we are here, we can run `sudo su` and get root access.

![](Using%20Cloud%20for%20Cracking-20261006-085006.png)

Then, we can simply install `hashcat` with the following command

```sh
3kjS@htb[/htb]$ apt-get install hashcat
```
#### Running our Attack

As previously noted, using a cloud-based approach provides access to a significantly higher number of cores. To initiate the attack, we can use the same command as outlined earlier.

```sh
3kjS@htb[/htb]$ hashcat -a 3 -m 22000 -w 4 hash --increment --increment-max 13 --increment-min 8 -1 likelychars.hcchr ?1?1?1?1?1?1?1?1?1?1?1?1?1
```

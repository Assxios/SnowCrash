# SnowCrash

![snowcrash](files/snowcrash.png)

42 project which aims to introduce us to computer security.

SnowCrash is a **CTF** like project where you have to solve a challenge in order to get a flag to move on to the next level. There is a total of 15 levels (10 regular levels and 5 bonus levels). 

It will test your knowledge in:
- `UNIX` based systems
- Different programming languages such as `ASM, Perl, Lua, PHP`
- Network packet data analysis
- `Shell` scripting

Of course since this is an introduction none of the exercises are too tricky, that being said some of them are a bit more challenging than others.

# Tips

## Hints

First off, I will remind you that you should try to solve the levels by youself, make sure you have tried **EVERYTHING** before asking for help (*especially if you want to continue this branch of projects*).

For each level there is a folder named `Ressources` which includes a file named `Read.md`, it contains the solution to the level. However, each of those `Read.md` start with a `HINT` and then have an `ANSWER` section. If you are really stuck feel free to take look at the `HINT` section (*I made sure to be as vague as possible*) !

## VM / SSH

As the subject specifies you should use SSH to connect to the VM. However, the video to install and configure the VM is not available on the intranet anymore.
Therefore here are the following steps (**made for on-campus usage**):
<details>
	<summary>INSTALL THE VM</summary>
		<p>First, download the ISO file on the intranet <i>(I recommend storing it on your sgoinfre).</i> Once it's downloaded let's create a new VM on VirtualBox, click the <code>New</code> button such as:<br>
		<img src="files/new.png"/><br>
		For the memory size the recommended amount is way enough, the VM is extremely light don't bother giving it extra memory it won't use.<br>
		For the hard disk choose the <code>Do not add a virtual hard disk</code> option.<br>
		Now we need to mount the ISO file to the VM we just created, click on the <code>Settings</code> of our VM and then head to the <code>Storage</code> tab. Under <code>Storage Devices</code> you should see a CD icon which say empty, select that and them head over to <code>Attributes</code>, once again click on the CD icon and select the <code>Choose a disk file</code> option such as:<br>
		<img src="files/storage.png"/><br>
		Now simply choose the ISO file and Voila !</p>
</details>
<details>
	<summary>SSH</summary>
		<p>This step is only useful if you are <b>having issues</b> connecting to your VM through SSH from the same computer, if your ip starts with <code>10.1</code> then we're going to have to create a host only network adapter.<br>
		In VirtualBox, on the top left click on <code>File</code> and then select <code>Host Network Manager</code> from the drop down menu, from there just click on the <code>Create</code> button and your adapter should appear such as:<br>
		<img src="files/adapter.png"/><br>
		Now we just go back to the settings of our VM and head to the <code>Network</code> tab, we need to change the <code>Attached to</code> from <code>NAT</code> to <code>Host-Only Adapter</code> and select our newly created adapter from the <code>name</code> drop down menu, such as:<br>
		<img src="files/network.png"/><br>
		You should now be able to connect to your VM through SSH ! <b>This is not the only way to do this just an example of what I did, do what works best for you !</b></p>
</details>

## Tools

We heavily recommend using the following tools / commands:
- **GDB** *(Please take the time to learn how to use it, seriouly !)*
- **strings**
- **Wireshark**
- **curl**
- **netcat**

You will also have to write your own programs / scripts to solve the levels. Pick whichever language works best for you.<br>
Good luck and enjoy !

## MADE WITH LOVE BY :

- [execrate0](https://github.com/execrate0/) aka **ahallain**
- [Assxios](https://github.com/assxios/) aka **droge**

# SSH

The [secure shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) is the preferred mechanism for contacting remote systems.  All traffic between the endpoints is encrypted.

## Key
SSH works through the use of an "SSH key", an assymmetric encryption key pair that allows the end-to-end communication to proceed.

### Generating an SSH Key

#### Windows (10+)
1. Check to see if you have the OpenSSH client installed:
	2. Open the `Settings` panel, then click `Apps`.
	3. Under the Apps and Features heading, click `Optional Features`. ![[OptionalFeatures.png]]
	4. Scroll down the list to see if `OpenSSH Client` is listed.
		1. If not, click the plus-sign next to Add a feature.
		2. Scroll through the list to find and select `OpenSSH Client`.![[OptionalFeatures2.png]]
		3. Click `Install`
2. Open `Command Prompt` as Administrator
	1. Press the Windows key.
	2. Type `cmd`
	3. Under "Best Match", _right-click_ `Command Prompt`
	4. Click `Run as Administrator` ![[CMDasAdmin.png]]
	5.  If prompted, click `Yes` in the `Do you want to allow this app to make changes to your device?` pop-up ![[AllowChangesPopup.png]]
3.  Generate OpenSSH key
	1.  Generate the key
		1. If you are not instructed to specify a "Key size in bits" (usually 4096, if someone requests it)  enter `ssh-keygen` ![[keygenimage.png]]
		2. Otherwise, enter `ssh-keygen -b 4096` or whatever key size was requested.
	2. Stick to the default options, and press Enter. The system will save the keys to `C:\Users\{your_username}\.ssh\id_rsa`
	3.  You’ll be asked to enter a passphrase. Hit Enter to skip this step or enter your passphrase. Passphrase is optional; however, a minimum of 8 characters is required.
		- If you enter a passphrase, do not forget it. The key will be useless without it.
	4. The system will generate the key pair, and display the key fingerprint and a randomart image. ![[randomart.png]]
	5. Open the file browser and navigate to `C:\Users\{your_username}\.ssh
	6. There should be 2 files ![[SSHkeyfiles.png]]
		1. `id_rsa` is the _private key_ 
		2. `id_rsa.pub` is the _public key_

#### Mac OSX (and generally, Linux)

1.  Open a command terminal.
	2.  At the prompt, enter the following command: `ls -l ~/.ssh/id_rsa.pub`
		1.  If the file exists, you already have an ssh key
		2.  if not, then proceed to generate one
2.  At the prompt,
	1.  If you were instructed to use a specific key size, enter  `ssh-keygen -b {keysize} -P "{your_passphrase}"` 
	2.  If you were not instructed to use a specific key size, enter `ssh-keygen -P "{your_passphrase}"` 
	3.  Your passphrase can be set as `-P ""` (an empty string) if you do not want to use a passphrase
3.  The file will be generated in `~/.ssh` ![[generatedsshkeyosx.png]]

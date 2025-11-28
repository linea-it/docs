## How to create an SSH key pair

To access our environment via SSH, you need to generate a key pair following the steps below:


### Linux

a) To generate the key pair, use the following command in your terminal.

    ssh-keygen -t rsa -b 4096
    
    Generating public/private rsa key pair.
    Enter file in which to save the key (/root/.ssh/id_rsa):  [press ENTER]
    Created directory '/root/.ssh'.
    Enter passphrase (empty for no passphrase): [enter a password and press ENTER to confirm]
    Enter same passphrase again: [repeat password and press ENTER]

![Image](../images/retorno_da_criacao_de_chave_ssh.png)

b) After receiving the message that the key was generated, you can view the two created files by listing the directory contents `ls $HOME/.ssh id_rsa  id_rsa.pub`

c) Once the keys are generated, send the **.pub** key to the IT team via email at [helpdesk@linea.org.br](mailto:helpdesk@linea.org.br). The LIneA IT team will configure the key on the server and respond with login instructions for the *Apollo* cluster. ***Please wait for confirmation***.

### Windows

To generate a key pair on the Windows operating system:

a) Download and install the *PuTTY* application.

b) Access the folder where the program was installed (this installation was performed on Windows 10) `C:\Program File\PuTTY` (the path may vary depending on the operating system). Open the *PuTTYgen* program.

![Image](../images/caminho_do_programa_puttygem.png)

c) Click *Generate* (keep the key type as **RSA**).

![Image](../images/gerando_chave_no_windons.png)

**NOTE: Moving the mouse pointer helps generate the key more quickly, as it generates random bits**.

![Image](../images/carregamento_da_criacao_da_chave.png)

d) Key pair successfully generated.

 - Copy the public key to be saved on the server (highlighted in yellow in the image);
 - Set a password for the public key (highlighted in blue in the image);
 - After copying, save the public and private keys on your computer (highlighted in red in the image) and send the `.pub` key to the IT team via email at [helpdesk@linea.org.br](mailto:helpdesk@linea.org.br). The LIneA IT team will configure the key on the server. ***Please wait for confirmation***.

![Image](../images/copiando_chave.png)
    
e) After receiving the confirmation email that the `.pub` key was registered on the access server, configure the *PuTTY* program.
    
 - Create a desktop shortcut and open *PuTTY*;
 - Enter the Hostname: login.linea.org.br.

![Image](../images/tela_do_putty.png)

f) On the left side, go to the option `SSH > Auth (highlighted in blue) > click Browse (highlighted in yellow) and select the key file with .ppk extension`.

![Image](../images/configurando_aquivo_ppk.png)

h) If you need to use a tunnel, perform the following configuration.    
**NOTE: tunnels are configured according to what the user needs to access**

- Go to the *Tunnels* option (left side);
- In *Source port*, enter the port number;
- *Destination* > enter the destination address > *Add*.

![Image](../images/configurando_tunel_no_putty.png)

Return to the left side and go to the first menu option `Session (in red), enter the session name (in yellow) and click Save (in blue)`. To access, click `Open`.

![Image](../images/logando_no_ambiente_putty.png)

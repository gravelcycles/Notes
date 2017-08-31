# Help with easily accessing the lab computers.

If you are connecting to the department of computer science computer lab from ssh on your laptop, you should, if all possible, use public key authentication rather than a password authentication.

Public key authentication is a technique that works through a key pair: a public key and a private key. The pair are related strongly, although the private key cannot be computed (practically speaking) from the public key. The public key can be used, however, in a protocol that convinces the public key holder that the counter-party of the protocol knows the private key while releasing minimal information about the private key.

If you want to learn more, Dr. Rosenberg has more on the topic here: http://blog.cs.miami.edu/burt/2015/02/02/ssh-to-the-lab-machines/


## How to use public key encryption on the lab computers

1. On your machine, change directories to your machine's hidden SSH folder: `cd ~/.ssh`. We will need to modify these files slightly.
2. In your .ssh folder, generate a public-private key pair by running the following command: `ssh-keygen`. You will need to name the file (I would recommend just `id_rsa`) and provide a password (I would recommend leaving the password blank).
3. In your .ssh folder, open the file "config" with your editor of choice. You will need to add a few things:

    ```
    Host lee

    HostName lee.cs.miami.edu

    User <your_username>

    IndentityFile <the_name_of_your_ssh_key>
    ```

    If you'd like to SSH without a password onto any other lab computer, add the following for each machine you want to log onto:

    ```
    Host <your_nickname_for_the_host_machine>

    ProxyCommand ssh -o StrictHostKeyChecking=no lee nc %h 22

    User <your_username>

    IndentityFile <the_name_of_your_ssh_key>
    ```

    The "ProxyCommand" line lets you hop from Lee to your host of choice.

4. Now, you need to copy your public key onto **_each_** of the hosts that you want to access. Here's how:

    ```
    scp <the_name_of_your_ssh_key>.pub <hostname>:.ssh/authorized_keys
    ```

    For example, if I (username dwgr322) want to copy my public key onto lee, I would do the following: 
    
    `scp id_rsa.pub lee:.ssh/authorized_keys`. 

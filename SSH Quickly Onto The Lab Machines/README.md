# How to easily log into the lab computers.

If you use the lab machines a lot, it quickly becomes a pain to type `ssh usrname@lee.cs.miami.edu` and then type in your password over and over again. This becomes especially painful if you want to (1) have multiple shells open at once (2) go from Lee to another lab machine. Public key authentication allows you to shortcut this process: if set up correctly, you can log into any lab machine quickly without remembering your password. For example, I can log into the Boston host with just `ssh boston`. Follow the steps below to learn how.

Public key authentication is a technique that works through a key pair: a public key and a private key. By giving the lab machines a public key that is linked to a private key that you hold, the lab machines will securely know that it is you. 

If you want to learn more about how private key authentication works, Dr. Rosenberg has more on the topic here: http://blog.cs.miami.edu/burt/2015/02/02/ssh-to-the-lab-machines/


## Let's get you started

1. On your machine, change directories to your machine's hidden SSH folder: `cd ~/.ssh`. We will need to modify these files slightly.
2. In your .ssh folder, generate a public-private key pair by running the following command: `ssh-keygen`. You will need to name the file (I would recommend just `id_rsa`) and provide a password (I would recommend leaving the password blank).
3. In your .ssh folder, open the file `config` with your editor of choice. You will need to add a few things:

    ```
    Host lee

    HostName lee.cs.miami.edu

    User <your_username>

    IdentityFile <the_name_of_your_ssh_key>
    ```

    If you'd like to SSH without a password onto any other lab computer, add the following to your `config` file for each machine you want to log onto:

    ```
    Host <host_name>

    ProxyCommand ssh -o StrictHostKeyChecking=no lee nc %h 22

    User <your_username>

    IdentityFile <the_name_of_your_ssh_key>
    ```

    The "ProxyCommand" line lets you hop from Lee to your host of choice.

4. Now, you need to copy your public key onto **_each_** of the hosts that you want to access. Here's how:

    ```
    scp <the_name_of_your_ssh_key>.pub <hostname>:.ssh/authorized_keys
    ```

    For example, if I (username dwgr322) want to copy my public key onto lee, I would do the following: 
    
    ```
    scp id_rsa.pub lee:.ssh/authorized_keys
    ```
    
5. Now, you should be able to log onto any lab computer that you have (1) added your public key to its `authorized_key` file and (2) added the necessary code to your `config` file. All you need to log onto `lee` now is the following: `ssh lee`.

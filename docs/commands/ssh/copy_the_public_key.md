# Copy the Public Key to Server

[Fuente](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1804)

The quickest way to copy your public key to the Ubuntu host is to use a utility called ```ssh-copy-id```. Due to its simplicity, this method is highly recommended if available. If you do not have ```ssh-copy-id``` available to you on your client machine, you may use one of the two alternate methods provided in this section (copying via password-based SSH, or manually copying the key).

## Copying Public Key Using ssh-copy-id

The ```ssh-copy-id``` tool is included by default in many operating systems, so you may have it available on your local system. For this method to work, you must already have password-based SSH access to your server.

To use the utility, you simply need to specify the remote host that you would like to connect to and the user account that you have password SSH access to. This is the account to which your public SSH key will be copied.

The syntax is:

```bash
ssh-copy-id username@remote_host
```

You may see the following message:

```bash
Output
The authenticity of host '203.0.113.1 (203.0.113.1)' can't be established.
ECDSA key fingerprint is fd:fd:d4:f9:77:fe:73:84:e1:55:00:ad:d6:6d:22:fe.
Are you sure you want to continue connecting (yes/no)? yes
```

This means that your local computer does not recognize the remote host. This will happen the first time you connect to a new host. Type “yes” and press ```ENTER``` to continue.

Next, the utility will scan your local account for the ```id_rsa.pub``` key that we created earlier. When it finds the key, it will prompt you for the password of the remote user’s account:

Type in the password (your typing will not be displayed for security purposes) and press ```ENTER```. The utility will connect to the account on the remote host using the password you provided. It will then copy the contents of your ```~/.ssh/id_rsa.pub``` key into a file in the remote account’s home ```~/.ssh``` directory called ```authorized_keys```.

You should see the following output:

```bash
Output
Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'username@203.0.113.1'"
and check to make sure that only the key(s) you wanted were added.
```

## Copying Public Key Using SSH

If you do not have ```ssh-copy-id``` available, but you have password-based SSH access to an account on your server, you can upload your keys using a conventional SSH method.

We can do this by using the ```cat``` command to read the contents of the public SSH key on our local computer and piping that through an SSH connection to the remote server.

On the other side, we can make sure that the ```~/.ssh``` directory exists and has the correct permissions under the account we’re using.

We can then output the content we piped over into a file called ```authorized_keys``` within this directory. We’ll use the ```>>``` redirect symbol to append the content instead of overwriting it. This will let us add keys without destroying previously added keys.

The full command looks like this:

```bash
cat ~/.ssh/id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && chmod -R go= ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

You may see the following message:

```bash
Output
The authenticity of host '203.0.113.1 (203.0.113.1)' can't be established.
ECDSA key fingerprint is fd:fd:d4:f9:77:fe:73:84:e1:55:00:ad:d6:6d:22:fe.
Are you sure you want to continue connecting (yes/no)? yes
```

This means that your local computer does not recognize the remote host. This will happen the first time you connect to a new host. Type “yes” and press ```ENTER``` to continue.

Afterwards, you should be prompted to enter the remote user account password:

```bash
Output
username@203.0.113.1's password:
```

After entering your password, the content of your ```id_rsa.pub``` key will be copied to the end of the ```authorized_keys``` file of the remote user’s account. Continue on to Step 3 if this was successful.

## Copying Public Key Manually

If you do not have password-based SSH access to your server available, you will have to complete the above process manually.

We will manually append the content of your ```id_rsa.pub``` file to the ```~/.ssh/authorized_keys``` file on your remote machine.

To display the content of your ```id_rsa.pub``` key, type this into your local computer:

```bash
cat ~/.ssh/id_rsa.pub
```

You will see the key’s content, which should look something like this:

```bash
Output
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCqql6MzstZYh1TmWWv11q5O3pISj2ZFl9HgH1JLknLLx44+tXfJ7mIrKNxOOwxIxvcBF8PXSYvobFYEZjGIVCEAjrUzLiIxbyCoxVyle7Q+bqgZ8SeeM8wzytsY+dVGcBxF6N4JS+zVk5eMcV385gG3Y6ON3EG112n6d+SMXY0OEBIcO6x+PnUSGHrSgpBgX7Ks1r7xqFa7heJLLt2wWwkARptX7udSq05paBhcpB0pHtA1Rfz3K2B+ZVIpSDfki9UVKzT8JUmwW6NNzSgxUfQHGwnW7kj4jp4AT0VZk3ADw497M2G/12N0PPB5CnhHf7ovgy6nL1ikrygTKRFmNZISvAcywB9GVqNAVE+ZHDSCuURNsAInVzgYo9xgJDW8wUw2o8U77+xiFxgI5QSZX3Iq7YLMgeksaO4rBJEa54k8m5wEiEE1nUhLuJ0X/vh2xPff6SQ1BL/zkOhvJCACK6Vb15mDOeCSq54Cr7kvS46itMosi/uS66+PujOO+xt/2FWYepz6ZlN70bRly57Q06J+ZJoc9FfBCbCyYH7U/ASsmY095ywPsBo1XQ9PqhnN1/YOorJ068foQDNVpm146mUpILVxmq41Cj55YKHEazXGsdBIbXWhcrRf4G2fJLRcGUr9q8/lERo9oxRm5JFX6TCmj6kmiFqv+Ow9gI0x8GvaQ== demo@test
```

Access your remote host using whichever method you have available.

Once you have access to your account on the remote server, you should make sure the ~/.ssh directory exists. This command will create the directory if necessary, or do nothing if it already exists:

```bash
mkdir -p ~/.ssh
```

Now, you can create or modify the ```authorized_keys``` file within this directory. You can add the contents of your ```id_rsa.pub``` file to the end of the ```authorized_keys``` file, creating it if necessary, using this command:

```bash
echo public_key_string >> ~/.ssh/authorized_keys
```

In the above command, substitute the ```public_key_string``` with the output from the ```cat ~/.ssh/id_rsa.pub``` command that you executed on your local system. It should start with ```ssh-rsa AAAA....```

Finally, we’ll ensure that the ```~/.ssh``` directory and ```authorized_keys``` file have the appropriate permissions set:

```bash
chmod -R go= ~/.ssh
```

This recursively removes all “group” and “other” permissions for the ```~/.ssh/``` directory.

If you’re using the ```root``` account to set up keys for a user account, it’s also important that the ```~/.ssh``` directory belongs to the user and not to ```root```:

```bash
chown -R sammy:sammy ~/.ssh
```

In this tutorial our user is named ```sammy``` but you should substitute the appropriate username into the above command.

We can now attempt passwordless authentication with our Ubuntu server.

# Security

## Creating an SSH Key and adding it to a DigitalOcean droplet
1. From your local terminal run the following command: `ssh-keygen -t rsa -b 4096 -C <your.email@address.com>`. This will prompt you with two different questions: you can leave the file to save the key to as default (`/home/<YOURUSER>/.ssh/id_rsa`), and I would highly suggest you created a passphrase, but it is up to you.
2. [Log in to DigitalOcean](https://cloud.digitalocean.com/login), go to Settings -> Security and add the newly created ssh-key, by copying the PUBLIC key (by default under `/home/<YOURUSER>/.ssh/id_rsa.pub`, note the file extension) and pasting it in the required field.
3. [Create your own user on the droplet, with the necessary rights](https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/) (with the help of a sudo user)
4. Use `ssh-copy-id <YOUR_USERNAME>@<DROPLET_IP_ADDRESS>` to copy your newly created ssh-key onto the droplet. (This will prompt you for your user password)


---
title: Setting Up Your Environment
slug: /environment
---

This helps walk you through how to set things up on azure, we recommend this wonderful [guide](https://mirror.xyz/ladislaus.eth/joTqwZ1sBLxlJayV4pIYxCkwl4RWheM_xipU_OCp9MM) if you're running from home.

# Setting up an SGX instance on Azure

Secure-Signer has been developed and tested on Microsoft Azure. The goal of this guide is to expedite setting up your own Azure server. Stay tuned for more information on setting up Secure-Signer at home or on other cloud providers.

![azure1](/img/azure_1.png)

Select `Create a resource`.

---

![azure2](/img/azure_2.png)

Select `Create` to start setting up a VM.

---

![azure3](/img/azure_3.png)

Ensure that the Image is set to `Ubuntu Server 20.04 LTS -x64 Gen2`.

---

![azure4](/img/azure_4.png)

Select a VM size from the `DC-Series`. The smallest (and cheapest) VM with 4 GiB of RAM is sufficient to run Secure-Signer; however, it is recommended to use a larger VM if you plan to run the execution client and consensus client on this server. For this demo, we will use Secure-Signer as a truly remote-signing tool and choose the 4 GiB VM (our execution client and consensus client will be run externally on a different server).

---

![azure5](/img/azure_5.png)

Set SSH credentials according to your needs. In this case we will generate a new SSH private key called `Secure-Signer-key` that we can use to sign into this server.

---

![azure6](/img/azure_6.png)

Add any additional configurations that fit your needs. Then select `Review and Create`. Verify the OS image and VM size are correct. Then `Create` the VM.

---

![azure7](/img/azure_7.png)

Securely store the SSH private key, for example in the `~/.ssh/` directory.

---

![azure8](/img/azure_8.png)

Once deployment finishes, select `Go to resource`.

---

### Increasing VM disk space

> This is optional if Secure-Signer is being used as a standalone remote-signing tool. If you plan to run the consensus client and execution client on this same VM, we recommend following these steps to increase the size of the disk.

![azure9](/img/azure_9.png)

To adjust the disk size, first stop the VM.

![azure10](/img/azure_10.png)

Select `Stop`

---

![azure11](/img/azure_11.png)

Confirm `Yes` and wait for confirmation that is has been stopped.

---

![azure12](/img/azure_12.png)

Navigate to the `Disks` tabs on under `Settings` on the left-hand side.

---

![azure13](/img/azure_13.png)

Select your `Disk name` in this case `Secure-Signer_OsDisk_1_04e04e...`

---

![azure14](/img/azure_14.png)

Select `Size + performance` under `Settings` on the left-hand side.

---

![azure15](/img/azure_15.png)

Select a disk size and click `Save`. In this case, a 1024 GiB disk is sufficient to store the required state for the execution client.

---

![azure16](/img/azure_16.png)

![azure17](/img/azure_17.png)
Navigate back to the overview page and `Start` the VM again.

---

In a terminal run the following, substituting your server’s IP and the path to your SSH private key:

> `ssh -i <path_to_ssh_key> ss@XX.XX.XXX.XXX`

![azure18](/img/azure_18.png)

Congratulations! At this point you are ready to proceed to the [next section](../installation) to install the dependencies for Secure-Signer.

### SSH Tunneling with the Secure-Signer server

It’s possible to run Secure-Signer on the remote Azure server while running your consensus client and execution client locally. Assuming Secure-Signer is installed following the [these steps](install-prereqs) and it is running on port 9003, you can SSH tunnel by running:
`ssh -i <path_to_ssh_key> -N -L 9003:localhost:9003 ss@XX.XX.XXX.XXX`

# jenkins-slave-node-via-SSH

Setting Up a Jenkins Slave Node via SSH

This document provides a step-by-step guide to configure a Jenkins slave node using the Docker image jenkins/ssh-agent and connect it to a Jenkins master node via SSH. By following these instructions, you will successfully integrate a slave node with your Jenkins master.

Prerequisites

Jenkins Master: A running Jenkins master instance.

Docker: Installed on the machine intended for the slave node.

SSH Key Pair: Public and private keys generated for secure SSH communication.

Steps to Configure the Slave Node

1. Pull the Docker Image

On the machine where the slave node will run, pull the jenkins/ssh-agent Docker image:

docker pull jenkins/ssh-agent

For more details about the image, refer to its Docker Hub page.

2. Generate SSH Keys

Generate an SSH key pair for secure communication between the Jenkins master and the slave node. Run the following command:

ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa

This creates a private key (id_rsa) and a public key (id_rsa.pub) in the .ssh directory of your home folder.

3. Run the Slave Node Container

Start the Docker container for the slave node using the jenkins/ssh-agent image. Include the public key in the container’s environment variable JENKINS_AGENT_SSH_PUBKEY:

docker run -d --name agent1 -e JENKINS_AGENT_SSH_PUBKEY="$(cat ~/.ssh/id_rsa.pub)" jenkins/ssh-agent

This command:

Runs the container in detached mode (-d).

Names the container agent1.

Sets the JENKINS_AGENT_SSH_PUBKEY environment variable with the contents of your public key.

The slave node container is now running and ready for SSH connections.

Steps to Configure the Master Node

1. Access the Jenkins Dashboard

Log in to your Jenkins master instance.

Navigate to Manage Jenkins > Nodes and Clouds.

Click New Node.

2. Configure the Node

Node Name: Provide a unique name for the node.

Type: Choose Permanent Agent and click OK.

Remote Root Directory: Set it to /home/jenkins.

Labels: Add relevant labels to identify the node.

3. Configure the Launch Method

Under Launch Method, select Launch Agents via SSH.

Enter the Host IP address of the slave node (e.g., 172.17.0.2).

4. Add SSH Credentials

Under Credentials, click Add > Jenkins.

Choose Kind: SSH Username with Private Key.

Fill out the fields:

ID: Set an identifier for the credentials (e.g., ssh).

Username: Use jenkins (the default username for the jenkins/ssh-agent container).

Private Key: Select Enter Directly and paste the private key.

To retrieve the private key, run:

cat ~/.ssh/id_rsa

Passphrase: If your private key has a passphrase, enter it here.

Click Add.

5. Save the Node Configuration

Click Save to finish configuring the node.

Verify the Node Status

After saving, the newly added slave node should appear in the Nodes section of Jenkins. Its status should show as Online. If it doesn’t, ensure the following:

The IP address and credentials are correct.

The SSH service is running on the slave node.

The jenkins user has the appropriate permissions.

Conclusion

By following these steps, you have successfully configured a Jenkins slave node using the jenkins/ssh-agent Docker image and connected it to your Jenkins master. This setup enables distributed builds and enhances the scalability of your CI/CD pipeline.

For further assistance, refer to the Jenkins Documentation or the Docker Hub page for the jenkins/ssh-agent image.


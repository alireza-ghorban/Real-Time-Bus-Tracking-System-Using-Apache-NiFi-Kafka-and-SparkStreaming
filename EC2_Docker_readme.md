# EC2 and Docker Setup

This guide outlines the steps to set up an AWS EC2 instance and install Docker. The EC2 instance will serve as the foundational infrastructure for running the various components of this data engineering project.

1. **Create a New AWS EC2 Instance**:
   - Choose the instance type and key pair. Opt for t3.xlarge or t3.2xlarge for better performance.
   - Allocate ample storage (e.g., at least 8 GB) as this instance will collect data from the API.
2. **SSH into the EC2 Instance**:
`ssh -i ~/.ssh/your_pem_file.pem ec2-user@'<your_EC2_external_IP>'`

3. **Install Docker on EC2**:
`sudo yum install docker -y`

4. **Start Docker Services**:

```bash
sudo service docker start
sudo systemctl enable docker
sudo usermod -a -G docker ec2-user
```

5. **Exit and Reconnect to EC2**:
After configuring Docker, exit and reconnect to the EC2 instance to apply the changes.

`ssh -i ~/.ssh/your_pem_file.pem ec2-user@'<your_EC2_external_IP>'`

6. **Verify Docker Installation**:

```bash
docker --version
sudo service docker status
```

If you encounter any issues during the installation, refer to the [Docker Installation Guide](https://docs.docker.com/engine/install/) for troubleshooting tips and detailed installation instructions.
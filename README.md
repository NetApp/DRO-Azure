# NetApp Disaster Recovery Orchestrator (DRO)

NetApp’s DRO provides an ideal solution for customers who need a flexible solution for easy disaster recovery including a zero-compute footprint approach; it offers the benefits of a proven and trusted DR platform with the scale and flexibility of the public cloud. 

DRO leverages NetApp's SnapMirror replication-based approach for a powerful and economical solution for protecting data and applications running on VMware environments on Microsoft Azure (Azure VMware Solution) integration with Azure NetApp Files (ANF).

# DRO Installation Steps

## Pre-requisites

- Following packages must be installed on the host machine (The script will install it if not already installed):
  - docker
  - docker-compose
  - jq
  - openssl
-	Connectivity to SRC and DST site vCenter and Storage systems
-	DNS resolution in place if using DNS names in place of IPs for vCenter/Storage systems
-	User with root permissions

**Note:** Recommended host OS: Ubuntu 20.04 (2 CPU and 4GB Memory)

## Installation Steps

1. Download the installation package on the designated virtual machine:

``` git clone https://github.com/NetApp/DRO-Azure.git ```

2. Unzip the package and navigate into the unziped folder:

``` cd DRO-Azure ```

``` tar -xf DRO-prereq.tar ```

``` cd dro_package ```

3. Run the deployment script and enter your host IP (for example: 10.10.10.10):

``` sudo sh deploy_azure.sh ```

4. Once the script runs successfully, access the UI using below credentials:

**Username:** admin
**Password:** admin

## Minor Version Update

1. Download the installation package on the designated virtual machine:

``` git clone https://github.com/NetApp/DRO-Azure.git ```

2. Unzip the package and navigate into the unziped folder:

``` cd DRO-Azure ```

``` tar -xf DRO-prereq.tar ```

``` cd dro_package ```

3. Run the update script and enter your host IP (for example: 10.10.10.10):

``` sudo sh update_azure.sh ```

4. Once the script runs successfully, access the UI using below credentials:

**Username:** admin
**Password:** admin

## Troubleshooting Steps

1. If you encounter an error during initial installation, run the uninstallation script before retrying the installation again to cleanup any residual data. 
    
    ``` sudo sh uninstall.sh ```
  
2. If you encounter any issue post installation, please run the troubleshoot script and reach out to support from the UI.

    ``` sudo sh troubleshoot.sh ```

## Additional Connector Deployment Steps

The DRO Setup comes with a default connector installed on the VM where the installation script is run. In case you want to install an additional connector on a separate VM, follow the below steps on the VM where you want to deploy the connector:

1. Download the installation package on the designated virtual machine:
   
``` git clone https://github.com/NetApp/DRO-Azure.git ```

2. Unzip the package and navigate into the unziped folder:
   
``` cd DRO-Azure ```

``` tar -xf additional-connector-installation.tar ```

``` cd dro_package ```

3. Run the deployment script and enter the host IP for the primary host where DRO is installed and the secondary host where the connector is being installed (for example: 10.10.10.10):

``` sudo sh connector_install.sh ```

4. In the DRO UI, Click on the Settings button on the top right > Manage Connectors and Add the new connector to start using it.


## Verify Package

1. Download the installation package on the designated virtual machine:
   
``` git clone https://github.com/NetApp/DRO-Azure.git ```

2. Unzip the package and navigate into the unziped folder:
   
``` cd DRO-Azure ```

``` tar -xf signature.tar```

``` cd DRO-signature ```

3. Extract the public key from the pem file using below command:

``` openssl x509 -pubkey -noout -in csc-prod-SIE-DRO.pem > SIE-DRO-public.pub ```

4. Verify each tar file using the public key generated in the previous step:

``` openssl dgst -sha256 -verify SIE-DRO-public.pub -signature DRO-prereq.tar.sig ../DRO-prereq.tar ```

``` openssl dgst -sha256 -verify SIE-DRO-public.pub -signature additional-connector-installation.tar.sig ../additional-connector-installation.tar ```

5. To verify the code signing certificate against the chain certificate:

``` openssl verify -CAfile csc-prod-chain-SIE-DRO.pem csc-prod-SIE-DRO.pem ```


## Points to Note

1. During Failover operation, users can failover using the latest snapshot only. However, during Test Failover operation, users can select any older snapshot available.

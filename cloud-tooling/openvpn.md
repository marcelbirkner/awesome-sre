---
description: OpenVPN is a virtual private network (VPN)
---

# OpenVPN

OpenVPN is a virtual private network (VPN) system that implements techniques to create secure point-to-point or site-to-site connections in routed or bridged configurations and remote access facilities. It implements both client and server applications.

## Infrastructure Overview

The following diagram shows a typical setup for a SAAS environment that is using OpenVPN. All critical systems run in private subnets. Only the OpenVPN bastion host is accessible from the public Internet. Thats how SRE / OPS / DEVS can connect to the environment using secure VPN clients (like [Tunnelblick on MacOS](https://tunnelblick.net/downloads.html) or [openvpn client on Linux](https://openvpn.net/cloud-docs/openvpn-3-client-for-linux/))

1. Setup **VPC**
2. Setup **Public Subnet**
   1. used for anything that needs to be accessible from the public internet, i.e. a OpenVPN bastion host, loadbalancer, ...
3. Setup **Private Subnet**
   1. thats where critical systems get installed, i.e. datastores, kubernetes cluster, ...
4. Setup **NAT Gateway**
   1. NAT Gateway will be used by all hosts that need to talk to the public internet
5. Setup **OpenVPN Bastion** host
   1. This server is only accessible from the public Internet via port **1194**

![](<../.gitbook/assets/image (2).png>)

## OpenVPN with 2FA

For additional level of security OpenVPN can be configured with 2-Factor-Authentication

1. Users installs DUO App on Mobile Phone
2. Once a user has authenticated using user/password, openvpn makes a request to duo.com to push a message to the Mobile Phone for the user. This process can be seen in the second diagram

![](<../.gitbook/assets/image (3).png>)

1. Open VPN connection initiated
2. Primary authentication
3. Open VPN connection established to Duo Security over TCP port 443
4. Secondary authentication via Duo Security’s service
5. Open VPN receives authentication response
6. Open VPN session logged in

Source: [https://duo.com/docs/openvpn](https://duo.com/docs/openvpn)

## Setup OpenVPN

### Step 1. Generate the CA key

```
openssl genrsa 4096 > vpn-ca-key.pem
```

### Step 2. Using the CA key, generate the CA certificate (10 years)

```
openssl req -new -x509 -nodes -days 365000 -key vpn-ca-key.pem -out vpn-ca.pem
openssl x509 -noout -text -in vpn-ca.pem
```

### Step 3: Generate Intermediate CA key

```
openssl genrsa 4096 > intermediate.cakey.pem
```

### Step 4: Create intermediate CA Certificate Signing Request (CSR)

```
openssl req -new -sha256 -config intermediate/openssl.cnf -key intermediate/private/intermediate.cakey.pem -out intermediate/csr/intermediate.csr.pem
```

### Step 5: Sign and generate intermediate CA certificate

```
openssl ca -config openssl.cnf -extensions v3_intermediate_ca -days 2650 -notext -batch -in intermediate/csr/intermediate.csr.pem -out intermediate/certs/intermediate.cacert.pem
```

### Links

https://www.golinuxcloud.com/openssl-create-certificate-chain-linux/

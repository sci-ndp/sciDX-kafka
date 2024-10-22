# sciDX-kafka

# Kafka SSL/TLS Setup Guide

This document outlines the steps needed to configure SSL/TLS for a ZooKeeper instance and multiple Kafka brokers (`broker0`, `broker1`, `broker2`). It includes generating a Certificate Authority (CA), creating truststores and keystores, and signing certificates for secure communication.

## ZooKeeper SSL Configuration

### Step 1: Generate Certificate Authority (CA)
Generate a self-signed Certificate Authority (CA) that will be used to sign the certificates for ZooKeeper and Kafka brokers.

```sh
openssl req -new -x509 -keyout ca-key -out ca-cert -days 3650
```

### Step 2: Create ZooKeeper Truststore
Create a truststore for ZooKeeper to store the CA certificate.

```sh
keytool -keystore kafka.zookeeper.truststore.jks -alias ca-cert -import -file ca-cert
```

### Step 3: Create ZooKeeper Keystore
Create a keystore for ZooKeeper and generate a key pair for the ZooKeeper server.

```sh
keytool -keystore kafka.zookeeper.keystore.jks -alias zookeeper -validity 3650 -genkey -keyalg RSA -ext SAN=dns:localhost
```

### Step 4: Create Certificate Signing Request (CSR) for ZooKeeper
Generate a Certificate Signing Request (CSR) for the ZooKeeper keystore.

```sh
keytool -keystore kafka.zookeeper.keystore.jks -alias zookeeper -certreq -file ca-request-zookeeper
```

### Step 5: Sign the CSR for ZooKeeper
Sign the CSR using the CA created in Step 1.

```sh
openssl x509 -req -CA ca-cert -CAkey ca-key -in ca-request-zookeeper -out ca-signed-zookeeper -days 3650 -CAcreateserial
```

### Step 6: Import the CA into ZooKeeper Keystore
Import the CA certificate into the ZooKeeper keystore.

```sh
keytool -keystore kafka.zookeeper.keystore.jks -alias ca-cert -import -file ca-cert
```

### Step 7: Import the Signed Certificate into ZooKeeper Keystore
Import the signed certificate back into the ZooKeeper keystore.

```sh
keytool -keystore kafka.zookeeper.keystore.jks -alias zookeeper -import -file ca-signed-zookeeper
```

## Kafka Broker 0 SSL Configuration

### Step 1: Create Kafka Broker 0 Truststore
Create a truststore for Kafka broker 0 to store the CA certificate.

```sh
keytool -keystore kafka.broker0.truststore.jks -alias ca-cert -import -file ca-cert
```

### Step 2: Create Kafka Broker 0 Keystore
Create a keystore for Kafka broker 0 and generate a key pair.

```sh
keytool -keystore kafka.broker0.keystore.jks -alias broker0 -validity 3650 -genkey -keyalg RSA -ext SAN=dns:localhost,dns:broker0
```

### Step 3: Create Certificate Signing Request (CSR) for Broker 0
Generate a Certificate Signing Request (CSR) for the Kafka broker 0 keystore.

```sh
keytool -keystore kafka.broker0.keystore.jks -alias broker0 -certreq -file ca-request-broker0
```

### Step 4: Sign the CSR for Broker 0
Sign the CSR using the CA created earlier.

```sh
openssl x509 -req -CA ca-cert -CAkey ca-key -in ca-request-broker0 -out ca-signed-broker0 -days 3650 -CAcreateserial
```

### Step 5: Import the CA into Broker 0 Keystore
Import the CA certificate into the Kafka broker 0 keystore.

```sh
keytool -keystore kafka.broker0.keystore.jks -alias ca-cert -import -file ca-cert
```

### Step 6: Import the Signed Certificate into Broker 0 Keystore
Import the signed certificate back into the Kafka broker 0 keystore.

```sh
keytool -keystore kafka.broker0.keystore.jks -alias broker0 -import -file ca-signed-broker0
```

## Kafka Broker 1 SSL Configuration

### Step 1: Create Kafka Broker 1 Truststore
Create a truststore for Kafka broker 1 to store the CA certificate.

```sh
keytool -keystore kafka.broker1.truststore.jks -alias ca-cert -import -file ca-cert
```

### Step 2: Create Kafka Broker 1 Keystore
Create a keystore for Kafka broker 1 and generate a key pair.

```sh
keytool -keystore kafka.broker1.keystore.jks -alias broker1 -validity 3650 -genkey -keyalg RSA -ext SAN=dns:localhost,dns:broker1
```

### Step 3: Create Certificate Signing Request (CSR) for Broker 1
Generate a Certificate Signing Request (CSR) for the Kafka broker 1 keystore.

```sh
keytool -keystore kafka.broker1.keystore.jks -alias broker1 -certreq -file ca-request-broker1
```

### Step 4: Sign the CSR for Broker 1
Sign the CSR using the CA created earlier.

```sh
openssl x509 -req -CA ca-cert -CAkey ca-key -in ca-request-broker1 -out ca-signed-broker1 -days 3650 -CAcreateserial
```

### Step 5: Import the CA into Broker 1 Keystore
Import the CA certificate into the Kafka broker 1 keystore.

```sh
keytool -keystore kafka.broker1.keystore.jks -alias ca-cert -import -file ca-cert
```

### Step 6: Import the Signed Certificate into Broker 1 Keystore
Import the signed certificate back into the Kafka broker 1 keystore.

```sh
keytool -keystore kafka.broker1.keystore.jks -alias broker1 -import -file ca-signed-broker1
```

## Kafka Broker 2 SSL Configuration

### Step 1: Create Kafka Broker 2 Truststore
Create a truststore for Kafka broker 2 to store the CA certificate.

```sh
keytool -keystore kafka.broker2.truststore.jks -alias ca-cert -import -file ca-cert
```

### Step 2: Create Kafka Broker 2 Keystore
Create a keystore for Kafka broker 2 and generate a key pair.

```sh
keytool -keystore kafka.broker2.keystore.jks -alias broker2 -validity 3650 -genkey -keyalg RSA -ext SAN=dns:localhost,dns:broker2
```

### Step 3: Create Certificate Signing Request (CSR) for Broker 2
Generate a Certificate Signing Request (CSR) for the Kafka broker 2 keystore.

```sh
keytool -keystore kafka.broker2.keystore.jks -alias broker2 -certreq -file ca-request-broker2
```

### Step 4: Sign the CSR for Broker 2
Sign the CSR using the CA created earlier.

```sh
openssl x509 -req -CA ca-cert -CAkey ca-key -in ca-request-broker2 -out ca-signed-broker2 -days 3650 -CAcreateserial
```

### Step 5: Import the CA into Broker 2 Keystore
Import the CA certificate into the Kafka broker 2 keystore.

```sh
keytool -keystore kafka.broker2.keystore.jks -alias ca-cert -import -file ca-cert
```

### Step 6: Import the Signed Certificate into Broker 2 Keystore
Import the signed certificate back into the Kafka broker 2 keystore.

```sh
keytool -keystore kafka.broker2.keystore.jks -alias broker2 -import -file ca-signed-broker2
```

---

By following these commands, you will have successfully set up SSL/TLS for your ZooKeeper instance and all three Kafka brokers (`broker0`, `broker1`, `broker2`). Each component will use a certificate signed by the same Certificate Authority, ensuring trusted and secure communication across the entire Kafka cluster.



### Generate PKCS12 keystore
```shell
keytool -genkeypair -alias server -keyalg RSA -keysize 4096 -validity 365 -dname "CN=Server,OU=Server,O=Examples,L=,S=CA,C=U" -keypass server1234 -keystore server.p12 -storeType PKCS12 -storepass server1234
keytool -genkeypair -alias client -keyalg RSA -keysize 4096 -validity 365 -dname "CN=Client,OU=Server,O=Examples,L=,S=CA,C=U" -keypass client1234 -keystore client.p12 -storeType PKCS12 -storepass client1234
```

### Export public keys
```shell
keytool -exportcert -alias client -file client.cer -keystore client.p12 -storepass client1234
keytool -exportcert -alias server -file server.cer -keystore server.p12 -storepass server1234
```

### Import public keys to trust stores.

```shell
keytool -importcert -keystore client-truststore.p12 -alias server-public -file server.cer -storepass client1234 -noprompt
keytool -importcert -keystore server-truststore.p12 -alias client-public -file client.cer -storepass server1234 -noprompt
```

### Inspect keystore

```shell
keytool -list -keystore server-truststore.p12
```

### Call our service ignoring self-signed certificate

```shell
curl -v -k https://localhost:8080/greetings
```
- `-k` ignore self-signed certificate
- `-v` verbose

## Call our service presenting certificate

```shell
curl -v --cert-type P12 --cert client.p12:client1234 https://localhost:8080/greetings
```

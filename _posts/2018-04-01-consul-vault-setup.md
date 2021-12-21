---
layout: post
title: Quick Installation of Vault and Consul
date: 2018-04-01
categories: Open Source
---
I would like to cover a quick and simple setup of Consul and Vault for learning purpose. These steps short circuit a real world advanced setup options.

<div class="divider"></div>

Download the proper package for you OS and the architecture from

Download Consul [https://www.consul.io/downloads.html](https://www.consul.io/downloads.html)
Download Vault [https://www.vaultproject.io/downloads.html](https://www.vaultproject.io/downloads.html)

Consul Setup -
write a shell or batch file with following command E.g. consul_start.bat with following content

## Consul Startup command
> consul agent -ui -server -data-dir mydata -advertise 127.0.0.1 -bootstrap-expect 1

Vault Setup -
write a shell or batch file with following command E.g. consul_start.bat with following content

## consul_start.bat
```shell
> vault server -config=vault-example.hcl
```

## vault-example.hcl content
```javascript
 backend "consul" {
 address="127.0.0.1:8500"
 path="vault"
 }
 listener "tcp" {
  address = "127.0.0.1:8200"
  tls_disable = 1
 }
 disable_mlock = true
```

## Initialize Vault
>vault operator init

Error initializing: Put https://127.0.0.1:8200/v1/sys/init: http: server gave HTTP response to HTTPS client [0m
The default address of the client uses https. Just set the -address parameter or set the VAULT_ADDRESS env var and you'll be good to go.

## Set Vault Address
>set VAULT_ADDR=http://127.0.0.1:8200
vault operator init -key-shares=5 -key-threshold=2

-key-threshold - This defines the number of keys required to unseal vault
-key-shares - This defines number of keys that should be generated during initialization

## Sample Output
[0;0mUnseal Key 1: 45MbjNghzYmFjaagC5Fcx4qj5E2jOl5ZWYByFctXV+T3 [0m
[0;0mUnseal Key 2: Z/TffCSL0mUw4NbMbHpf0dEjGge5iSZja6C7dEoY1AP1 [0m
[0;0mUnseal Key 3: C5YvqiPS20VgtiPPDuqgEwsAt6+qPE0+qO7P8zyqCEJf [0m
[0;0mUnseal Key 4: WLazf5dhHIQUSnOLLi5z29WFaLwD9ap9XASKbgHXXuc0 [0m
[0;0mUnseal Key 5: O1fgO9aT38klIVemz6q5WCXlpQaFS4jjxByaACco9Ga4 [0m
[0;0m [0m
[0;0mInitial Root Token: 4360d91f-cf9d-a33f-45ca-2bdc699caef1 [0m

Make a note of at least two Unseal keys and your root keys
unseal is a one time process as long as your backend does not change.

Now, let us unseal by running this command twice, provide atleast two unique unseal key from the init operation

## Unseal Vault Command
>vault operator unseal

Unseal Key (will be hidden):


Login, by making use of Root token that was generated during init process


## Vault Login Command
>vault login <Root Key>

Now , let us run commands to store and retrieve key/value sercrets

## Vault Command to encrypt and decrypt
>vault.exe write secret/my-app/s3password value=s3qwqwq
vault.exe read secret/my-app/s3password

You may also login from the UI and navigate to view the stored key/values
[http://localhost:8500/ui/](http://localhost:8500/ui/)

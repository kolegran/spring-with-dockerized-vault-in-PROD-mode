## Spring with dockerized Vault in PROD mode:

- Write docker-compose file for starting Vault (you can use my dc file as example or use ```https://hub.docker.com/_/vault```)
- Start dc with ```docker-compose up```
- Go to sh with ```docker exec -it mw_vault sh```. Vault is seal by default, see the next steps
- Run ```vault operator init``` (For details read the next docs: ```https://cloud.spring.io/spring-cloud-vault/reference/html/#_quick_start```)
- Then you got 5 unseal keys & INITIAL ROOT TOKEN (use for your app or generate the new!)
- Take 3 unseal keys and make the same for each key ```vault operator unseal ${ONE_OF_5_KEYS}``` (it's security, babe :D)
- Then ```vault login```. Use INITIAL ROOT TOKEN when Vault asks you for a token 
- Okay, let's write our keys. See Vault secrets list ```vault secrets list -detailed```
- Create kv if you need it ```vault secrets enable -path=secret kv``` (For details see: ```https://learn.hashicorp.com/tutorials/vault/getting-started-secrets-engines```)
- Write your keys ```vault kv put secret/${VAULT_CONFIG_NAME} ${YOUR_API_OR_SMTH_ELSE_NAME}.${KEY_NAME}=${KEY_VALUE}```

##### What about Spring: use bootstrap.properties/yaml
```spring.application.name=${VAULT_CONFIG_NAME}```

```spring.cloud.vault.token={VAULT_TOKEN} (it can be an INITIAL ROOT TOKEN)```

```spring.cloud.vault.scheme=http (it can be https. For more details see: https://cloud.spring.io/spring-cloud-vault/reference/html/#_quick_start)```

```spring.cloud.vault.kv.enabled=true```

##### Note that ```VAULT_DEV_ROOT_TOKEN_ID``` (in docker-compose) is REDUNDANT for PROD mode and will be ignored by Vault during starting.
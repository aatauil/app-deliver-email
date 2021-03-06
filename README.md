# App-deliver-email

Backend app to test the [deliver-email-service](https://github.com/redpencilio/deliver-email-service).

## How to

First clone this repo then open your docker-compose.yml file. By default the deliver-email-service is set to "development".
It is also set to "smtp". 

**Be sure to let the migration service do its work! wait until you see logs from the migration service telling you that the migration is [DONE]**

### Check for migrations

run the following query at ```http://localhost:8890/sparql```

```

PREFIX nmo: <http://www.semanticdesktop.org/ontologies/2007/03/22/nmo#>
PREFIX fni: <http://www.semanticdesktop.org/ontologies/2007/03/22/fni#>
PREFIX nie: <http://www.semanticdesktop.org/ontologies/2007/01/19/nie#>

SELECT ?mailfolders
  WHERE {
    GRAPH <http://mu.semte.ch/graphs/system/email> {
      <http://data.lblod.info/id/mailboxes/1> nie:hasPart ?mailfolders.
    }
  }
    
```

<sup>If the mailfolders column is empty it means that it has not finished yet, if you see 6 folder uri's then you are good to go.</sup>

### SMTP
> If you want to use a temporary testing mailbox where you will receive the emails, then skip this part and go to "TEST"

If you want to use "smtp" as your protocol and send/receive the emails from a specific mailbox then you will have to change the following environment variables <br>
WELL_KNOWN_SERVICE, <br> EMAIL_ADDRESS, <br> EMAIL_PASSWORD, <br>FROM_NAME(optional).
<br>
Do not forget to assign the path in volumes to where the deliver-email-service is cloned so you can develop with live-reload

```yaml
    volumes:
      - /path/to/local/cloned/deliver-email-service/folder/:/app/
```
As soon as you have done that you can simple start the app.

```bash
docker-compose up
```

Inspect the logs and wait for the migrations to happen. After that you should be good to go.

### TEST

For use with a temporary mailbox you can simply change the WELL_KNOWN_SERVICE env to "test". This will go through the same process as any other smtp service expect that for every email, you will receive a preview url where you can find more details about the email.

```yaml
    environment:
      NODE_ENV: "development"
      WELL_KNOWN_SERVICE: "test"
```

**More Detailed information can be found on the [deliver-email-service repo](https://github.com/redpencilio/deliver-email-service)**

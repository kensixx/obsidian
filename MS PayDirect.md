For Ken: **Batch**

Running in dev:

```bash
mvn clean compile && quarkus dev \\
  -D%dev.quarkus.datasource.jdbc.url=jdbc:mysql://appiandev.c0uifvxczuks.ap-southeast-1.rds.amazonaws.com:50305/BLUE \\
  -D%dev.quarkus.datasource.username=appian \\
  -D%dev.quarkus.datasource.password=app1@N123 \\
  -D%dev.iv.key=RTBGRDMwREU2NEQ4N0Q5Mg== \\
  -D%dev.secret.key=RDI4NUVFMTU1RDdGNzg1MDcxODlBOEI1NEU5Nzk0RDc=
```

# PayDirect Request Body

[PayDirect Request Body.json](https://prod-files-secure.s3.us-west-2.amazonaws.com/58a72922-6af8-4d90-bc80-1b0b7d79af60/6111443c-2996-4038-aaf5-562ba961ceee/PayDirect_Request_Body.json)

# Sequence Diagram

[http://172.18.2.24:8103/ubp/TsysPrime_1.0.0.md](http://172.18.2.24:8103/ubp/TsysPrime_1.0.0.md) - POST tsys/prime/v1/cards/groups/inquiry

[http://172.18.2.24:8103/ubp/CardsPrimeCardpro_1.0.0.md](http://172.18.2.24:8103/ubp/CardsPrimeCardpro_1.0.0.md) - POST primepro/v1/cards/inquire

![[Pasted image 20240514135557.png]]
# PayDirect Batch

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/58a72922-6af8-4d90-bc80-1b0b7d79af60/2835aa39-181d-4b47-aac0-e5acedf84ddd/Untitled.png)

**Credit Card Details API** - `ub.ms.client.TsysCardsPrimeProClient`

## Pesonet Processing

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/58a72922-6af8-4d90-bc80-1b0b7d79af60/8299b289-e6d7-4d72-aee5-018157c23552/Untitled.png)

## On-Us Processing

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/58a72922-6af8-4d90-bc80-1b0b7d79af60/712f5b61-ea3d-4d05-bad6-a055db567243/Untitled.png)
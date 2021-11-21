# Webhooks

O recebimento de eventos por webhooks se dá pelo envio POST dos dados para uma determinada URL informada. Todas tentativas são logadas e processadas posteriormente.

## Autenticação
Para que o evento seja devidamente validado quanto a sua origem, um token de autorização pré acordado entre as partes envolvidas (receptor e chamador) deverá ser enviado no header da requisição POST.

## Resposta (Sucesso)
Como resposta ao envio POST dos dados para URL uma resposta é enviada como confirmação do correto recebimento e validade da autorização requerida.

**HTTP Status Code: 201**
```json
{
  "eventId": "8dc6ef55-31ee-41a3-b224-ad480ed219aa",
  "message": "Event delivered",
  "delivered": true
}
```

## Resposta (Falha)
Em caso de falha no recebimento dos dado enviados uma resposta de falha é informada ao chamador.

**HTTP Status Code: 4XX ou 5XX**
```json
{
  "eventId": "8dc6ef55-31ee-41a3-b224-ad480ed219ab",
  "message": "Event NOT delivered. Internal error",
  "delivered": false
}
```

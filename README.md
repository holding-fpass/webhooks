# Webhooks

O recebimento/envio de eventos por webhooks se dá pelo envio POST dos dados para uma determinada URL informada. Todas tentativas são logadas e processadas posteriormente.

## Autenticação
Para que o evento seja devidamente validado quanto a sua origem, um token de autorização pré acordado entre as partes envolvidas (receptor e chamador) deverá ser enviado no header da requisição POST.

```curl
curl 'https://{webhookUrl}' \
  -H 'authorization: b1c32c8e-27f7-438d-849c-35f4c54d35ca' \
  ...
```

## Incoming Webhooks
Os eventos enviados para plataforma FPASS seguem a autenticação definida entre as partes e tem os seguintes resultados das chamadas para URL acordada:

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

## Outgoing Webhooks
Os eventos enviados para plataformas externas seguem a autenticação (Header de Autorização padrão: authorization: {token}) definida entre as partes e tem os seguintes modelos:

### Request
Envio do evento é realizado por meio de um requisição POST feita para uma URL pública e segura (HTTPS) contendo o seguinte corpo na requisição.

* eventId: Identificação única do evento gerado
* eventType: Tipo do evento gerado
* resourceId: Identificação única da entidade no qual o evento gerado se refere
* resourceType: Tipo da entidade
* externalId: Identificação do usuário na plataforma destino
* data: Dados adcionais (e opcionais) do evento gerado
* timestamp: Data da geração do evento em formato ISO 8601

#### Body
```json
{
  "eventId": "c1b55ffa-b3ee-4266-817e-567630e1dac3",
  "eventType": "live.reaction.created",
  "resourceId": "8dc6ef55-31ee-41a3-b224-ad480ed219ab",
  "resourceType": "live",
  "externalId": "{string}",
  "data": {
    "emoji": "❤️"
  }
  "timestamp": "2021-01-01T10:30:05.000Z"
}
```

### Response
Como resposta ao envio da requisição é esperado uma evidencia do correto recebimento da mensagem através do status code 201 (Created).
Para evidenciar do o correto recebimento segue um exemplo possível do corpo da resposta:

```json
{
  "eventId": "8dc6ef55-31ee-41a3-b224-ad480ed219ab",
  "timestamp": "2021-01-01T10:30:05.000Z"
}
```


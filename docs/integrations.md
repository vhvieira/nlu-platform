# Integrações Lifty API #
Para usar as integrações da API Lifty é necessário antes de tudo conseguir uma API de acesso para sua empresa.
Em casos de dúvidas sobre como conseguir a API key entre em contato pelo e-mail: *victor@lifty.com.br*


## Whatsapp API ##
Aqui estão descritos os endpoints para integrações com API do whatsapp.
São operações para ler QRCodes, ativar novos números, pausar, despausar, ou remover sessões.

### Obter status de conexão com whatsapp ###

*URL:*
>https://integration-gateway-prod-8xgj7n6l.uc.gateway.dev/integration/bot/${id}/whatsapp?key=${apiKey}

*Método:*
>GET

*DESCRIÇÃO:*
>Onde id é o token do bot para cada cliente, e são obtidos na URL do dashboard após a criação do mesmo na tela de integrações.
Onde apiKey é a chave de acesso da sua empresa descrita no inicio desse documento.

Esse primeiro endpoint vai retornar o seguinte payload:
```json
{
"statusURL": "<<URL_STATUS>>",
"tokenURL": null
}
```

Onde statusURL é usada para obter o status da conexão com a whatsapp, é preciso fazer um get nessa URL.
Ela simplemente vai retornar o seguinte payload:
```json
{
"result": "CONNECTED"
}
```

Abaixo estão os principais status que a API retorna:
>CONNECTED => A conexão está ativa e não existe problema

>NOT_FOUND => A conexão com o whatsapp ainda não foi configurada/iniciada.

>QRCODE => A conexão está aguardando a leitura do QRCode.

### Conectar um novo whatsapp ao bot ###
*URL:*
>https://integration-gateway-prod-8xgj7n6l.uc.gateway.dev/integration/bot/${id}/startWhatsapp?key=${apiKey}

*Método:*
>GET

*DESCRIÇÃO:*
>Onde id é o token do bot para cada cliente, e são obtidos na URL do dashboard após a criação do mesmo na tela de integrações.
Onde apiKey é a chave de acesso da sua empresa descrita no inicio desse documento.

Essa URL também é um GET porém além de devolver a *statusURL*, *tokenURL* vai apresentar o caminho para a imagem do QRCode e basta exibir essa imagem para ser lida com a câmera do celular.
```json
{
"statusURL": "<<URL_STATUS>>",
"tokenURL": "<<URL_PARA_QRCODE>>"
}
```

*outras opções:*
>https://integration-gateway-prod-8xgj7n6l.uc.gateway.dev/integration/bot/${id}/startWhatsapp?key=${apiKey}&options=c

Na URL acima podemos o parametro options=c no final da URL, este parametro é recomendável pois limpa sessões inativas do banco de dados do robo.

*Dicas:*
>Na UI interna a implementação é a seguinte, a primeira chamada o startWhatsapp e depois fica fazendo o pulling a cada 5s na status URL até o status ser QRCODE, quando é QRCode exibimos a imagem da tokenURL.
Detalhe: não é necessário chamar startWhatsapp mais de uma vez, pois a tokenURL já vem preenchida na primeira resposta, mesmo que o QRCODE ainda não esteja pronto.
A múltipla chamada da startWhatsapp pode iniciar mais de uma conexão em paralelo para o mesmo bot. (Sim, é possível connectar mais de um número no mesmo robo)

*Mais dicas:*
>Detalhe, a própria URL faz o refresh da imagem do QRCode caso o whatsapp web atualize o mesmo, mas existe um timeout de 60 segundos para leitura do QRCode, que caso excedido requer uma nova chamada para startWhatsapp.
Nós fazemos o refresh da imagem a cada 5s, mas para evitar problema de cache do navegador colocamos um timestamp no src da imagem, que vai ficar assim no exemplo:
src="<<URL_PARA_QRCODE>>&timestamp=7023121221"
Esse timestamp não afeta o serviço que retorna a imagem mas evita que o navegador faça um cache local da mesma.

*Como saber que o QRCode foi lido?:*
>Basta fazer um pulling na url de status até o status mudar para CONNECTED. Também usamos o intervalo de 5s.
Na verdade fazemos sempre o pulling do status, e se ainda é QRCODE, aí sim fazemos o refresh da imagem, caso seja CONNECTED então fechamos o popup e mostramos "Conexão ativada".

### Pausar um bot para um numero especifico ###

*URL:*
>https://integration-gateway-prod-8xgj7n6l.uc.gateway.dev/integration/bot/${id}/pause/${numero}?key=${apiKey}

*Método:*
>GET

*DESCRIÇÃO:*
>Onde id é o token do bot para cada cliente, e são obtidos na URL do dashboard após a criação do mesmo na tela de integrações.
Onde número é o número de telefone pra o qual o bot deve parar de responder, no formato DDI + DDD + NUMERO. Ex: 554899991234
Onde apiKey é a chave de acesso da sua empresa descrita no inicio desse documento.

### Resumir um bot para um numero especifico ###

*URL:*
>https://integration-gateway-prod-8xgj7n6l.uc.gateway.dev/integration/bot/${id}/resume/${numero}?key=${apiKey}

*Método:*
>GET

*DESCRIÇÃO:*
>Onde id é o token do bot para cada cliente, e são obtidos na URL do dashboard após a criação do mesmo na tela de integrações.
Onde número é o número de telefone pra o qual o bot deve parar de responder, no formato DDI + DDD + NUMERO. Ex: 554899991234
Onde apiKey é a chave de acesso da sua empresa descrita no inicio desse documento.

### Desconectar um whatsapp do bot ###

*URL:*
>https://integration-gateway-prod-8xgj7n6l.uc.gateway.dev/integration/bot/${id}/stopWhatsapp?key=${apiKey}

*Método:*
>GET

*DESCRIÇÃO:*
>Onde id é o token do bot para cada cliente, e são obtidos na URL do dashboard após a criação do mesmo na tela de integrações.
Onde apiKey é a chave de acesso da sua empresa descrita no inicio desse documento.

*Dicas:*
A leitura do status aqui é desnecessária, pois a morte da sessão é certa sempre que retorna 200.
Caso seja necessário pausar a conexão existem outros métodos que preciso te explicar com calma depois.
# Tags  #
Conversation API has several special tags that can be used for handling different functions.
These tags are divided in three categories and explained here

## Dynamic Input Tags ##
Essas tags são usadas para oferecer uma opção de input dinamico, como por exemplo lisas vindo de modelos mantidos pelo usuário e definido em nível de bot.
Essas tags devem ser usadas dentro do dialogflow como action and parameters.

![INPUT TAGS](./images/inputTags.jpg)


### inputModel ####
Input model cria uma lista dinamica de produtos, ou opções dinamicas para o usuário escolher.
Ele pode ser exibido como uma lista com letras A, B, C em caso de API não oficiais ou como um select em caso de API oficial whatsapp.
A renderização desse componente vai variar de acordo com o conector e o canal utilizado.

**Como usar:**
`#inputModel{0}` onde **0** é o indice do modelo dinamico utilizado.
`#inputModel{0;#desc}` para guardar o label/descrição ao invés do código;
`#inputModel{0;filterField}` para trazer uma lista filtrada por um parametro;
`#inputModel{0;#desc;filterField}` para guardar o label/descrição com a lista filtrada;
O modelo dinamico pode vir de um sistema externo ou pode ser mantido dentro do dashboard.

Os modelos dinamicos são acessados clicando no botão abaixo:
![MODELS_BUTTON](./images/dinamicModels.jpg)

E podem ser facilmente editados em formato de arvore de dados conforme imagem abaixo:
![MODELS_DATA](./images/dinamicModelsData.jpg)

### inputExternalModel ####
Input external model cria uma lista dinamica de produtos, ou opções dinamicas para o usuário escolher.
Ele pode ser exibido como uma lista com letras A, B, C em caso de API não oficiais ou como um select em caso de API oficial whatsapp.
A renderização desse componente vai variar de acordo com o conector e o canal utilizado.

A diferença é que a lista vem de um endpoint externo, que pode ser um CRM ou ERP.

**Como usar:**
`#inputExternalModel-->0;1;2;3;4;5;6--*` onde:
**0** é o endpoint a ser chamado.
**1** é o método HTTP a ser chamado (Suporta apenas GET)
**2** é o caminho do JSON que retorna código.
**3** é o caminho do JSON que retorna label.
**4** é o caminho do JSON que retorna price (opcional).
**5** é o valor do filtro que será aplicado na lista (opcional).
**6** é conteúdo é o indice do modelo dinamico utilizado (opcional -> default 0).
![INPUT_EXTERNAL_MODEL](./images/inputExternalModel.png)

### inputTextToBot ####
InputTextToBot deve ser usado em conjunto com a tag inputTextToBot para salvar dados do modelo selecionado.
Ele envia os valores para o motor NLU para ser armazenado em contextos / variáveis.

**Como usar:**
`#inputTextToBot{#code}` Envia o código do último dynamic model selecionado para o NLU Engine.
`#inputTextToBot{#desc}` Envia a descrição do último dynamic model selecionado para o NLU Engine.
`#inputTextToBot{#price}` Envia o preço do último dynamic model selecionado para o NLU Engine.

Obs: Essa tag deve sempre ser usada após um #inputModel ou #inputExternalModel
![INPUT_TEXT_TO_BOT](./images/inputTextToBot.png)

### inputActionModel ####
Assim com em Input model essa tag cria uma lista dinamica de produtos, ou opções dinamicas para o usuário escolher.
A diferença é que a lista vem de outra intent do Dialogflow, e o fluxo chama essa intent para obter o modelo.
Isso permitir fazer integrações com sistemas externos para obter a lista de modelos dinamicos, ou mesmo simplesmente manter em uma intent diferente.
**Como usar:**
`#inputActionModel{ACTION_1}` onde **ACTION_1** é o nome da ação a ser chamada (nome do evento da mesma).
O modelo dinamico deve retorna em um formato específico de JSON.

Os modelos dinamicos formato de JSON esperado:
```json
{
            "key": "0",
            "data": {
                "name": "Lista especialidades",
                "type": "folder"
            },
            "children": [
                {
                    "key": "0-1",
                    "data": {
                        "name": "Clinica geral",
                        "type": "text",
                        "value": {
                            "code": "1",
                            "name": "Clinico geral"
                        }
                    }
                },
                {
                    "key": "0-2",
                    "data": {
                        "name": "Pediatra",
                        "type": "text",
                        "value": {
                            "code": "2",
                            "name": "Pediatra"
                        }
                    }
                }
            ]
        }
```

### inputItem ####
Geralmente utilizada para carrinhos de compras e deve sempre ser utilizada após o inputModel.
Essa tag vai salvar o item no formato JSON para ser enviado para a API de Carrinho de compras.
É importante que o item possua o valor do mesmo no modelo em formato númerico, ou haverá erro de conversão.

![INPUT_ITEM](./images/inputItem.jpg)

**Como usar:**
`#inputItem{0}` onde **0** é o indice da lista de produtos para ser listada

### inputMeetings ###
Geralmente utilizada para cancelar agendamentos para o telefone quando está sendo usada a agenda integrada da plataforma (Google Agenda).
Lista todos os agendamentos para o telefone informado.
![INPUT_MEETINGS](./images/inputMeetings.jpg)

**Como usar:**
`#inputMeetings{data, telefone, modelKey}` onde **data** é a data inicial para buscar os agendamentos, 
e **telefone** é número de telefone que foi utilizado no processo de agendamento
, e **modelKey** é o chave criada para o modelo para ser usada por tags de reply.

Geralmente é combinado com outras tags internas como na imagem abaixo:
![INPUT_MEETINGS_COMBINADO](./images/inputMeetingsCombinado.jpg)

Na imagem acima, o valor de date é enviado ao bot com `#inputTextToBot{hoje}`, setando a variavel date para a data de hoje.
E o telefone é setado para `#inputInternalVariable{conversationRequest.participantId}` pegando o número do whatsapp automaticamente.

Abaixo está um exemplo de como inputMeetings é renderizado no whatsapp utilizando a Cloud API:
![INPUT_MEETINGS_EXAMPLE](./images/inputMeetingsExample.jpg)

### sessionMeetings ####
Essa tag deve ser usada para fluxos de confirmação de consulta onde a integração cria uma sessão.
Para tal `createSessionOnSendingMsg` da coleção bot-integrations deve estar *true*
Também é importante que na mensagem de disparo da integração, quer seja template ou simple message seja informado
os seguintes campos:
`meetingId` que contém o ID do agendamento.
`meetingTitle` que contém a descrição (título) do agendamento.

![SESSION_MEETING](./images/sessionMeeting.jpg)

**Como usar:**
`#sessionMeetings{0}` onde **0** é apenas um identificador interno.

### sessionMeetings ####
Essa tag deve ser usada para fluxos de confirmação de consulta onde a integração cria uma sessão.
Para tal `createSessionOnSendingMsg` da coleção bot-integrations deve estar *true*
Também é importante que na mensagem de disparo da integração, quer seja template ou simple message seja informado
os seguintes campos:
`meetingId` que contém o ID do agendamento.
`meetingTitle` que contém a descrição (título) do agendamento.

![SESSION_MEETING](./images/sessionMeeting.jpg)

**Como usar:**
`#sessionMeetings{0}` onde **0** é a chave do modelo para ser usada em tags de reply.

### inputCode ####
Essa tag deve ser usada para pegar um valor anterior com as tags #sessionMeeetings ou #inputModel.
e buscar o código do valor selecionado e passar para uma variável do dialogflow que pode estar em outra intent.

![INPUT_CODE](./images/inputCode.jpg)

**Como usar:**
`#inputCode{0}` onde **0** é a chave do modelo que foi usada em #sessionMeetings ou #inputMeetings.

#inputCode{
### inputService ###
Semelhante ao inputModel porém com a diferença que deve ser utilizado apenas para escolher o serviço que será agendado.

**Como usar:**
`#inputService{0}` onde **0** é o indice do modelo dinamico utilizado.
O modelo dinamico pode vir de um sistema externo ou pode ser mantido dentro do dashboard.

A grande diferença é que na tela de edição de modelo, o modelo possui uma lista de serviços para aquele template e não produtos, e como serviços estão associados a um tipo de agendamento conforme imagem abaixo:
![INPUT SERVICE](./images/inputService.jpg)

### inputInternalVariable ###
A conversation API possui algumas variaveis internas, e as vezes dentro do fluxo é necessário obter o valor dessas variavéis internas.
Usando essa tag é possível atribuir o valor da variável interna para uma variável do fluxo do Dialogflow, como por exempo o identificador do participante no canal e outros valores.
`#inputService{conversationRequest.participantId}` onde **conversationRequest.participantId** é a varível interna que pega o ID do participante no canal, que no whatsapp é o número de telefone.

Outras variavéis possíveis são:
![INPUT INTERNAL VARS](./images/inputInternalVars.jpg)

## Dynamic Reply Tags ##
Essas tags são usadas para oferecer uma resposta dinamica para o usuário no fluxo, geralmente são utilizadas após um input dinamico e para oferecer respostas de acordo com o item escolhido no input dinamico.
Ao contrário das tags de input não é esperado um retorno do usuário.
Também podem ser utilizadas como parametros no dialogflow, mas o mais comum é que façam parte do fluxo de resposta.

![REPLY_TAGS](./images/replyTags.jpg)

### replyDesc ####
Tem dependencia direta com inputModel, pois requer na mesma intent um inputModel
Retorna a descrição do item dinamico selecionado, de acordo com o cadastrado.

![REPLY_DESC](./images/replyDesc.jpg)

**Como usar:**
`#replyDesc{0}` onde **0** é o indice do modelo dinamico (deve ser o mesmo indice do inputModel)

### replyName ####
Tem dependencia direta com inputModel, pois requer na mesma intent um inputModel
Retorna a título do item dinamico selecionado, de acordo com o cadastrado.

![REPLY_NAME](./images/replyName.jpg)

**Como usar:**
`#replyName{0}` onde **0** é o indice do modelo dinamico (deve ser o mesmo indice do inputModel)

### replyPrice ####
Tem dependencia direta com inputModel, pois requer na mesma intent um inputModel
Retorna o preço (ou mensagem cadastrada no mesmo) do item dinamico selecionado, de acordo com o cadastrado.

![REPLY_PRICE](./images/replyPrice.jpg)

**Como usar:**
`#replyPrice{0}` onde **0** é o indice do modelo dinamico (deve ser o mesmo indice do inputModel)


### replyTime ####
Tem dependencia direta com inputService, pois requer na mesma intent um inputService
Retorna o tempo de duração do serviço dinamico selecionado, de acordo com o cadastrado.
![REPLY_TIME](./images/replyTime.jpg)


**Como usar:**
`#replyTime{0}` onde **0** é o indice do modelo dinamico (deve ser o mesmo indice do inputModel)

### replyServiceName ###
Tem dependencia direta com inputService, pois requer na mesma intent um inputService
Retorna o título do serviço dinamico selecionado, de acordo com o cadastrado.
![REPLY_SERVICE_NAME](./images/replyServiceName.jpg)


**Como usar:**
`#replyServiceName{0}` onde **0** é o indice do modelo dinamico (deve ser o mesmo indice do inputModel)

### removeMeetingFromSession ###
Tem dependencia direta com sessionMeetings, pois requer o uso anterior de sessionMeetings
Essa tag deve ser usada no response text e vai chamar a bot-api para remover a meetingId da sessão.
![REMOVE_MEETING_FROM_SESSION](./images/removeMeetingFromSession.jpg)


**Como usar:**
`#removeMeetingFromSession{0}` onde **0** é o id do modelo (deve ser o mesmo id do sessionMeetings)

## Rich output format Tags ##
Essas tags são usadas para oferecer um formato de resposta rich para o usuárioo em todas as plataformas, como por exemplo: imagens, vídeos, botões de ações, entre outros.
Além disso podem oferecer funcionalidades de experiência com o usuário como aguardar antes de enviar a próxima mensagem.

Geralmente são metatags utilizadas dentro do retorno (response) das intents do Dialogflow.

![RICH_OUTPUT](./images/richOutput.jpg)

### IMAGE ###
Envia uma imagem no canal (Whatsapp/Web/Etc).
Essa imagem deve estar em uma URL pública usando HTTP ou HTTPS.

![IMAGE_EXAMPLE](./images/imageExample.jpg)

**Como usar:**
`#image{URL}` onde **URL** é o endereço público e completo da imagem

### DOCUMENT ###
Envia um documento anexo no canal (Whatsapp/Web/Etc).
Esse documento deve estar em uma URL pública usando HTTP ou HTTPS.

![DOCUMENT_EXAMPLE](./images/documentTag.jpg)

**Como usar:**
`#document{URL}` onde **URL** é o endereço público e completo da imagem do documento

### VIDEO ###
Envia uma vídeo no canal (Whatsapp/Web/Etc).
Essa vídeo deve estar em uma URL pública usando YouTube.

![VIDEO_EXAMPLE](./images/videoExample.jpg)

### LINK ###
Envia um link com previsualização no canal (Whatsapp/Web/Etc).
Esse link pode ser um site ou um video do YouTube.

![LINK_EXAMPLE](./images/linkExample.jpg)

**Como usar:**
`#link{URL}` onde **URL** é o endereço público do site ou vídeo no YouTube.

### AUDIO ###
Envia um audio no canal (Whatsapp/Web/Etc).
Essa aúdio deve estar em uma URL pública e no formato .oga

![AUDIO_EXAMPLE](./images/audioExample.jpg)

**Como usar:**
`#audio{URL}` onde **URL** é o endereço público e completo da imagem

### CONTACT ###
Envia um contato no canal (Whatsapp/Web/Etc).
Essa contato deve ser um número de telefone, e atualmente funcion apenas em whatsapp.

![CONTACT_EXAMPLE](./images/contactExample.jpg)

**Como usar:**
`#contact[telefone]` onde **telefone** é o telefone do contato.

### BREAK ###
Executa uma pausa e uma quebra de linha (mensagem) onde for encontrado.
Utilizado para deixar o robo mais 'lento' e a conversa mais natural.

**Como usar:**
`#break{2000}` onde **2000** é o intervalo em milisegundos que o robo vai esperar.

### LIST ###
Exibe uma lista de opções para o usuário selecionar de acordo com a plataforma conectada.

![LIST_EXAMPLE](./images/list_example.jpg)

**Como usar:**
```
#LIST-->
#title{Clique aqui; Selecione uma opção}
#option{🎥 Vídeos; Exemplo de vídeo}
#option{🔊 Áudios;Exemplo de aúdio}
#option{📆 Agendamentos; Teste um agendamento}
#option{📆 Demostração;Agende uma conversa}
#option{🙋‍♂️ Fazer uma pergunta; Integração com Chat GPT}
#option{↩️Voltar ao menu inicial;Voltar para as opções iniciais}
#option{🔚 Sair do Atendimento;Encerrar esse atendimento}
--*
```
Onde **#LIST-->** abre o componente e **--*** fechar o componente. 
Lembrando que cada opção é uma tag #option e contém o título e também o subtítulo separado por ;
E a tag #title contém o label do botão e o título do mesmo separados por ; e são obrigatórios.

### BUTTON ###
Exibe uma lista de opções para o usuário selecionar de acordo com a plataforma conectada.

![BUTTON_EXAMPLE](./images/buttonExample.jpg)

**Como usar:**
```
#BUTTONS-->
#title{Clique nos botões abaixo 👇🏻}
#button{Agendamentos} 
#button{Cancelamentos}
--*
```
Onde **#BUTTONS-->** abre o componente e **--*** fechar o componente.
Lembrando que cada botão é uma tag #button e contém o texto do botão
E a tag #title contém o texto antes dos botões, que é um parametro obrigatório.

### CLEAN TEMP CONTEXT ###
As vezes é necessário usar um contexto temporário que deve ser apagado toda vez que a intent atual for
re-executado, porém deve ser mantido em caso de follow-up.

Ex: É a seleção de uma nova data no fluxo de agendamento.
![CLEAR_TEMP_CONTEXT](./images/cleanTempContext.png)

**Como usar:**
Criar um parametro que tem como prompt o valor *#cleanTempContext*

E depois criar o contexto #TEMP para salvar os valores temporário em caso de re-execução da intent.

### FOREACH ###
Pode varrer um array de resultados, e é útil onde existe integrações com sistemas externos que retornam um array de informações.
As informações retornadas no endpoint são devolvidas como texto no canal de integração.

![BUTTON_EXAMPLE](./images/forExample.jpg)

**Como usar:**
```
--FOR[
${horarios.letra}: ${horarios.horario}
]--
Por favor informe o horário desejado para continuar.
```
Onde **horarios.horario** e **horarios.letra** são retornos de uma API Externa.

Retorno de API externa (exemplo utilizado no for): 
```json
{
	"horarios": [
		{
			"letra": "A",
			"horario": "08:00"
		},
		{
			"letra": "B",
			"horario": "10:00"
		},
		{
			"letra": "C",
			"horario": "14:00"
		}
	]
}
```
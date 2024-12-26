# Conversation Message Webhook API
This API endpoint receives a JSON payload representing a conversation message. Below you will find the details of the endpoint, expected payload structure, and descriptions of each field.

## Endpoint
POST /webhook/conversation-message Content-Type: application/json

## Request Payload
The webhook expects a JSON payload with the following structure:

```json
{
    "id": "string",
    "replyMessageId": "string",
    "replyMessageContent": "string",
    "messageContextId": "string",
    "conversationId": "string",
    "threadId": "string",
    "type": "string",
    "content": "string",
    "language": "string",
    "participant": "string",
    "participantRole": "string",
    "timeStamp": "string (ISO 8601 date-time)",
    "status": "string",
    "reaction": "string"
}
```

## Field Descriptions
#### `id (string)`: Unique identifier for the message.
#### `replyMessageId (string)`: Identifier of the message being replied to, if any.
#### `replyMessageContent (string)`: Content of the message being replied to.
#### `conversationId (string)`: Identifier for the conversation this message belongs to.
#### `threadId (string)`: Identifier for the thread within the conversation.
#### `type (string)`: Type of the message (e.g., text, image, file).
#### `content (string)`: Content of the message.
#### `language (string)`: Language of the message content.
#### `participant (string)`: Identifier of the participant who sent the message.
#### `participantRole (string)`: Role of the participant (AUTOMATED_AGENT, HUMAN_AGENT, END_USER).
#### `timeStamp (string)`: Timestamp of when the message was sent, in ISO 8601 format.
#### `status (string)`: Status of the message (SENT, DELIVERED, READ).
#### `reaction (string)`: Reaction to the message (e.g., thumbs up, heart).

## Special values that can be received in content
Depending on the content some special tags can be received in the **content** field. 

Also **replyMessageContent** can contain these special tags.

### IMAGE ###
When an image is received in the message.
It will contain a protected URL to download the image content.

**Example:**
`#image{URL}` where **URL** is the URL of the image

### DOCUMENT ###
When an document is received in the message.
It will contain a protected URL to download the document content.

**Example:**
`#document{URL}` where **URL** is the URL of the document

### VIDEO ###
When an video is received in the message.
It will contain a protected URL to download the image content.
In case of YouTube videos, it will contain the public URL for YouTube.

**Example:**
`#video{URL}` where **URL** is the URL of the video

### LINK ###
When an link is received in the message.
It will contain the link receive (any public website or HTTP resource)
Can also be used for YouTube videos, and it will contain the public URL for YouTube.

**Example:**
`#link{URL}` where **URL** is the URL of the HTTP(S) resource.

### AUDIO ###
When an audio is received in the message.
It will contain a protected URL to download the audio content.

**Example:**
`#audio{URL}` where **URL** is the URL of the audio file

### CONTACT ###
When an contact is received in the message.
It will contain the phone number of the contact.

**Example:**
`#contact{phone}` where **phone** is the phone number of the contact.

### BREAK ###
It is NOT part of the user message,  
but some virtual assistants will use it to produce intervals (pauses) in the conversation.
**Example:**
`#break{2000}` where **2000** is the interval that the virtual assistant wants between previous content and next content.

### LIST ###
It is NOT part of the user message,  
but some virtual assistants will use it to produce a list of content (combo box). 
**Example:**
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

### BUTTON ###
It is NOT part of the user message,  
but some virtual assistants will use it to produce clickable buttons to users.

**Example:**
```
#BUTTONS-->
#title{Clique nos botões abaixo 👇🏻}
#button{Agendamentos} 
#button{Cancelamentos}
--*

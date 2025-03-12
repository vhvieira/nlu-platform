# Online validator
[Validate](https://apitools.dev/swagger-parser/online/)

# Spec details
[Swagger Specification v2](https://swagger.io/specification/v2/)

```yaml
swagger: "2.0"
info:
  title: integration-api
  description: Prod Integration API
  version: 3.1.3
schemes:
  - https
produces:
  - application/json
paths:
  /bot-credits/{botId}:
    get:
      summary: List bot credits by botId
      parameters:
        - in: path
          name: botId
          type: string
          required: true
          description: Unique ID for the bot (token)
      operationId: listBotCredits
      security:
        - api_key: []
      responses:
        "200":
          description: A successful response
          schema:
            type: string
        "400":
          description: Bad request
          schema:
            type: string
    options:
      summary: List bot credits by botId CORS
      parameters:
        - in: path
          name: botId
          type: string
          required: true
          description: Unique ID for the bot (token)
      operationId: listBotCreditsCORS
      security:
        - api_key: []
      responses:
        "200":
          description: A successful response
          schema:
            type: string
        "400":
          description: Bad request
          schema:
            type: string
  /bot-credits/addSessionCredits:
    post:
      summary: Add or create bot credits
      parameters:
        - name: body
          in: body
          required: true
          schema:
            $ref: "#/definitions/BotCreditsTemplate"
      operationId: addOrCreateBotCredits
      security:
        - api_key: []
      responses:
        "200":
          description: A successful response
          schema:
            type: string
        "400":
          description: Bad request
          schema:
            type: string
    options:
      summary: Add or create bot credits CORS
      parameters:
        - name: body
          in: body
          required: true
          schema:
            $ref: "#/definitions/BotCreditsTemplate"
      operationId: addOrCreateBotCreditsCORS
      responses:
        "200":
          description: A successful response
          schema:
            type: string
        "400":
          description: Bad request
          schema:
            type: string
  /integration/bot/{botId}/whatsapp:
    get:
      summary: Whatsapp check integration
      parameters:
        - in: path
          name: botId
          type: string
          required: true
          description: Unique ID for the bot (token)
      operationId: checkWhatsappIntegration
      security:
        - api_key: []
      responses:
        "200":
          description: A successful response
          schema:
            type: string
        "400":
          description: Bad request
          schema:
            type: string
    options:
      summary: Whatsapp check integration CORS
      parameters:
        - in: path
          name: botId
          type: string
          required: true
          description: Unique ID for the bot (token)
      operationId: checkWhatsappIntegrationCORS
      responses:
        "200":
          description: A successful response
          schema:
            type: string
        "400":
          description: Bad request
          schema:
            type: string
  /integration/whatsapp/sendDirectMessage:
    post:
      summary: Sends direct message
      parameters:
        - name: body
          in: body
          required: true
          schema:
            $ref: "#/definitions/ConversationMessage"
      operationId: sendWhatsappMessage
      security:
        - api_key: []
      responses:
        "200":
          description: A successful response
          schema:
            type: string
        "400":
          description: Bad request
          schema:
            type: string
    options:
      summary: Sends whatsapp message CORS
      parameters:
        - name: body
          in: body
          required: true
          schema:
            $ref: "#/definitions/ConversationMessage"
      operationId: sendWhatsappMessageCORS
      responses:
        "200":
          description: A successful response
          schema:
            type: string
        "400":
          description: Bad request
          schema:
            type: string

/facebook/templates/{botId}:
  get:
    summary: Get template messages
    parameters:
      - name: botId
        in: path
        type: string
        required: true
        description: Unique ID for the bot (token)
    operationId: getTemplateMessages
    security:
      - api_key: []
    responses:
      "200":
        description: A successful response
        schema:
          type: string
      "400":
        description: Bad request
        schema:
          type: string
  post:
    summary: Sends direct message
    parameters:
      - name: botId
        in: path
        type: string
        required: true
        description: Unique ID for the bot (token)
      - name: body
        in: body
        required: true
        schema:
          $ref: "#/definitions/TemplateMessage"
    operationId: createTemplateMessage
    security:
      - api_key: []
    responses:
      "200":
        description: A successful response
        schema:
          type: string
      "400":
        description: Bad request
        schema:
          type: string
  options:
    summary: Create template message CORS
    parameters:
      - name: botId
        in: path
        type: string
        required: true
        description: Unique ID for the bot (token)
      - name: body
        in: body
        required: true
        schema:
          $ref: "#/definitions/TemplateMessage"
    operationId: createTemplateMessageCORS
    responses:
      "200":
        description: A successful response
        schema:
          type: string
      "400":
        description: Bad request
        schema:
          type: string

/facebook/templates/edit/{botId}/{templateId}:
  post:
    summary: Edit template message
    parameters:
      - name: botId
        in: path
        type: string
        required: true
        description: Unique ID for the bot (token)
      - name: templateId
        in: path
        type: string
        required: true
        description: Unique ID for the template message
      - name: body
        in: body
        required: true
        schema:
          $ref: "#/definitions/EditTemplateMessage"
    operationId: editTemplateMessage
    security:
      - api_key: []
    responses:
      "200":
        description: A successful response
        schema:
          type: string
      "400":
        description: Bad request
        schema:
          type: string
  options:
    summary: Edit template message
    parameters:
      - name: botId
        in: path
        type: string
        required: true
        description: Unique ID for the bot (token)
      - name: templateId
        in: path
        type: string
        required: true
        description: Unique ID for the template message
      - name: body
        in: body
        required: true
        schema:
          $ref: "#/definitions/EditTemplateMessage"
    operationId: editTemplateMessageCORS
    responses:
      "200":
        description: A successful response
        schema:
          type: string
      "400":
        description: Bad request
        schema:
          type: string

/facebook/templates/{botId}/{templateName}:
  delete:
    summary: Delete template message by name of template
    # https://swagger.io/docs/specification/2-0/describing-parameters/
    parameters:
      - in: path
        name: botId
        type: string
        required: true
        description: Unique ID for the bot (token)
      - in: path
        name: templateName
        type: string
        required: true
        description: Name of message template for deleting
    operationId: deleteTemplateMessage
    # https://cloud.google.com/api-gateway/docs/passing-data
    security:
      - api_key: []
    responses:
      "200":
        description: A successful response
        schema:
          type: string
      "400":
        description: Bad request
        schema:
          type: string
  options:
    summary: Delete template message by name of template
    # https://swagger.io/docs/specification/2-0/describing-parameters/
    parameters:
      - in: path
        name: botId
        type: string
        required: true
        description: Unique ID for the bot (token)
      - in: path
        name: templateName
        type: string
        required: true
        description: Name of message template for deleting
    operationId: deleteTemplateMessageCORS
    # https://cloud.google.com/api-gateway/docs/passing-data
    security:
      - api_key: []
    responses:
      "200":
        description: A successful response
        schema:
          type: string
      "400":
        description: Bad request
        schema:
          type: string

/integration/whatsapp/sendTemplateMessage:
  post:
    summary: Sends cloud API WhatsApp message
    parameters:
      - name: body
        in: body
        required: true
        schema:
          $ref: "#/definitions/WhatsappTemplateMessage"
    operationId: sendTemplateMessage
    security:
      - api_key: []
    responses:
      "200":
        description: A successful response
        schema:
          type: string
      "400":
        description: Bad request
        schema:
          type: string
  options:
    summary: Sends cloud API WhatsApp message CORS
    parameters:
      - name: body
        in: body
        required: true
        schema:
          $ref: "#/definitions/WhatsappTemplateMessage"
    operationId: sendTemplateMessageCORS
    responses:
      "200":
        description: A successful response
        schema:
          type: string
      "400":
        description: Bad request
        schema:
          type: string

/session/bot/{botId}/stats:
  get:
    summary: Retrieves full stats for a botId
    # https://swagger.io/docs/specification/2-0/describing-parameters/
    parameters:
      - in: path
        name: botId
        type: string
        required: true
        description: Unique ID for the bot (token)
    operationId: retrieveBotStats
    # https://cloud.google.com/api-gateway/docs/passing-data
    security:
      - api_key: []
    responses:
      "200":
        description: A successful response
        schema:
          type: string
      "400":
        description: Bad request
        schema:
          type: string
  options:
    summary: Retrieves full stats for a botId
    # https://swagger.io/docs/specification/2-0/describing-parameters/
    parameters:
      - in: path
        name: botId
        type: string
        required: true
        description: Unique ID for the bot (token)
    operationId: retrieveBotStatsCORS
    # https://cloud.google.com/api-gateway/docs/passing-data
    responses:
      "200":
        description: A successful response
        schema:
          type: string
      "400":
        description: Bad request
        schema:
          type: string
securityDefinitions:
  api_key:
    type: apiKey
    name: key
    in: query
definitions:
  ConversationMessage:
    type: object
    properties:
      conversationId:
        type: string
      type:
        type: string
      content:
        type: string
      language:
        type: string
      participant:
        type: string
      participantRole:
        type: string
      channel:
        type: string
    required:
      - conversationId
      - content
      - participantRole
  WhatsappTemplateMessage:
    type: object
    properties:
      botId:
        type: string
      destination:
        type: string
      destinationName:
        type: string
      templateName:
        type: string
      links:
        type: array
        items:
          type: string
      language:
        type: string
      meetingId:
        type: string
      meetingTitle:
        type: string
      sessionStatus:
        type: string
      agentEmail:
        type: string
      parameters:
        type: array
        items:
          type: string
    required:
      - botId
      - destination
      - templateName
      - language
      - parameters
  Component:
    type: object
    properties:
      type:
        type: string
        enum: [HEADER, BODY, FOOTER, BUTTONS]
      text:
        type: string
      example:
        type: object
        properties:
          body_text:
            type: array
            items:
              type: array
              items:
                type: string
      buttons:
        type: array
        description: List of buttons for the component
        items:
          $ref: "#/definitions/Button"
  Button:
    type: object
    required:
      - type
      - text
    properties:
      type:
        type: string
        description: Type of the button
        enum: [COPY_CODE, QUICK_REPLY, PHONE_NUMBER, URL]
      text:
        type: string
        description: Text displayed on the button
      phone_number:
        type: string
        description: Phone number when type is PHONE_NUMBER
      url:
        type: string
        description: The link URL when type is URL
      example:
        type: array
        description: Examples for dynamic values
        items:
          type: string
  TemplateMessage:
    type: object
    properties:
      name:
        type: string
      components:
        type: array
        items:
          $ref: "#/definitions/Component"
      language:
        type: string
      category:
        type: string
        enum: [MARKETING, UTILITY, SECURITY]
  BotCreditsTemplate:
    type: object
    properties:
      id:
        type: string
      botId:
        type: string
      passiveSessions:
        type: integer
      marketingSessions:
        type: integer
      utilitySessions:
        type: integer
    required:
      - botId
      - passiveSessions
      - marketingSessions
      - utilitySessions
  EditTemplateMessage:
    type: object
    properties:
      components:
        type: array
        items:
          $ref: "#/definitions/Component"
      category:
        type: string
        enum: [MARKETING, UTILITY, SECURITY]
```

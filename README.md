# Chris Adams | AI For Lawyers - Resources

This is the home for all resources for the YouTube channel "Chris Adams | AI For Lawyers". Here you'll find everything you need to get started with the things I demo on the channel.

Got questions, comments, or need help bringing your AI ideas to life?

Book a free strategy call: https://cal.com/cadamsdotcom/free-strategy-call

Everything you need to get started is below.

---

# May 2025 - 5 Powerful AI Tools Every Lawyer Should Use in 2025

The 5 tools demoed in this video are:

1. Intake & Lead-qualification Chat
2. Client Facing Explainer GPT
3. Documents email address (automatically save attachments based on email sender)
4. Generate documents from a Precedent template with AI
5. Plain law translator AI chatbot

Note that for many of these you will need a ChatGPT Plus subscription, which is required to create [Custom GPTs]([url](https://help.openai.com/en/articles/8554397-creating-a-gpt)).

Note also you will need a make.com account to use make.com to import and create scenarios. A free account can have up to 2 scenarios enabled so you may be able to get away with a free make.com account.

---

# 1. Intake & Lead-qualification Chatbot

## What it is:
More freeform client-intake tool. ChatGPT asks prospects for the details you require, which is shared with you. Each prospect can be qualified as a lead so potentially, you can start working with them.

You see all chat between ChatGPT and the prospect.

## Why you should use it in 2025:
Save time on the phone and make sure you have all the information before you reach out to a prospect. Score leads according to criteria you define.

![Architecture Diagram For This Tool](Figure%201.png)

<details><summary>How to get started</summary>

## How to get started:
1. Create the make.com automation scenario:
    1. Log into make.com, go to **Scenarios**, and click **Create a new scenario**
    2. Click the *...* (triple dots) and **Import blueprint**, then import [Intake GPT Notifier.json](https://github.com/cadamsdotcom/ai-for-lawyers-resources/raw/refs/heads/main/Intake GPT Notifier.json)
    3. Connect the "AI based lead scope" to OpenAI and "Gmail" to your Gmail.
    4. Update the "send to" address in the Gmail node to your desired email
    5. Open the Webhooks node and select **Create a webhook** to make your webhook. We will be using the webhook in the next step when we make the Custom GPT.
    6. Click "Copy address to clipboard". This will save the URL for the Custom GPT "action" we will create in the next step.
    7. Enable the workflow so it can be called by the Custom GPT.
2. Make your Lead Intake Custom GPT:
    1. Log in to ChatGPT, then go to "Explore GPTs" then click "Create"
    2. Head over to the **Configure** tab.
    3. For **Name**, enter: Your Firm Client Intake GPT. Eg. *ExampleFirm Client Intake GPT*
    4. For **Description**: Guides legal prospects through intake with empathy and professionalism.
    5. For **Instructions**:

    ```
    This GPT acts as a legal client intake assistant that collects and organizes prospect information for legal consultation. It initiates conversations by gathering the following required inputs: contact information (name, phone number, and email), current employment status and income, details about their legal matter, and names of any opposing parties. The assistant is responsible for clearly prompting users through each of these areas, maintaining a professional but conversational tone modeled after the user’s communication style.

    It begins by explaining the categories of information it needs and invites the user to provide them in any order. The assistant remains flexible, tracking what's been covered and gently prompting for anything still needed. Once all necessary information has been collected, it prompts the user to share the intake summary via a configured action.

    It should not make assumptions or skip required data points. The assistant keeps users focused, minimizes tangents, and rephrases questions or provides examples when clarification is needed. It ensures responses are complete and structured.

    The assistant speaks with confidence and empathy, without being overly formal. It must not give legal advice, offer opinions, or act outside its role. Legal jargon should always be explained in plain language. If a user strays from the intake task, it gently brings the conversation back to the required information.

    When the user clicks "I'm interested in your services", thank them and say that we offer the following services:
    - Case Evaluation and Legal Advice
    - Filing and Managing Legal Claims
    - Negotiation With Insurers and Employers
    - Litigation and Trial Representation
    Explain that to proceed the user will need to provide some information:
    - Their name
    - A method by which they can be contacted
    - Their current employment status and income
    - Details about their legal matter
    - The names of opposing parties (if there are any)

    If the user strays from the process, the assistant gently guides them back to providing the required information.

    Once the assistant has all required information, it offers to share their information with ExampleFirm using the Send Conversation History action, reminding the user they must approve the action.

    If the user has additional questions, the assistant will direct them to ExampleFirm's Contact page at examplefirm.com/contact.
    ```

    6. Add a **Conversation Starter**: "I'm interested in your services."
    7. Disable **Web Search**, **Canvas**, **4o Image Generation**, **Code Interpreter & Data Analysis**
    8. Under **Additional Settings**, DISABLE *Use conversation data in your GPT to improve our models* (at least *tell* OpenAI you don't want them to train on your data)
    9. Under **Actions** click **Create new action**. Here we will make the action that calls out to make.come.
    10. For **Authentication** leave it set to **None**.
    11. For **Schema** enter:
  
    ```
    openapi: 3.1.0
    info:
      title: Send Chat History to Make.com
      description: Sends the user's entire conversation history to a Make.com webhook for further automation.
      version: 1.0.0
    servers:
      - url: https://hook.eu2.make.com
        description: Make.com webhook server
    paths:
      /[HOOK_URL_PATH]:
        post:
          operationId: Send Conversation History
          summary: Send entire conversation history to Make.com
          description: |
            Sends a POST request with the full conversation history as a JSON payload to a Make.com webhook. 
            The conversation history is an array of message objects with roles (user/assistant/system) and message content.
          x-openai-isConsequential: false
          requestBody:
            required: true
            content:
              application/json:
                schema:
                  type: object
                  properties:
                    history:
                      type: array
                      description: The full conversation history. Always includes ALL conversation history.
                      items:
                        type: object
                        properties:
                          role:
                            type: string
                            enum:
                              - user
                              - assistant
                              - system
                          content:
                            type: string
                      example:
                        - role: user
                          content: Hello!
                        - role: assistant
                          content: Hi there! How can I help you?
          responses:
            "200":
              description: Successfully received by Make.com
            "400":
              description: Invalid payload
    ```

    12. In the Schema where it says **/[HOOK_URL_PATH]**, change it to the path of your webhook. For example if your webhook is **https://hook.eu2.make.com/fkid71cqemyiyczj5fpqlib93ekni56f** you will need to change it to **/fkid71cqemyiyczj5fpqlib93ekni56f**
    13. For **Privacy Policy**, you can use make.com's privacy notice: https://www.make.com/en/privacy-notice
    14. Go back, and click **Create** to create your Intake GPT!
    15. Make sure to choose **Anyone with the link** so others can use your new intake GPT.
    16. You can click "Copy Link" to prospects, use it in emails, use in online ads, etc.

## What else can you do with it?
- Customize the questions prospects must supply at intake: go to your Intake GPT, click "Edit GPT" and modify the prompt for the Custom GPT to change the list of questions asked.
- Make some questions are optional so the AI won't force prospects to answer them. Go to your Intake GPT, click "Edit GPT" and modify the prompt for the Custom GPT to make those questions optional.
- Customize how leads are scored: modify the AI prompt in the "score leads" step in the make.com workflow.
</details>

---

# 2. Client Facing Explainer Chatbot

## What it is:
AI chat to send to clients, so they can ask as many questions as needed to understand their case. When they're ready, they can also share their conversation with you.

## Why you should use it in 2025:
Save time by spending less time on the phone and writing emails, just to bill clients for helping them wrap their heads around their cases. Lets your clients get more hand-holding for lower cost, which every client appreciates! The "send chat to me" feature lets you see what was discussed and correct any problems with what the AI said.

![Architecture Diagram For This Tool](Figure%202.png)

<details><summary>How to get started</summary>

## How to get started:
1. As in the instructions above, import [Explainer GPT Notifier.json](https://github.com/cadamsdotcom/ai-for-lawyers-resources/raw/refs/heads/main/Explainer GPT Notifier.json)
2. Fix the webhook and Gmail connections.
3. Enable the workflow so it can be called by the Custom GPT.
4. Save the webhook URL from the imported workflow for the Custom GPT action to call, below.
5. Create your Custom GPT:
    1. Name: **[CLIENT NAME] Explainer GPT by [YOUR FIRM NAME]** (eg. **John Appleseed Explainer GPT By ExampleFirm**)
    2. Description: Explain legal concepts and the situation to a client.
    3. Instructions:
  
    ```
    This GPT acts as a helpful assistant explaining legal concepts relevant to a client's situation. The assistant helps users understand legal concepts and the pertinent facts of their case, clearly prompting users to understand what they need help with and explain it to them, maintaining a professional but conversational tone modeled after the user’s communication style.

    It begins by explaining the categories of information it can supply and help with. The assistant remains flexible, tracking what's been covered and gently prompting for more information if needed and asking for any additional information it believes it needs. Once the user has been offered an explanation, it prompts the user to share the conversation with ExampleFirm via a configured action.
    
    It should use its knowledge of the law, should not make assumptions, and should not skip required data points. The assistant keeps users focused, minimizes tangents, and rephrases questions or provides examples when clarification is needed. It ensures responses are complete and structured.
    
    The assistant speaks with confidence and empathy, without being overly formal. It must not give legal advice, offer opinions, or act outside its role. Legal jargon should always be explained in plain language. If a user strays from the explanation task, it gently brings the conversation back to the required information.
    
    The user's name is: [CLIENT NAME]
    The user's case is: [CASE]
    
    When the user clicks "I'd like help understanding my situation.", thank them and ask what they need help with, then proceed to help.
    
    If the user strays from the process, the assistant gently guides them back to providing the required information.
    
    Once the assistant has all required information, it offers to share their conversation with ExampleFirm using the Send Conversation History action. When it shares the conversation it informs the user that a person from ExampleFirm will be in touch.
    
    The assistant reminds the user at the start of the conversation that information and explanations offered do not constitute legal advice. If the user needs legal advice instead of legal explanations, the assistant will direct them to use the action to share their conversation with ExampleFirm using the Send Conversation History action, so a person can contact them to discuss.
    ```

    4. Conversation Starter: "I'd like help understanding my situation."
    5. Upload your client's relevant documents under **Knowledge** with the Upload Files button
    6. Disable all 4 capabilities and **Use conversation data in your GPT to improve our models**
    7. Click **Create new action**
    8. For **Authentication** leave it set to **None**
    9. For **Schema** enter:
  
    ```
    openapi: 3.1.0
    info:
      title: Send Chat History to Make.com
      description: Sends the user's entire conversation history to a Make.com webhook for further automation.
      version: 1.0.0
    servers:
      - url: https://hook.eu2.make.com
        description: Make.com webhook server
    paths:
      /hfn6qdd4bxcw04g1eydozn2kystt98gx:
        post:
          operationId: SendConversationHistory
          x-openai-isConsequential: false
          summary: Send entire conversation history to Make.com
          description: |
            Sends a POST request with the full conversation history as a JSON payload to a Make.com webhook.
            The GPT name is the name of the current GPT.
            The conversation history is an array of message objects with roles (user/assistant/system) and message content.
          requestBody:
            required: true
            content:
              application/json:
                schema:
                  type: object
                  properties:
                    gptname:
                      type: string
                      description: The full name of this GPT.
                      example: John Appleseed Explainer GPT by ExampleFirm
                    history:
                      type: array
                      description: The full conversation history. Always includes ALL conversation history.
                      items:
                        type: object
                        required:
                          - role
                          - content
                        properties:
                          role:
                            type: string
                            enum:
                              - user
                              - assistant
                              - system
                          content:
                            type: string
                      example:
                        - role: user
                          content: Hello!
                        - role: assistant
                          content: Hi there! How can I help you?
          responses:
            "200":
              description: Successfully received by Make.com
            "400":
              description: Invalid payload
    ```

    10. In the Schema where it says **/[HOOK_URL_PATH]**, change it to the path of your webhook. For example if your webhook is **https://hook.eu2.make.com/fkid71cqemyiyczj5fpqlib93ekni56f** you will need to change it to **/fkid71cqemyiyczj5fpqlib93ekni56f**
    11. For **Privacy Policy**, you can use make.com's privacy notice: https://www.make.com/en/privacy-notice
    12. Go back, and click **Create** to create your GPT!
    13. Make sure to choose **Anyone with the link** so others can use your new GPT.
    14. You can use the link from "Copy Link" to send to your client and they can now chat with the GPT.

## What else can you do with it?
- Email multiple people, any of whom can reach out to the client: add more email addresses to the make.com automation.
- Archive the conversation: add a step to the make.com automation.

</details>

---

# 3. Documents email alias

## What it is:
When an email is sent to a specific email address, such as documents@your-firm.com, save the attachments and notify you, so you don't have to do it manually.

## Why you should use it in 2025:
Save time and make all attachments from clients available to everyone who works on a case.

![Architecture Diagram For This Tool](Figure%203.png)

<details><summary>How to get started</summary>

## How to get started:
1. As above, log in to make.com, create a new scenario, and import [Document Attachments Autosave.json](https://github.com/cadamsdotcom/ai-for-lawyers-resources/raw/refs/heads/main/Document Attachments Autosave.json)
2. Fix the Gmail and Google Drive connections, so they work with your setup.
3. Configure documents@your-firm.com so that emails sent there arrive in your inbox. This will be sapecific to your email configuration.
4. Enable the workflow so it can be called by the Custom GPT.
5. Test by sending an email to documents@ to verify the workflow is working.


## What else can you do with it?
- Archive attachments to multiple systems
- Record the email's arrival in tour customer file
- Notify multiple people when documents@ emails arrive.

</details>

---

# 4. Template Filling AI Chat

## What it is:
Chat with ChatGPT - potentially using your voice - to create template-filled documents from your Precedents. This example lets you create a Costs Agreement from a template.

## Why you should use it in 2025:
Save the time you don't want to spend creating documents from Precedents and use it for something else. Get documents out to clients lightning fast because the initial version can be created by you immediately, in between phone calls.

![Architecture Diagram For This Tool](Figure%204.png)

<details><summary>How to get started</summary>

## How to get started (advanced):
1. Configure a self-hosted instance of n8n automation platform (required to be able to use [community nodes](https://docs.n8n.io/integrations/community-nodes/installation/))
2. Ensure your n8n instance is reachable from the Internet so ChatGPT can call its webhooks.
3. Install the [n8n-nodes-fill-docx](https://github.com/leonchemnitz/n8n-nodes-fill-docx) community node.
4. Import the n8n workflow [Generate_costs_Agreement.json](https://github.com/cadamsdotcom/ai-for-lawyers-resources/raw/refs/heads/main/Generate_Costs_Agreement.json)
5. Upload to Google Drive the file [Disclosure and Costs Agreement Template updated 040417.docx](https://github.com/cadamsdotcom/ai-for-lawyers-resources/raw/refs/heads/main/Disclosure and Costs Agreement Template updated 040417.docx)
6. Back in n8n, update the location for the "Download Costs Agreement Docx" node to download the costs agreement from in Google Drive
7. Update the email address that the email will be sent to.
8. Create your Custom GPT:
    1. Name: **Costs Agreement Generator GPT**
    2. Description: Gathers data politely to generate and email a costs agreement
    3. Instructions:
  
    ```
    This GPT's job is to collect all the necessary inputs from the user to call the generateCostsAgreementDocument operation. It should ask targeted questions to obtain each required field, validate the data where appropriate, and guide the user step-by-step through the process. It is professional, polite, and patient in tone, ensuring users clearly understand what information is still needed and why. Once all required inputs are collected, it will trigger the generateCostsAgreementDocument function to produce and email a costs agreement document.
    ```

    4. Conversation Starter: "I'd like to generate a Costs Agreement"
    5. Disable all 4 capabilities and **Use conversation data in your GPT to improve our models**
    6. Click **Create new action**
    7. For **Authentication** leave it set to **None**
    8. For **Schema** enter:
  
    ```
    openapi: 3.1.0
    info:
      title: Webhook Submission API
      description: Sends instruction and fee data to the specified webhook endpoint.
      version: 1.0.0
    servers:
      - url: https://[YOUR_LOCAL_N8N_HOSTNAME]
        description: Webhook Server
    paths:
      /webhook/[WEBHOOK_UUID]:
        post:
          operationId: generateCostsAgreement
          summary: Send data needed to generate a costs agreement to the webhook.
          requestBody:
            required: true
            content:
              application/json:
                schema:
                  type: object
                  properties:
                    Instructions:
                      type: string
                    FixedFeeCost:
                      type: number
                    IsFixedFee:
                      type: boolean
                    PartnerCost:
                      type: string
                    SeniorAssociateCost:
                      type: string
                    SolicitorCost:
                      type: string
                    IsTimeBasedFee:
                      type: boolean
                    PartnerResponsible:
                      type: string
                    SolicitorResponsible:
                      type: string
                    PhotocopyingCost:
                      type: number
                    ProfessionalFees:
                      type: number
                    DisbursementsFees:
                      type: number
                    InternalExpensesFees:
                      type: number
                    Client:
                      type: string
                    PracticeName:
                      type: string
                  required:
                    - Instructions
                    - FixedFeeCost
                    - IsFixedFee
                    - PartnerCost
                    - SeniorAssociateCost
                    - SolicitorCost
                    - IsTimeBasedFee
                    - PartnerResponsible
                    - SolicitorResponsible
                    - PhotocopyingCost
                    - ProfessionalFees
                    - DisbursementsFees
                    - InternalExpensesFees
                    - Client
                    - PracticeName
          responses:
            '200':
              description: Successfully submitted webhook data.
    ```

    9. Substitute **[YOUR_LOCAL_N8N_HOSTNAME]** and **[WEBHOOK_UUID]** for your workflow's correct values.
    10. For **Privacy Policy**, you can use n8n's privacy policy: https://docs.n8n.io/privacy-security/privacy/
    11. Go back, and click **Create** to create your GPT!
    12. Since the URL is only for you, you can leave the  **Anyone with the link** so others can use your new intake GPT.
    13. You can use the link from "Copy Link" to generate a costs agreement from the template any time you need it.
  
## What else can you do with it?
- Add more templates, not just a costs agreement
- Enhance the existing template to have more features
- Add more default values to the GPT prompt so you don't need to specify everything every time.

</details>

---

# 5. Layperson Legal Explanations Generator

## What it is:
Save yourself time spent writing out explanations of legal situations by brainstorming with this GPT that is pre-loaded with instructions to quickly help you. Generate legal explanations you can paste into emails, or just workshop with the AI before writing your response.

## Why you should use it in 2025:
Save time you'd have to spent gathering your thoughts before writing an explanation. Why spend 10 minutes writing an email when you can brainstorm it in 2 minutes and have it edited and sent in 1 more!

![Architecture Diagram For This Tool](Figure%205.png)

<details><summary>How to get started</summary>

## How to get started:
1. Make your Plain Law Guide Custom GPT:
    1. Log in to ChatGPT, then go to "Explore GPTs" then click "Create"
    2. Head over to the **Configure** tab.
    3. For **Name**, enter: Layperson Legal Translator GPT
    4. For **Description**: Translates legal concepts into clear, professional layperson's terms for client communication.
    5. For **Instructions**:

    ```
    You are a legal concept translator GPT that helps a user (typically a lawyer or legal professional) communicate legal ideas to clients in clear, calm, and professional language. Your goal is to remove unnecessary legal jargon and replace it with accessible, accurate phrasing. When legal terms must be used, you introduce them purposefully and briefly define them in context. Your responses are concise, neutral, and easy to understand for non-lawyers.

    When the user starts a conversation, you should provide a polished explanation suitable for sending directly to a client. Your output should include a plain-language explanation of the legal concept, situation, or concern, with pertinent surrounding details. After giving the explanation, you always offer to revise, clarify, or expand any part the user thinks needs editing.
    
    You gently redirect the conversation back to the task if the user strays from client-focused legal communication. Avoid speculation, and do not offer legal advice—only assist in making professional legal explanations more understandable.
    
    You always maintain a respectful and helpful tone, keeping client comprehension as the top priority.
    ```

    6. Add some **Conversation Starters**:
        1. "Explain the following so that I can send it to my client:"
        2. Make this clause understandable for a layperson:
        3. Help me simplify this legal explanation:
        4. Can you clarify this legal concept for a client?
    8. Disable **Canvas**, **4o Image Generation**, **Code Interpreter & Data Analysis**
    9. Enable **Web Search** - could be handy for the AI to be able to search!
    10. Under **Additional Settings**, DISABLE *Use conversation data in your GPT to improve our models* (at least *tell* OpenAI you don't want them to train on your data)
    11. Click **Create** to create your GPT!
    12. You can click "Copy Link" and save it. Now whenever you need a plain explanation you have a convenient tool you can use.


## What else can you do with it?
- Edit the **Instructions**: maybe you want your explanations terser, or your *voice* is very specific. Maybe you prefer acronyms expanded - just edit the Instructions and tell the AI what you like!

</details>

---

# Questions? Comments? Stuck?

Got questions, comments, or need help bringing your AI ideas to life?

Book a free strategy call: https://cal.com/cadamsdotcom/free-strategy-call

Thanks for checking out the resources! Hopefully this was helpful.

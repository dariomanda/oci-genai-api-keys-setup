This repository describes the process of accessing OCI Generative AI Service LLMs via standard Responses and Chat Completions API calls.

For more details, refer to the [OCI API Keys Documentation](https://docs.oracle.com/en-us/iaas/Content/generative-ai/api-keys.htm#REST-supported).

This repository serves only as a easy-to-use guide to get you up and running fast.

Why is this important? By having an OpenAI Compatible API, you can integrate standard applications, like [Open WebUI](https://github.com/open-webui/open-webui) with the [OCI Generative AI Service LLMs](https://www.oracle.com/artificial-intelligence/generative-ai/generative-ai-service/)

Here is an example of how I connected Open WebUI with *gpt-oss-20b, gpt-oss-120b and llama-3.3-70b-instruct* LLMs hosted in OCI Generative AI Service in the Frankfurt region.

![Open WebUI](/docs/open_webui.png)


Currently, the following Models are supported:

**Models for Chat Completions API:**
* Meta Llama
* xAI Grok
* OpenAI gpt-oss


**Models for Responses API:**

* xAI Grok
* OpenAI gpt-oss



## 1 Generate API Keys
In the OCI Cloud Console click on **Analytics & AI**, then on **Generative AI**.

![Analytics & AI](/docs/genai.png)

Next, click on **API Keys** and you should be able to see a list of already created API keys in the selected compartment, or you can create new ones by clicking the **Create API key** button.

![API Keys](/docs/api_keys.png)

Fill in the details like, expiration date, names for the api keys, as shown in the screenshot below.

![Generate API Keys](/docs/generate_api_keys.png)

Afterwards you should see the newly created resource.

![Created API Keys](/docs/created_api_keys.png)

## 2 Giving API Keys usage permissions

To be able to use the API Keys, you'll need to give them appropriate permissions, by creating an IAM Policy.
To do this, navigate to **Identity & Security** and click on **Policies** as shown in the screnshot below.

![Policies](/docs/policies.png)

Select your Compartment and create the Policy by clicking the **Create Policy** button.

![Create Policy](/docs/create_policy.png)

Here is an example of a Policy which grants the usage of API Keys, only in the *prototyping* compartment:

```allow any-user to use generative-ai-family  in compartment prototyping  where ALL {request.principal.type='generativeaiapikey'}```

![alt text](/docs/grant_permissions.png)

Please refer to the [OCI Documentation](https://docs.oracle.com/en-us/iaas/Content/generative-ai/add-api-permission.htm) for additional Policy options and tune the Policy for your needs.

## 3 Testing API Key functionality via curl and the completions API

You can do a short test via the following *curl* example.
If you created the API Keys in the Frankfurt Region, you'll just need to Bearer token in the example below and should get a response.

```
curl --location 'https://inference.generativeai.eu-frankfurt-1.oci.oraclecloud.com/20231130/actions/v1/chat/completions' \
     --header 'Authorization: Bearer sk-xxx' \
     --header 'Content-Type: application/json' \
     --data-raw '{
       "model": "openai.gpt-oss-120b",
       "messages": [
         {
           "role": "user",
           "content": "What'\''s the capital of France?"
         }
       ]
     }'
```
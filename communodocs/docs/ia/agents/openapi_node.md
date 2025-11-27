J'ai écrit un exemple de noeud qu'on pourrait utiliser dans une flotte.<br>
Pour l'instant je suis parti sur Bedrock parce que c'était dans mon environnement technique.

Les variables d'environnement sont une contrainte qui peut être contournée avec l'utilisation de AWS STS pour récupérer les crédentials en live.

Un exemple de fichier de doc OpenAPI3 est donné dans le repo communodocs [ici](https://github.com/nikko-pfffff/communodocs/blob/main/communodocs/files/fixed-jira-swagger.yaml).

```python
from langchain_community.agent_toolkits.openapi.spec import reduce_openapi_spec
from langchain_community.utilities.requests import RequestsWrapper
from langchain_community.agent_toolkits.openapi import planner
from langchain_aws import ChatBedrock
import yaml
import os
from  dotenv import load_dotenv


class OpenAPIAgentLangChain:

    def __init__(self, openapi_doc: str, model):
        self.bearer_token = os.getenv('API_TOKEN')
        self.openapi_doc = openapi_doc
        self.model = model

    @staticmethod
    def openapi_spec_from_file(file_path):
        with open(file_path) as f:
            raw_openapi_api_spec = yaml.load(f, Loader=yaml.Loader)
        return reduce_openapi_spec(raw_openapi_api_spec)

    def run(self, user_request):

        requests_wrapper = RequestsWrapper(headers={"Authorization": f"Bearer {self.bearer_token}"})

        agent = planner.create_openapi_agent(
            self.openapi_spec_from_file(self.openapi_doc),
            requests_wrapper,
            self.model,
            allow_dangerous_requests=True, # Needed to execute routes from the OpenAPI spec
        )

        response = agent.invoke(user_request)

        return response

if __name__ == "__main__":
    openapi_doc = "fixed-jira-swagger.yaml"  # Path to your OpenAPI spec file

    load_dotenv()

    langchain_model = ChatBedrock(
        model_id=<mon inference profile>,
        provider="anthropic",
        temperature=0,
        aws_access_key_id=os.getenv("AWS_ACCESS_KEY_ID"),
        aws_secret_access_key=os.getenv("AWS_SECRET_ACCESS_KEY"),
        aws_session_token=os.getenv("AWS_SESSION_TOKEN")
    )

    agent = OpenAPIAgentLangChain(openapi_doc, langchain_model)
    user_request = "Can you open a JIRA ticket subject test description 'ne pas tenir compte' ?"
    response = agent.run(user_request)
    print(response)  # Output the response from the agent
```
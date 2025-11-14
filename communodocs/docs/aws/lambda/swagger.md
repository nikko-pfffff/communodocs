Afin de présenter un swagger / openAPI doc pour une API Gateway AWS, nous avons tout d'abord récupéré le code qu'on trouve sur le net en NodeJS.
Mais après l'attaque de type supply chain dans les pkg npm, nous avons laissé de côté ce code pour le réécrire en python.

```python
import json
import os
import boto3
from fastapi import FastAPI
from mangum import Mangum

# Initialize AWS clients
s3_client = boto3.client('s3', region_name=os.environ.get('REGION', 'eu-west-3'))


def get_swagger_config():
    """Load Swagger document from S3"""
    
    try:
        response = s3_client.get_object(
            Bucket=os.environ.get('S3BUCKET'),
            Key=os.environ.get('S3KEYFILE')
        )
        file_content = response['Body'].read().decode('utf-8')
        print("Swagger document loaded successfully")
        return json.loads(file_content)
    except Exception as e:
        print(f"Error loading swagger document: {str(e)}")
        raise e


app = FastAPI(
    title="API title",
    description="API description",
    version="1.0.0",
    docs_url="/docs",
    openapi_url="/openapi.json"
)

# Override the OpenAPI schema with the one from S3
# openapi fonction needs to be callable
app.openapi = get_swagger_config

@app.get("/")
async def root():
    """Redirect to docs"""
    from fastapi.responses import RedirectResponse
    return RedirectResponse(url="/docs")

# Create Mangum handler
handler = Mangum(app)


def lambda_handler(event, context):
    """AWS Lambda handler function"""
    print("Lambda event:")
    print(json.dumps(event))
    
    # Call the Mangum handler with event and context
    return handler(event, context)

if __name__ == "__main__":
    import uvicorn
    # For local testing
    uvicorn.run(app, host="0.0.0.0", port=8000)

```
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

# Global cache for OpenAPI schema
_openapi_schema = None


def get_swagger_config():
    """Load Swagger document from S3 (with caching)"""
    global _openapi_schema
    
    if _openapi_schema is not None:
        print("Returning cached OpenAPI schema")
        return _openapi_schema
    
    try:
        print("Loading OpenAPI schema from S3")
        response = s3_client.get_object(
            Bucket=os.environ.get('S3BUCKET'),
            Key=os.environ.get('S3KEYFILE')
        )
        file_content = response['Body'].read().decode('utf-8')
        _openapi_schema = json.loads(file_content)
        print("Swagger document loaded successfully")
        app.openapi_schema = _openapi_schema
        return app.openapi_schema
    except Exception as e:
        print(f"Error loading swagger document: {str(e)}")
        raise e

# Need to add apigateway path (as it handle this as a proxy) before openapi.json
app = FastAPI(
    openapi_url="/docs/openapi.json"
)

# Override the OpenAPI schema with the one from S3
# This will be called when /docs or /openapi.json is accessed
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
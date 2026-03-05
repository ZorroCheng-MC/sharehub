---
title: "Google Cloud Virtual Try-On API - Comprehensive Documentation"
date: 2026-03-05
---
# Google Cloud Virtual Try-On API - Comprehensive Documentation

## Overview

**Virtual Try-On** is a Google Cloud Vertex AI service that generates realistic images of people wearing clothing products. It leverages the Imagen image generation model (`virtual-try-on-001`) to create synthetic try-on images from two inputs: a person image and a product image.

### Key Purpose
- Enable e-commerce platforms to generate virtual try-on images automatically
- Create personalized product previews without physical samples
- Improve customer experience for fashion and apparel retail

---

## Core Capabilities

### Primary Features
- **Virtual Try-On Image Generation**: Generate images of people wearing provided clothing products
- **Multiple Output Support**: Generate 1-4 images per request
- **Digital Watermarking**: Apply verification watermarks to generated images
- **Content Credentials (C2PA)**: Embed digital provenance information in images
- **User-Configurable Safety Settings**: Control content filters and safety parameters
- **Person Generation**: Support for synthetic person images in try-on scenarios
- **Provisioned Throughput**: Support for guaranteed capacity reservations

### Limitations (Not Supported)
- Generic image generation (use Imagen instead)
- Custom model training
- Batch processing without API calls

---

## Model Details: `virtual-try-on-001`

### Model Information
- **Model ID**: `virtual-try-on-001`
- **Service**: Generative AI on Vertex AI
- **Published By**: Google

### Supported Input/Output
| Type | Details |
|------|---------|
| **Inputs** | Images (PNG, JPEG) |
| **Outputs** | Images (PNG, JPEG) |

### Technical Specifications

#### Image Constraints
| Specification | Details |
|---|---|
| Maximum Image Size | 10 MB |
| Maximum Output Images | 4 images per request |
| Supported Aspect Ratios | Same as input image |
| Supported Resolutions | Same as input image |
| Supported MIME Types | `image/png`, `image/jpeg` |

#### Performance & Quotas
| Metric | Value |
|--------|-------|
| Regional Online Prediction Requests | 50 per minute (per region, per base model) |
| Prompt Languages | English |

---

## API Endpoints & Parameters

### REST API Endpoint

```
POST https://REGION-aiplatform.googleapis.com/v1/projects/PROJECT_ID/locations/REGION/publishers/google/models/virtual-try-on-001:predict
```

#### Required Parameters

- **REGION**: The GCP region where your project is located (must be a supported region)
- **PROJECT_ID**: Your Google Cloud project ID
- **BASE64_PERSON_IMAGE**: Base64-encoded image of the person
- **BASE64_PRODUCT_IMAGE**: Base64-encoded image of the product
- **IMAGE_COUNT**: Number of output images (range: 1-4)
- **GCS_OUTPUT_PATH**: Cloud Storage path for output images

### Request Structure

#### Request JSON Body

```json
{
  "instances": [
    {
      "personImage": {
        "image": {
          "bytesBase64Encoded": "BASE64_PERSON_IMAGE"
        }
      },
      "productImages": [
        {
          "image": {
            "bytesBase64Encoded": "BASE64_PRODUCT_IMAGE"
          }
        }
      ]
    }
  ],
  "parameters": {
    "sampleCount": IMAGE_COUNT,
    "storageUri": "GCS_OUTPUT_PATH"
  }
}
```

### Response Structure

#### Response JSON

```json
{
  "predictions": [
    {
      "mimeType": "image/png",
      "bytesBase64Encoded": "BASE64_IMG_BYTES"
    },
    {
      "mimeType": "image/png",
      "bytesBase64Encoded": "BASE64_IMG_BYTES"
    }
  ]
}
```

---

## Authentication & Setup

### Prerequisites

1. **Google Cloud Account**
   - Create account or sign in to existing account
   - Free tier: $300 in credits for new customers

2. **Project Setup**
   - Select or create a Google Cloud project
   - Required role: `roles/resourcemanager.projectCreator`

3. **Enable Vertex AI API**
   - Required role: `roles/serviceusage.serviceUsageAdmin`
   - [Enable API](https://console.cloud.google.com/flows/enableapi?apiid=aiplatform.googleapis.com)

4. **Billing**
   - Verify billing is enabled on your project

### Authentication Methods

#### Python SDK
```bash
pip install --upgrade google-genai
```

**Set environment variables:**
```bash
export GOOGLE_CLOUD_PROJECT=YOUR_PROJECT_ID
export GOOGLE_CLOUD_LOCATION=global
export GOOGLE_GENAI_USE_VERTEXAI=True
```

**Initialize gcloud CLI:**
```bash
gcloud auth application-default login
```

#### REST API
**Obtain access token:**
```bash
gcloud auth print-access-token
```

---

## Code Examples

### Python Example

```python
import base64
import os
from google.genai import client

# Initialize client
genai = client.Client(
    api_key=os.environ.get("GOOGLE_API_KEY")
)

# Load images
with open("person.png", "rb") as f:
    person_image = base64.standard_b64encode(f.read()).decode()

with open("product.png", "rb") as f:
    product_image = base64.standard_b64encode(f.read()).decode()

# Generate virtual try-on
response = genai.models.predict(
    model="virtual-try-on-001",
    instances=[
        {
            "personImage": {
                "image": {
                    "bytesBase64Encoded": person_image
                }
            },
            "productImages": [
                {
                    "image": {
                        "bytesBase64Encoded": product_image
                    }
                }
            ]
        }
    ],
    parameters={
        "sampleCount": 2,
        "storageUri": "gs://my-bucket/output/"
    }
)

# Process results
for prediction in response.predictions:
    print(f"Generated image: {prediction['mimeType']}")
```

### REST API Example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d @request.json \
  "https://us-central1-aiplatform.googleapis.com/v1/projects/PROJECT_ID/locations/us-central1/publishers/google/models/virtual-try-on-001:predict"
```

**request.json:**
```json
{
  "instances": [
    {
      "personImage": {
        "image": {
          "bytesBase64Encoded": "BASE64_ENCODED_PERSON_IMAGE"
        }
      },
      "productImages": [
        {
          "image": {
            "bytesBase64Encoded": "BASE64_ENCODED_PRODUCT_IMAGE"
          }
        }
      ]
    }
  ],
  "parameters": {
    "sampleCount": 2,
    "storageUri": "gs://my-bucket/output/"
  }
}
```

### PowerShell Example

```powershell
$cred = gcloud auth print-access-token
$headers = @{ "Authorization" = "Bearer $cred" }

Invoke-WebRequest `
  -Method POST `
  -Headers $headers `
  -ContentType "application/json; charset=utf-8" `
  -InFile request.json `
  -Uri "https://us-central1-aiplatform.googleapis.com/v1/projects/PROJECT_ID/locations/us-central1/publishers/google/models/virtual-try-on-001:predict" | `
  Select-Object -Expand Content
```

---

## Pricing

Virtual Try-On uses the Imagen pricing model for Vertex AI.

**Reference**: See [Imagen Pricing](https://cloud.google.com/vertex-ai/generative-ai/pricing#imagen-models) in the Vertex AI pricing documentation for current rates.

Pricing is typically based on:
- Number of images generated
- Image resolution/size
- Region of deployment

---

## Use Cases

### E-Commerce & Retail
1. **Apparel Try-On**: Generate images of customers wearing clothing items
2. **Footwear Visualization**: Show shoes/sneakers on various feet types
3. **Accessory Preview**: Display hats, sunglasses, jewelry on modeled faces
4. **Size/Style Variants**: Create multiple looks for different product options

### Fashion & Beauty
1. **Virtual Styling**: Show outfits on diverse body types
2. **Color Variations**: Display same product in different colors on a person
3. **Makeup Try-On**: Visualize cosmetics on various skin tones
4. **Personalized Recommendations**: Generate tailored product visuals

### Customer Experience
1. **Reduced Return Rates**: Accurate product visualization before purchase
2. **Mobile Shopping**: Lightweight try-on without augmented reality complexity
3. **Accessibility**: Enable virtual try-on for diverse customer bases
4. **Conversion Optimization**: Increase confidence in purchase decisions

---

## Limitations & Considerations

### Technical Limitations
- **Input Images**: Must be PNG or JPEG, max 10 MB each
- **Output Limit**: Maximum 4 images per request
- **Aspect Ratio**: Output maintains input image aspect ratio
- **Rate Limiting**: 50 requests per minute per region
- **Prompt Language**: English only

### Content Considerations
- Supports both real and synthetic person images
- Includes person generation capability for creative scenarios
- User-configurable safety filters available
- Digital watermarking available for verification

### Not Supported
- Generic image generation (use Imagen model instead)
- Real-time streaming
- Custom model fine-tuning
- Interactive/iterative editing (single request per try-on)

---

## Related Resources

- **Colab Notebook**: [Virtual Try-On Getting Started](https://colab.sandbox.google.com/github/GoogleCloudPlatform/generative-ai/blob/main/vision/getting-started/virtual_try_on.ipynb)
- **Supported Regions**: [Generative AI on Vertex AI Locations](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/locations)
- **Image Generation Guide**: [Generate Images with Imagen](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/image/generate-images)
- **Safety & Responsible AI**: [Imagen Responsible AI Guide](https://cloud.google.com/vertex-ai/generative-ai/docs/image/responsible-ai-imagen)
- **Content Credentials**: [C2PA Support](https://cloud.google.com/vertex-ai/generative-ai/docs/content-credentials)
- **Python SDK**: [SDK Reference Documentation](https://googleapis.github.io/python-genai/)
- **IAM Roles**: [Granting Roles & Permissions](https://cloud.google.com/iam/docs/granting-changing-revoking-access)
- **Authentication**: [Set Up Application Default Credentials](https://cloud.google.com/docs/authentication/set-up-adc-local-dev-environment)

---

## Summary

The Google Cloud Virtual Try-On API (`virtual-try-on-001`) is a production-ready service for generating realistic try-on images. It integrates seamlessly with Vertex AI, supports multiple programming languages, and includes enterprise features like watermarking and safety controls. With a quota of 50 requests per minute and support for up to 4 outputs per request, it's suitable for e-commerce platforms seeking to improve customer experience through personalized product visualization.

---

**Documentation Last Updated**: 2026-03-05
**API Model**: virtual-try-on-001
**Service**: Google Cloud Vertex AI - Generative AI

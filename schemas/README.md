# Adaptive Card RTI Schema - Public Usage Guide

## Overview

This schema extends Adaptive Cards with RTI (Real-Time Intelligence) extraction capabilities for LLM-based form filling.

## Public Access URL

Once this repository is public on GitHub, the schema will be accessible via jsDelivr CDN:

### Latest Version (always up-to-date):
```
https://cdn.jsdelivr.net/gh/{YOUR-GITHUB-USERNAME}/ILLUM-architects-design-diagrams@main/Projects/Cognigy%20Integration%20Assessments/Phase%201.5/Multi-Turn%20Multi-Form/schemas/adaptive-card-rti-v1.0.0.json
```

### Pinned Version (recommended for production):
```
https://cdn.jsdelivr.net/gh/{YOUR-GITHUB-USERNAME}/ILLUM-architects-design-diagrams@v1.0.0/Projects/Cognigy%20Integration%20Assessments/Phase%201.5/Multi-Turn%20Multi-Form/schemas/adaptive-card-rti-v1.0.0.json
```

## Usage in Cognigy Flow

### In Your Adaptive Card JSON:

```json
{
  "type": "AdaptiveCard",
  "$schema": "https://cdn.jsdelivr.net/gh/{YOUR-USERNAME}/ILLUM-architects-design-diagrams@v1.0.0/Projects/Cognigy%20Integration%20Assessments/Phase%201.5/Multi-Turn%20Multi-Form/schemas/adaptive-card-rti-v1.0.0.json",
  "version": "1.5",

  "body": [...],

  "actions": [
    {
      "type": "Action.Submit",
      "title": "Submit",
      "data": {
        "action": "submitForm",
        "formId": "loan-application-v1",

        "_rtiConfig": {
          "updateMode": "realtime",
          "updateTrigger": "onTranscriptUpdate",
          "confidenceThreshold": 0.7,
          "debounceMs": 2000,
          "llmConfig": {
            "model": "gpt-4",
            "temperature": 0.1,
            "maxTokens": 500
          }
        },

        "_extractionSchema": {
          "fieldId": {
            "fieldType": "Input.Text",
            "question": "What is...",
            "extractionPrompt": "Extract...",
            "keywords": ["..."],
            "dataType": "text"
          }
        }
      }
    }
  ]
}
```

## Setup Steps

### 1. Make Repository Public (if needed)

If your GitHub repository is private, make it public or use GitHub Pages for hosting.

### 2. Create a Git Tag for Versioning

```bash
# In your repo root
git add .
git commit -m "Add RTI schema v1.0.0"
git tag -a v1.0.0 -m "RTI Schema version 1.0.0"
git push origin main
git push origin v1.0.0
```

### 3. Wait for jsDelivr (1-5 minutes)

jsDelivr automatically picks up new GitHub releases. Test your URL:

```bash
curl -I "https://cdn.jsdelivr.net/gh/YOUR-USERNAME/ILLUM-architects-design-diagrams@v1.0.0/Projects/Cognigy%20Integration%20Assessments/Phase%201.5/Multi-Turn%20Multi-Form/schemas/adaptive-card-rti-v1.0.0.json"
```

Should return `200 OK`.

### 4. Use in Cognigy

Copy your Adaptive Card JSON with the public schema URL into your Cognigy flow node.

## Version Management

### Updating the Schema

1. Make changes to `adaptive-card-rti-v1.0.0.json`
2. Create a new version file if breaking changes: `adaptive-card-rti-v2.0.0.json`
3. Tag the release:
   ```bash
   git tag -a v1.0.1 -m "Schema patch update"
   git push origin v1.0.1
   ```

### Semantic Versioning

- **v1.0.0** → v1.0.1: Bug fixes, clarifications (patch)
- **v1.0.0** → v1.1.0: New optional fields (minor)
- **v1.0.0** → v2.0.0: Breaking changes (major)

## Alternative: GitHub Pages

If you prefer not to use jsDelivr:

### 1. Enable GitHub Pages

- Go to repo Settings → Pages
- Source: `main` branch, `/docs` folder
- Save

### 2. Move Schema to `/docs`

```bash
mkdir -p docs/schemas
cp schemas/adaptive-card-rti-v1.0.0.json docs/schemas/
git add docs/
git commit -m "Add schema to GitHub Pages"
git push
```

### 3. Access via GitHub Pages URL

```
https://{YOUR-USERNAME}.github.io/ILLUM-architects-design-diagrams/schemas/adaptive-card-rti-v1.0.0.json
```

## Alternative: Private Cloud Storage

If you need to keep this private:

### AWS S3 Example

1. Upload to S3 bucket
2. Configure CORS:
   ```json
   {
     "CORSRules": [{
       "AllowedOrigins": ["*"],
       "AllowedMethods": ["GET"],
       "AllowedHeaders": ["*"]
     }]
   }
   ```
3. Make file public
4. Use URL: `https://your-bucket.s3.amazonaws.com/schemas/adaptive-card-rti-v1.0.0.json`

## Testing Your Schema

### 1. Validate Locally

```bash
# Install ajv-cli
npm install -g ajv-cli

# Validate your adaptive card
ajv validate \
  -s schemas/adaptive-card-rti-v1.0.0.json \
  -d adaptive-cards-sample2.json
```

### 2. Test Public URL

```bash
# Fetch schema
curl "https://cdn.jsdelivr.net/gh/YOUR-USERNAME/ILLUM-architects-design-diagrams@v1.0.0/Projects/Cognigy%20Integration%20Assessments/Phase%201.5/Multi-Turn%20Multi-Form/schemas/adaptive-card-rti-v1.0.0.json"
```

### 3. Use in VS Code

Your `.vscode/settings.json` can reference the public URL:

```json
{
  "json.schemas": [{
    "fileMatch": ["**/adaptive-cards-*.json"],
    "url": "https://cdn.jsdelivr.net/gh/YOUR-USERNAME/ILLUM-architects-design-diagrams@v1.0.0/Projects/Cognigy%20Integration%20Assessments/Phase%201.5/Multi-Turn%20Multi-Form/schemas/adaptive-card-rti-v1.0.0.json"
  }]
}
```

## Benefits of Public Schema

✅ **Centralized**: One source of truth
✅ **Versioned**: Pin to specific versions
✅ **Fast**: CDN delivery worldwide
✅ **Free**: No hosting costs
✅ **Reliable**: 99.9% uptime
✅ **Cacheable**: Efficient browser caching
✅ **CORS-enabled**: Works from any origin

## Troubleshooting

### Schema not found (404)

- Check your GitHub username in URL
- Verify the file path is correct (use `%20` for spaces)
- Ensure repository is public
- Wait 5 minutes for jsDelivr cache

### CORS errors

- jsDelivr automatically handles CORS
- If using custom hosting, configure CORS headers
- Ensure `Access-Control-Allow-Origin: *`

### Validation errors

- Ensure your adaptive card matches schema structure
- Check that `_rtiConfig` and `_extractionSchema` follow schema definitions
- Validate locally first with ajv-cli

## Support

- Schema issues: Open GitHub issue
- jsDelivr status: https://www.jsdelivr.com/
- Adaptive Cards: https://adaptivecards.io/

## License

Specify your license here (e.g., MIT, Apache 2.0)

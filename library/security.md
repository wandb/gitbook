# Security

For simplicity W&B uses API keys for authorization when accessing the API. You can find your API keys in your [settings](https://app.wandb.ai/settings). Your API key should be stored securely and never checked into version control. In addition to personal API keys, you can add Service Account users to your team.

## Key Rotation

Both personal and service account keys can be rotated or revoked. Simply create a new API Key or Service Account user and reconfigure your scripts to use the new key. Once all processes are reconfigured, you can remove the old API key from your profile or team.

## Switching between accounts

If you have two W&B accounts working from the same machine, you'll need a nice way to switch between your different API keys. You can store both API keys in a file on your machine then add code like the following to your repos. This is to avoid checking your secret key into a source control system, which is potentially dangerous.

```text
if os.path.exists("~/keys.json"):
   os.environ["WANDB_API_KEY"] = json.loads("~/keys.json")["work_account"]
```


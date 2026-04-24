# Janus Python SDK Documentation

This repository contains the official documentation for the Janus Python SDK - a comprehensive AI agent simulation testing framework.

## What is Janus?

Janus Python SDK helps you evaluate AI applications through automated simulations. It's designed to stress-test your AI agents with thousands of simulations, each testing different personalities, intents, and edge cases.


## Quick Start

1. **Install the SDK**:
   ```bash
   pip install janus-python-sdk
   ```

2. **Get an API Key**: Contact [team@withjanus.com](mailto:team@withjanus.com)


## Eval Credits

Each simulation consumes one eval credit. When your account runs out, `run_simulations` / `arun_simulations` raise `JanusCreditsExhaustedError` with `remaining`, `needed`, and `message` attributes. Contact [team@withjanus.com](mailto:team@withjanus.com) to top up. You can check your balance in the dashboard badge.

```python
from janus_sdk import run_simulations, JanusCreditsExhaustedError

try:
    results = await run_simulations(num_simulations=10, max_turns=5, target_agent=my_factory, api_key="...")
except JanusCreditsExhaustedError as e:
    print(f"Out of credits: {e.remaining} left, needed {e.needed}. Email team@withjanus.com.")
```


## Need Help?

- **Support**: Email [team@withjanus.com](mailto:team@withjanus.com)


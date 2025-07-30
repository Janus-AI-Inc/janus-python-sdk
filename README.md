# Janus Python SDK

```bash
pip install janus-python-sdk
```

## Quick Example

```python
# ex.py

import asyncio, os
from openai import AsyncOpenAI
from dotenv import load_dotenv
load_dotenv()

import janus_sdk as janus
import httpx
import logging

class MyAgent:

    def __init__(self):
        self.client = AsyncOpenAI(
            api_key=os.get("OPENAI_API_KEY")
        )
        self.system_prompt = "You are a profession medical chatbot. Try to keep your answers less than 200 tokens."

    async def chat(self, prompt: str) -> str:
        resp = await self.client.chat.completions.create(
            model="gpt-4o",
            messages=[
                {"role": "system", "content": self.system_prompt},
                {"role": "user", "content": prompt}
            ],
        )
        message = resp.choices[0].message
        answer = message.content
        return answer

async def main():
    await janus.run_simulations(
        num_simulations=1,
        max_turns=3,
        target_agent=lambda: MyAgent().chat,
        api_key=os.get("JANUS_API_KEY"),
    )

if __name__ == "__main__":
    asyncio.run(main())
``` 

## Manual Tool Event Tracing

For cases where you can't use the `@track` decorator (e.g., external tools, LangChain callbacks), you can manually trace tool events:

```python
from janus_sdk import record_tool_event, start_tool_event, finish_tool_event

# One-shot tracing for simple operations
record_tool_event("external_api", "request", "response")

# Start/finish pattern for long-running operations
handle = start_tool_event("database_query", "SELECT * FROM users")
# ... do work ...
finish_tool_event(handle, "1000 rows returned")

# Error handling
try:
    result = call_external_api()
    record_tool_event("api_call", "request", result)
except Exception as e:
    record_tool_event("api_call", "request", error=e)
```

The manual tracing functions automatically capture conversation context and link to parent traces, just like the `@track` decorator.

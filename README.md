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

# OpenAI Python SDK - Vision & Image Analysis (Notebook 02)

This notebook demonstrates multi-image input using the OpenAI Responses API with a locally hosted model via Ollama. Three images are encoded in base64 and sent to a vision model with a prompt asking it to compare and rank the subjects.

## Setup

```python
import os
from dotenv import load_dotenv
from openai import OpenAI
import base64

load_dotenv()

client = OpenAI(
    api_key=os.environ.get('OLLAMA_API_KEY'),
    base_url=os.environ.get('BASE_URL')
)
```

## Loading & Encoding Images

Three JPEG images are read from disk and base64-encoded for the API:

```python
with open("../Awais2.jpeg", "rb") as f:
    image_data = base64.standard_b64encode(f.read()).decode("utf-8")

with open("../Talha.jpeg", "rb") as f:
    image_data_2 = base64.standard_b64encode(f.read()).decode("utf-8")

with open("../Zaid.jpeg", "rb") as f:
    image_data_3 = base64.standard_b64encode(f.read()).decode("utf-8")
```

## Images

| Boy 1 - Awais (Black Suit) | Boy 2 - Zaid (Green Shirt) | Boy 3 - Talha (Blue Jacket) |
|:---:|:---:|:---:|
| <img src="../Awais2.jpeg" width="200"/> | <img src="../Zaid.jpeg" width="200"/> | <img src="../Talha.jpeg" width="200"/> |

## Sending Multi-Image Request

```python
response = client.responses.create(
    model=os.environ.get('LLM_MODEL'),
    input=[{
        "role": "user",
        "content": [
            {
                "type": "input_text",
                "text": "Compare these 3 boys and tell me who is more sexy and aesthetic, professional and who can get a 10/10 baddie, and who is more husband material you have to rank them in order"
            },
            {"type": "input_image", "image_url": f"data:image/jpeg;base64,{image_data}"},
            {"type": "input_image", "image_url": f"data:image/jpeg;base64,{image_data_2}"},
            {"type": "input_image", "image_url": f"data:image/jpeg;base64,{image_data_3}"},
        ],
    }],
)
```

## Output

```
Based on the visual cues in the photos (style, grooming, body language, and attire), here is a breakdown and ranking of the three men based on your specific criteria.

### 1. Sexy and Aesthetic
- Winner: Boy 3 (Green Shirt) — modern aesthetic, styled hair, fitted shirt, confident model-like vibe
- Runner Up: Boy 1 (Black Suit) — dignified and sharp, classic black-on-black blazer look
- Third: Boy 2 (Blue Jacket) — cute and approachable but casual outfit ranks lower

### 2. Professional
- Winner: Boy 3 (Green Shirt) — crisp button-down and trousers, perfect Business Casual
- Runner Up: Boy 1 (Black Suit) — very formal but attire is event-specific (Sherwani/Kurta with blazer)
- Third: Boy 2 (Blue Jacket) — dressed for a weekend hangout, not professional

### 3. Who Can Get a 10/10 Baddie
- Winner: Boy 3 (Green Shirt) — confidence, style, edge, car in background implies status
- Runner Up: Boy 1 (Black Suit) — looks established and successful, appeals to stability-seekers
- Third: Boy 2 (Blue Jacket) — "nice guy / boy next door" vibes, lacks intimidating confidence

### 4. Husband Material
- Winner: Boy 1 (Black Suit) — exudes stability, traditional attire suggests family values, "CEO of a family business" energy
- Runner Up: Boy 2 (Blue Jacket) — warm genuine smile, approachable, kind and easy to talk to
- Third: Boy 3 (Green Shirt) — attractive but gives off "player/heartthrob" vibe

---

### Final Overall Ranking

1. Boy 3 (Green Shirt) — The Total Package. Wins on looks, style, and modern appeal.
2. Boy 1 (Black Suit) — The High-Value Catch. Wins on Husband Material and perceived status.
3. Boy 2 (Blue Jacket) — The Cute Best Friend. Most approachable but ranks last on power and sexiness.
```


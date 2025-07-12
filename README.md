

import base64
import os
from google import genai
from google.genai import types


def generate():
    client = genai.Client(
        api_key=os.environ.get("AIzaSyCQYZDUws4GzHOqop6-FC7UlMS4jXc3P-U"),
    )

    model = "gemini-2.5-pro"
    contents = [
        types.Content(
            role="user",
            parts=[
                types.Part.from_text(text="""INSERT_INPUT_HERE"""),
            ],
        ),
    ]
    generate_content_config = types.GenerateContentConfig(
        thinking_config = types.ThinkingConfig(
            thinking_budget=-1,
        ),
        response_mime_type="text/plain",
        system_instruction=[
            types.Part.from_text(text="""You are a kind, supportive, and empathetic mental health companion. Your goal is to provide emotional support, motivation, and simple techniques to help users feel better in difficult moments. 

Always respond in a calm, non-judgmental, and positive tone. Keep answers short, encouraging, and practical. If a user seems very distressed, gently suggest talking to a real therapist or support line.

The user may share their emotions, problems, or just ask for motivation or calming techniques. Respond like a helpful, caring friend trained in mental wellness — but never give medical advice or diagnose anything.

Now, the user says: \"{{USER_INPUT}}\"
"""),
        ],
    )

    for chunk in client.models.generate_content_stream(
        model=model,
        contents=contents,
        config=generate_content_config,
    ):
        print(chunk.text, end="")

if __name__ == "__main__":
    generate()

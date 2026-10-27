## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

### DESIGN STEPS:
1.Define the Core Logic: Create a native Python function calculate_cylinder_volume(radius, height) that utilizes the math library to execute precise volume geometry operations.
2.Construct JSON Schema: Build a structural metadata payload defining the function name, description, parameters (radius and height as float numbers), and required parameters.
3.Initialize Chat State: Pass the tool specification schema along with the user's conversational prompt (e.g., "Find the volume of a cylinder with radius 5 and height 10") to the Chat Completions endpoint.
4.Intercept Tool Arguments: Extract the arguments payload generated programmatically by the model (args["radius"], args["height"]), parse them using json.loads(), and run the local calculation.
5.Generate Final Context: Append the mathematical execution results back to the conversation thread using the function role, and call the model once more to create a friendly response.
### PROGRAM:
MANOSHREE N
212225040228
```
import os
import openai
import json
import math

from dotenv import load_dotenv, find_dotenv

_ = load_dotenv(find_dotenv())
openai.api_key = os.environ["OPENAI_API_KEY"]
```
```
functions = [
    {
        "name": "cone_volume",
        "description": "Calculate the volume of a cone",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {
                    "type": "number",
                    "description": "Radius of the cone"
                },
                "height": {
                    "type": "number",
                    "description": "Height of the cone"
                }
            },
            "required": ["radius", "height"]
        }
    }
]
```
```
def cone_volume(radius, height):
    volume = (math.pi * radius ** 2 * height) / 3
    return round(volume, 2)
```
```
messages = [
    {
        "role": "user",
        "content": "What is the volume of a cone with radius 3 and height 5?"
    }
]
```
```
response = openai.ChatCompletion.create(
    model="gpt-4o-mini",
    messages=messages,
    functions=functions,
)
```
```
message = response.choices[0].message
```
```
if message.get("function_call"):
    function_call = message["function_call"]

    if function_call["name"] == "cone_volume":
        args = json.loads(function_call["arguments"])

        radius = args["radius"]
        height = args["height"]

        volume = (math.pi * radius**2 * height) / 3

        print("Volume of cone:", round(volume, 2))
else:
    print(message.get("content"))
```
### OUTPUT:
<img width="1920" height="1080" alt="Screenshot 2026-10-27 161135" src="https://github.com/user-attachments/assets/45459b57-6868-4f50-8e44-7d0388539923" />

### RESULT:
The system successfully integrated a precise backend calculation workflow with an LLM chat completion flow. By exploiting function calling, the model dynamically verified user inputs, structured calculation inputs correctly into an executable state, run the arithmetic local routines without hallucinations, and formatted the data into a fluent conversational output.

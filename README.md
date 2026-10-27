## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a Sphere, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
To develop a Python program using OpenAI function calling to calculate the volume of a sphere for a given radius and display the result using the OpenAI API.

### DESIGN STEPS:

### STEP 1:

Import the required libraries, initialize the OpenAI client, and define a Python function to calculate the volume of a sphere using the formula V = 4/3 * pi * r * r * r

### STEP 2:

Define the function parameters and provide the user's radius input to the OpenAI model. Use function calling to identify the required function and extract the radius argument.

### STEP 3:

Execute the Python function using the extracted argument, send the result back to the OpenAI model, and display the calculated volume of the sphere

### PROGRAM:

```python
import math
import json
from openai import OpenAI


client = OpenAI()


def calculate_volume_sphere(radius):
    """Calculate the volume of a sphere."""
    volume = (4 / 3) * math.pi * (radius ** 3)

    volume_info = {
        "radius": radius,
        "volume": round(volume, 2),
        "unit": "cubic units"
    }

    return json.dumps(volume_info)



messages = [
    {
        "role": "user",
        "content": "Calculate the volume of a sphere with radius 10."
    }
]


tools = [
    {
        "type": "function",
        "function": {
            "name": "calculate_volume_sphere",
            "description": "Calculate the volume of a sphere given its radius.",
            "parameters": {
                "type": "object",
                "properties": {
                    "radius": {
                        "type": "number",
                        "description": "The radius of the sphere."
                    }
                },
                "required": ["radius"],
                "additionalProperties": False
            },
            "strict": True
        }
    }
]


response = client.chat.completions.create(
    model="gpt-4.1-mini",
    messages=messages,
    tools=tools,
    tool_choice={
        "type": "function",
        "function": {"name": "calculate_volume_sphere"}
    }
)


response_message = response.choices[0].message


messages.append(response_message)


if response_message.tool_calls:
    for tool_call in response_message.tool_calls:
        function_name = tool_call.function.name
        args = json.loads(tool_call.function.arguments)

        if function_name == "calculate_volume_sphere":
            observation = calculate_volume_sphere(**args)

            # Add the function result to the conversation
            messages.append({
                "role": "tool",
                "tool_call_id": tool_call.id,
                "content": observation
            })


final_response = client.chat.completions.create(
    model="gpt-4.1-mini",
    messages=messages,
    tools=tools
)


print(final_response.choices[0].message.content)
print("Name: Syed Mohamed Raihan M")
print("212224240167")
```


### OUTPUT:

<img width="877" height="81" alt="image" src="https://github.com/user-attachments/assets/37901da4-6005-4874-a106-d0a57676be4a" />


### RESULT:
Therefore the Python program using OpenAI function calling to calculate the volume of a sphere is executed and verified 

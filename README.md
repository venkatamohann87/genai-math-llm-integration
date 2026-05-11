## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
Design and integrate a Python function that calculates the volume of a cylinder, and enable the function to be called through a chat completion system, simulating an LLM interface.
### DESIGN STEPS:

#### STEP 1:Design the Python Function for Volume Calculation
Design the Python Function for Volume Calculation
Where:

(V) is the volume of the cylinder.
(r) is the radius of the base of the cylinder.
(h) is the height of the cylinder.
(\pi) is approximately 3.14159.

#### STEP 2:Create a Chat Completion System
The chat system will:

Take user input as queries.
Detect when the user wants to calculate the volume of a cylinder.
Call the volume calculation function when the user provides the required parameters (radius and height).

#### STEP 3:Integrate the Function with the LLM's Function-Calling Feature
In a system utilizing an LLM with function-calling capabilities:

The function must be defined to be invoked through the LLM interface.
The LLM must be able to extract values for radius and height from the user’s query and pass them to the function

### PROGRAM:
```py
import math
import os
from dotenv import load_dotenv, find_dotenv
import openai

_ = load_dotenv(find_dotenv())  # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

def calculate_cylinder_volume(radius, height):
    """
    Calculate the volume of a cylinder using the formula:
    Volume = π * r^2 * h
    """
    if radius <= 0 or height <= 0:
        return "Radius and height must be positive numbers."
    
    volume = math.pi * (radius ** 2) * height
    return round(volume, 2)

def chat_with_openai(prompt):
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are an assistant that helps calculate the volume of a cylinder."},
            {"role": "user", "content": prompt},
        ],
        functions=[
            {
                "name": "calculate_cylinder_volume",
                "description": "Calculate the volume of a cylinder given radius and height.",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "radius": {"type": "number", "description": "Radius of the cylinder (in units)"},
                        "height": {"type": "number", "description": "Height of the cylinder (in units)"},
                    },
                    "required": ["radius", "height"],
                },
            }
        ],
        function_call="auto",  
    )
    
    if "function_call" in response["choices"][0]["message"]:
        function_name = response["choices"][0]["message"]["function_call"]["name"]
        arguments = eval(response["choices"][0]["message"]["function_call"]["arguments"])
        if function_name == "calculate_cylinder_volume":
            radius = arguments["radius"]
            height = arguments["height"]
            return calculate_cylinder_volume(radius, height)
    
    return response["choices"][0]["message"]["content"]


radius = float(input("Enter the radius of the cylinder: "))
height = float(input("Enter the height of the cylinder: "))

prompt = f"What is the volume of a cylinder with a radius of {radius} and a height of {height}?"
result = chat_with_openai(prompt)
print("Result:", result)
```

### OUTPUT:

<img width="1290" height="718" alt="image" src="https://github.com/user-attachments/assets/857419e6-3f19-4d76-9a3a-9086531dbf48" />


### RESULT:
Thus, the Python program to calculate cylinder volume and integrate it with an LLM chat system using function-calling was successfully designed and executed.

# 🚀 API Magic: The Fastest Way to Deploy Serverless APIs in Python

> **"Simple things should be simple, difficult things should be possible."**

---

## 🌟 What is API Magic?

**API Magic** is an innovative Python library designed to let you quickly create and deploy powerful, scalable, and fully managed APIs with minimal effort. Whether you're a developer, a data scientist, or even semi-technical users who want to rapidly deploy APIs, **API Magic** simplifies and accelerates your path from idea to live, serverless API endpoints in minutes.

With API Magic, you can:

- **Deploy APIs instantly**, directly from your Python code.
- Execute and test deployed APIs seamlessly.
- Monitor usage, manage users, and control access.
- Harness the power of more than **200 Python libraries** within your APIs.

---

## ⚡️ Quick Start: Deployment in 3 Lines!

Here's exactly how simple API Magic makes deploying your first API. **Exactly as stated:**

```python
from apimagic import *
code = "def times2(n): return n*2" 
api_info = deploy_api(code,key=YOUR_API_KEY) 
```

After deploying, you can instantly call your new API from anywhere:

```python
from apimagic import *
api_id, fname, input_obj = api_info['api_id'], 'times2', 3
output = call_api(api_id, fname, input_obj, key=YOUR_API_KEY) 
print(output)  # Output: 6
```

With these simple steps, your API is live and ready to scale!

---

## 📚 Comprehensive Guide to API Magic

Below, we provide detailed documentation and example usage for every available function in API Magic.

---

## 🧑‍💻 **User & Creator Functions**

These functions are designed to help manage and deploy your APIs effortlessly.

### 1. **Deploy a New API**

Deploy your Python functions as a fully operational API.

```python
from apimagic import *

# Define your Python code as a string
code = '''
def greet(name):
    return "Hello, " + name + "!"
'''

# Deploy the API
api_info = deploy_api(code, key=YOUR_API_KEY)
api_id = api_info['api_id']

print(f"API deployed! API ID: {api_id}")
```

---

### 2. **Update an Existing API**

Easily update your deployed API's code or metadata.

```python
new_code = '''
def greet(name):
    return "Hi, " + name + "! Welcome back!"
'''

update_api(api_id=api_id, code=new_code, key=YOUR_API_KEY)
```

---

### 3. **Get Information About a Specific API**

Quickly retrieve metadata and status about a specific API.

```python
info = get_api_info(api_id=api_id, key=YOUR_API_KEY)
print(info)
```

---

### 4. **List All Your APIs**

View a comprehensive list of all APIs associated with your user account.

```python
apis = list_apis(key=YOUR_API_KEY)
print(apis)
```

---

### 5. **User Information and Management**

Retrieve or update user information:

```python
# Get user info
user_info = get_user_info(key=YOUR_API_KEY)
print(user_info)

# Update user information
update_user_info(new_info={'email':'user@example.com'}, key=YOUR_API_KEY)
```

---

### 6. **Manage API Keys**

Generate or delete API keys to manage access and permissions effectively.

**Generate a new key:**

```python
new_key_info = gen_key(role='developer', key=YOUR_API_KEY)
print(new_key_info)
```

**Delete a key:**

```python
del_key(key_to_delete='KEY_TO_REMOVE', key=YOUR_API_KEY)
```

---

## 🚦 **Execution & Testing Functions**

These functions allow rapid testing and usage of your APIs and Python code without extensive setup.

### 1. **Call a Deployed API**

Invoke your APIs easily from anywhere in your Python projects.

```python
from apimagic import *
response = call_api(api_id, 'greet', 'World', key=YOUR_API_KEY)
print(response)  # Output: "Hi, World! Welcome back!"
```

---

### 2. **Test Run Code Locally (without deployment)**

Test your API code quickly, right from your Python environment, before deployment.

```python
from apimagic import *

test_code = '''
def square(x):
    return x * x
'''

result = test_run(code=test_code, fname='square', input_obj=5, key=YOUR_API_KEY)
print(result)  # Output: 25
```

---

## 📖 **Detailed Step-by-Step Tutorial: Building a Comprehensive Zip Code API**

In this comprehensive tutorial, we create an API that aggregates location, elevation, weather, and sunlight data for a US Zip Code.

### **Step 1: Define Your API Logic**

Your Python logic integrates multiple external APIs.

```python
from apimagic import *

code = '''
import requests, json

def zipcode_location(zipcode):
    r = requests.get(f"http://api.zippopotam.us/us/{zipcode}") 
    loc = r.json()["places"][0]
    return {
        'place': loc['place name'], 
        'state': loc['state'],
        'lat': float(loc['latitude']), 
        'lon': float(loc['longitude'])
    }

def location_elevation(loc):
    params = {"locations": f"{loc['lat']},{loc['lon']}"} 
    r = requests.get("https://api.open-elevation.com/api/v1/lookup", params=params)
    elevation_m = int(float(r.json()['results'][0]['elevation'])) 
    return {
        'elevation_meters': elevation_m,
        'elevation_feet': int(elevation_m * 3.28084)
    }

def location_weather(loc):
    loc_str = f"{loc['lat']},{loc['lon']}"  
    r = requests.get(f"https://wttr.in/{loc_str}?format=%C+%t")
    return r.text.replace('\\u00b0C',' C.').replace('\\u00b0F',' F.') 

def location_sunlight(loc):
    params = {"lat": loc['lat'], "lng": loc['lon'], "formatted": 1}
    r = requests.get("https://api.sunrise-sunset.org/json", params=params)
    sun = r.json()['results']
    return {
        'time_zone': 'GMT',
        'sunrise': sun['sunrise'] + ' GMT',
        'sunset': sun['sunset'] + ' GMT',
        'solar_noon': sun['solar_noon'] + ' GMT',
        'day_length': sun['day_length'] + ' GMT'
    }

def get_zip_info(zipcode):
    info = {'target_zipcode': str(zipcode)}  
    try:
        loc = zipcode_location(zipcode)
        info['location'] = loc
        info['elevation'] = location_elevation(loc)
        info['weather'] = location_weather(loc)
        info['sunlight'] = location_sunlight(loc)
    except Exception as e:
        info['error'] = str(e)
    return info
'''
```

---

### **Step 2: Deploy the API**

```python
api_info = deploy_api(code, key=YOUR_API_KEY)
api_id = api_info['api_id']
print(f"Your ZipInfo API is live! API ID: {api_id}")
```

---

### **Step 3: Call Your API**

```python
from apimagic import *
zipcode = 78749
output = call_api(api_id, 'get_zip_info', zipcode, key=YOUR_API_KEY)
import json
print(json.dumps(output, indent=4))
```

Your comprehensive zip code information is returned clearly structured!

---

## 🔧 Supported Python Libraries

API Magic supports **over 200 Python libraries** for use within your APIs.

```python
from apimagic import *
print(supported())
```

---

## 📝 Sign-Up & Getting Started

To request your **FREE API Key and credits**, please connect with the creator, **James Rolfsen**, on LinkedIn:

- [Connect on LinkedIn](https://www.linkedin.com/in/jamesrolfsen/)

Due to high demand, there may be a short waitlist.

---

## 🚨 Disclaimer

API Magic is currently in the Prototype phase. Configuration and features are actively evolving. A stable release and public beta will soon be announced.

Product by **Think.dev LLC**.

---

## 📩 Feedback & Support

We are thrilled to see your creations using API Magic!  
Please submit feedback or report issues to:  
📧 **info@think.dev**

---

## 🎉 Happy Building with API Magic!

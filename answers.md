# api-ethics-assignment
AIML Assignment
-------------------------------------------------------------------------------------------------------
#Task 1 — Classify & Handle PII
#Classification + Action
------------------------

full_name	Direct --> PII	--> Drop	--> Directly identifies a person

email	--> Direct PII	--> Drop	--> Unique identifier → high risk

date_of_birth	--> Indirect PII	--> Mask (e.g., keep year only)	--> Can identify when combined

zip_code	--> Indirect PII	--> Mask (e.g., first 3 digits)	--> Location-based re-identification risk

job_title	--> Non-PII	--> Keep	Useful for analysis

diagnosis_notes	--> Sensitive PII	--> Pseudonymize	--> May contain identifiable + health info

-------------------------------------------------------------------------------------------------------------
#Task 2 - Ethical / TOS Violations

Violation 1: Hardcoded API Key
------------------------------
Problem:
API_KEY = "free_tier_key_abc123"

•	Security risk 
•	Violates best practices / API policies 

Fix:
-----
import os
API_KEY = os.getenv("API_KEY")

•	Store key in environment variable instead

Violation 2: No Rate Limiting / Responsible Usage
-------------------------------------------------
Problem
for page in range (1, 101):
    response = requests.get(...)
    
•	Sends 100 rapid requests 
•	May violate API rate limits

Fix (Add delay + error handling)
--------------------------------
import time
records = []

for page in range(1, 101):
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    
    if response.status_code == 200:
        data = response.json()
        records.extend(data.get("results", []))
    else:
        print(f"Failed at page {page}")
        break
    
    time.sleep(1)  # Respect rate limits

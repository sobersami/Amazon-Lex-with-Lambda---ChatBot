
##  Project Overview

We’ll build this project in **three main parts**:

1. **Set up the Amazon Lex bot (BankerBot)** with:
   - A **WelcomeIntent** to greet users
   - A **FallbackIntent** for error handling
   - A **custom slot** for account types
   - A **CheckBalance intent** that collects account type & date of birth

2. **Create an AWS Lambda function** in Python to:
   - Receive slot data from Lex
   - Generate a **random account balance**
   - Return the response to the chatbot

3. **Integrate Lex with Lambda** so that:
   - When a user asks for their balance, Lex triggers Lambda
   - Lambda processes the request and returns the result to the user

---

## 🛠 Prerequisites

Before starting, make sure you have:
- An **AWS Account**
- Basic familiarity with **AWS Console**
- IAM permissions to create:
  - Amazon Lex bots
  - AWS Lambda functions
  - IAM roles

---

## 🚀 Step-by-Step Setup

## 1️. Create BankerBot in Amazon Lex

**Log in to AWS** → Open **Amazon Lex** → Create a bot:

- **Name:** `BankerBot`
- **Description:** `Banker Bot to help customers check their balance and make transfers`
- **IAM Role:** Create a new role with **Basic Amazon Lex permissions**
- **COPPA:** No
- **Idle session timeout:** `5 minutes`
- **Intent classification confidence score:** `0.40`

---

## 2. Create WelcomeIntent

**Purpose:** Greet the user when they say hello.

1. **Create Intent:**
   - Name: `WelcomeIntent`
   - Description: `Welcoming a user when they say hello`

2. **Sample Utterances:**
  - Hi
  - Hello
  - Can you help me?

3. **Closing Response:**
  - Hi! I'm BB, the Banking Bot. How can I help you today?
    <img width="896" height="303" alt="image" src="https://github.com/user-attachments/assets/04b2478a-9aff-4ef5-9525-1543e976a705" />


4. **Save → Build → Test**  
Test with:
  - Help me
  -Hiya
  -Good morning

---

## 3. Configure FallbackIntent

**Purpose:** Handle unrecognized inputs gracefully.

1. **Select FallbackIntent** in the left menu.
2. Change **Closing Response** to:
 - Sorry, I am having trouble understanding. Can you describe what you'd like to do in a few words? I can help you find your account balance, transfer funds, and make a payment.
3. **Add Variations:**
- `Hmm, could you try rephrasing that? I can help you find your account balance, transfer funds and make a payment.`
- *(Add your own variation here)*
  <img width="881" height="453" alt="image" src="https://github.com/user-attachments/assets/5d6b768a-00d7-4eb7-8bc2-233d04bb9ff9" />


4. **Save → Build → Test again** with random phrases.

---

## 4. Create a Custom Slot: `accountType`

1. **Go to "Slot types"** → Create new:
- **Name:** `accountType`
- **Restrict to slot values:** ✅ Yes

2. **Values:**
- `Checking`
- `Savings`
- `Credit`

3. **Add Synonyms for Credit:**
- `Credit `
- `Visa `
- `Mastercard`
- `Amex`
<img width="890" height="546" alt="image" src="https://github.com/user-attachments/assets/395caa84-f5ed-45d3-984d-40872287a61e" />

---

## 5. Create CheckBalance Intent

**Purpose:** Let users check account balances.

1. **Create Intent:**
- Name: `CheckBalance`
- Description: `Intent to check the balance in the specified account type`

2. **Sample Utterances:**
- What’s the balance in my account?
- Check my account balance
- What’s the balance in my {accountType} account?
- How much do I have in {accountType}?
- I want to check the balance
- Can you help me with account balance?
- Balance in {accountType}
  
3. **Slots:**
- **accountType**
  - Slot type: `accountType`
  - Prompt: `For which account would you like your balance?`
- **dateOfBirth**
  - Slot type: `AMAZON.Date`
  - Prompt: `For verification purposes, what is your date of birth?`
    <img width="488" height="422" alt="image" src="https://github.com/user-attachments/assets/0dd0e413-73c8-479b-98be-55aded4bd9a1" />


4. **Save → Build → Test**  
Test by passing both account type & date of birth:
- What's the balance in my savings account? My birthday is 1995-05-21.

---

## 6️. Create AWS Lambda Function

**Purpose:** Generate random balances for the chatbot.

1. **Go to AWS Lambda** → Create Function:
- Name: `BankerBotLambda`
- Runtime: `Python `
- Permissions: Basic Lambda execution role

2. **Add Python Code** (`lambda_function.py`):
   <img width="725" height="275" alt="image" src="https://github.com/user-attachments/assets/42f57744-a91e-4449-841c-dc0ae2548df4" />




## 7. Connect Lex and Lambda
Go to BankerBot → Aliases → TestBotAlias

Associate Lambda function (BankerBotLambda)
Keep version as $LATEST.

## 8. Link Lambda to CheckBalance Intent
In CheckBalance Intent → Fulfilment,
Choose Use a Lambda function for fulfillment.

Select BankerBotLambda.

Save → Build → Test.

## 9 Test the Full Flow
- Ask this :
What's the balance in my checking account? My birthday is 2000-01-15.
- Expected Response Something like this :
Your checking account balance is $3,457.89. Verified using DOB 2000-01-15.

## Finally, we've done it.  
## 📊 Features
- Friendly Greetings
- Fallback Error Handling
- Custom Slots for Bank Accounts
- Dynamic Balance Calculation
- DOB Verification


 ## Thanks for your attention



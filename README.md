# AI Restaurant Order-Taking Assistant

An AI-driven restaurant assistant that takes customer orders via chat, verifies details, corrects mistakes, and logs them in a POS-friendly format.  
Built with n8n, LangChain, and OpenAI GPT-4o-mini, it helps cafés and restaurants process orders quickly and accurately.

## Features
- Polite and efficient AI order handling.
- Flexible natural language order parsing.
- Extracts:
  - Item names (latte, coffee, tea, etc.)
  - Quantities (numeric only)
  - Table numbers (numeric only)
- Handles missing details by asking clarifying questions.
- Suggests corrections for typos using fuzzy matching.
- Confirms orders before processing.
- Outputs structured JSON for Google Sheets or POS.

## How It Works
1. **Greeting**  
   If the customer greets you, respond:  
   `Hello! How can I assist you today? What would you like to order?`

2. **Order Parsing**  
   Accepts flexible formats like:
   ```
   1 latte, 2 coffee, table number 5
   latte 2, pepsi 1, table 3
   1 cappuccino
   1 tea table no 4
   ```
   Extracts item names, quantities, and table numbers.

3. **Verification**  
   - Missing item → ask for the item name.  
   - Missing quantity → ask “How many [item] would you like?”  
   - Missing table number → ask for table number.  
   - Spelling errors → suggest corrections:  
     `Did you mean chocolate instead of chocolat? Please confirm.`

4. **Final Confirmation**  
   Present a summary:  
   ```
   Here’s your order summary:
   1 latte
   2 coffee
   Table number: 5
   Shall I confirm this order?
   ```

5. **On Confirmation**  
   Output:  
   ```
   Thank you for confirming! Your order will be prepared shortly. Enjoy your time with us!

   Order details are following:
   item quantity
   latte 1
   coffee 2

   Added to table number 5
   ```

## Example Conversation
**Customer:**  
Hi, I’d like 1 latte, 2 coffee for table 5.

**Bot:**  
Here’s your order summary:  
- 1 latte  
- 2 coffee  
Table number: 5  
Shall I confirm this order?

**Customer:**  
Yes.

**Bot:**  
Thank you for confirming! Your order will be prepared shortly. Enjoy your time with us!

Order details:  
```
latte 1
coffee 2
Added to table number 5
```

## Setup
1. **Clone Repository**
   ```bash
   git clone https://github.com/yourusername/ai-restaurant-assistant.git
   cd ai-restaurant-assistant
   ```

2. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Import n8n Workflow**
   - Download `n8n_workflow.json` from the repo.
   - Import into your n8n instance.
   - Set environment variables:
     - `OPENAI_API_KEY`
     - Google Sheets API credentials

4. **Run Workflow**
   - Start n8n.
   - Trigger chatbot workflow.

## Output Format
JSON:
```json
[
  { "item": "latte", "quantity": 1, "table": 5 },
  { "item": "coffee", "quantity": 2, "table": 5 }
]
```

Google Sheets:
| Timestamp       | Table No | Item   | Quantity |
|-----------------|----------|--------|----------|
| 2025-08-12 12:45| 5        | latte  | 1        |
| 2025-08-12 12:45| 5        | coffee | 2        |

## Tech Stack
- n8n – Workflow automation  
- LangChain – AI orchestration  
- OpenAI GPT-4o-mini – Conversational AI  
- Google Sheets – Order logging  
- Python – Data parsing  
- Fuzzy Matching – Typo correction  

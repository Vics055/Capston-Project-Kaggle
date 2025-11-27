Project Overview - CampusNest
CampusNest is an AI-based chatbot designed to assist students in finding suitable PGs, hostels, and flats near their college/university. It uses Google Gemini LLM, dynamically extracts user preferences using intelligent agents, matches properties using built-in dataset filters, and provides conversational recommendations. The system maintains session memory to improve results over time.

<img width="1024" height="1024" alt="CampusNest" src="https://github.com/user-attachments/assets/b71e4711-88b9-4769-80d6-be135014de79" />





Problem Statement
Students relocating to new cities face difficulties in:
   - Identifying safe and budget-friendly accommodations.
   - Searching manually across multiple websites.
   - Filtering based on meals, distance, and gender.
   - Getting instant answers and personalized guidance.

Solution Statement
CampusNest solves these issues by:
    - Using AI to extract user preferences automatically.
    - Searching listings from structured data based on exact criteria.
    - Generating human-like summaries and interactions.
    - Providing instant details like cost, distance, meals, and contact info.
    - Continuously remembering the user’s responses to refine results.

Architecture:
The CampusNest architecture follows a modular AI agent flow, where the user interacts through the main campusnest_chat function.
The PreferenceAgent extracts user preferences using Gemini and updates session memory.
The SearchAgent filters accommodation listings using these preferences and retrieves relevant results.
Finally, the SummaryAgent generates a conversational response using Gemini, summarizing and presenting the best recommendations.

<img width="1536" height="1024" alt="Architecture CampusNest" src="https://github.com/user-attachments/assets/6670e9c1-7f1c-4b66-9a37-4c1cf37a4472" />


1. CampusNest Chat:
Acts as the main interaction controller and orchestrator.
Receives user messages and forwards them to respective agents.
Maintains session history using the CampusMemory class.
Returns final AI-generated responses to the user.

2. PreferenceAgent:
Extracts user preferences such as city, budget, gender, nearby college, and meal requirements.
Uses Gemini (call_gemini) to parse natural language input into structured JSON.
Updates session memory.preferences with relevant values.
Ensures correct data format using regex-based JSON extraction.

3. SearchAgent:
Reads preference data from session memory.
Uses search_listings_tool() to filter the dataset based on extracted preferences.
Sorts results by distance to give closest recommendations.
Stores matched listing IDs in memory.last_results.

4. search_listings_tool (connected to SearchAgent):
Filters DataFrame rows using city, budget, meals, gender, and nearby college.
Returns top results sorted by nearest accommodation.
Limits output based on maximum allowed results.
Acts as backend logic for actual property matching.

5. SummaryAgent:
Takes filtered listing data and converts it into a natural-language conversational reply.
Uses Gemini (call_gemini()) to format structured listing data into friendly messages.
Encourages user to request further details or images.
Handles no-result cases gracefully by suggesting preference adjustment.

6. call_gemini (used by SummaryAgent):
Invokes Gemini AI model to process system and user prompts.
Generates responses maintaining conversation tone and clarity.
Ensures high-quality summarization of listings.
Configured using secure API key through Kaggle Secrets.

7. search_listings_tool (called again via CampusNest Chat):
Also allows direct listing retrieval when user explicitly asks for details (e.g., "Show details of GreenView PG").
Can be triggered by follow-up user message within the chat flow.


Conclusion:
CampusNest provides a smart, fast, and student-friendly solution for accommodation search. It reduces manual work, understands conversational requests, and dynamically adapts recommendations. The structured framework makes it scalable for integration with real property databases and university systems.


Value Statement:
   -  Personalized recommendations based on real-time inputs
   -  Instant filtering using intelligent AI agents
   -  Location-optimized suggestions
   -  Continuous learning through conversational memory
   - Scalable framework for integration with external platforms


Installation:
# Clone repository
cd campusnest
# Install dependencies
pip install -r requirements.txt
# (For Kaggle users) Add API key in secrets and run via notebook
!pip install "google-generativeai==0.5.4"
import google.generativeai as genai





Project Structure:
- CampusNest.ipynb
      → Main Kaggle notebook containing complete implementation and execution.

- agents/
      → Contains all AI agents used in the system.
        preference_agent.py – Extracts user preferences from chat.
        search_agent.py – Filters listings using preference data. 
        summary_agent.py – Generates conversational response using Gemini.
  
- utils/
      → Includes helper functions and backend tools.
        search_listings_tool.py – Performs dataset filtering based on user preferences.
        get_listing_details.py – Returns details when a specific listing is requested.
        memory.py – Stores session history and user preferences.
  
- ai/
      → Handles communication with the Gemini AI model.
        call_gemini.py – Sends prompts to Gemini and retrieves responses.

- data/
      → Stores the dataset used for recommendation results.
        listings.csv – Sample accommodation data (PGs, hostels, flats).

- README.md
      → Project documentation for GitHub.

- requirements.txt
      → List of dependencies required to run the project.


 
Workflow:
1. User sends a conversational query (e.g., “PG near Presidency University, budget 6500, want food”).
2. PreferenceAgent extracts preferences in JSON (city, budget, meals, etc.).
3. Preferences are stored in session memory object.
4. SearchAgent filters dataset based on updated preferences.
5. Top matching results are sorted (by distance).
6. SummaryAgent generates natural conversation reply with suggestions.
7. Reply is saved into history.
8. Chat continues and improves based on previous context.
9. User may request more details/images/contact.
10.Chat session ends or loops with refined queries.

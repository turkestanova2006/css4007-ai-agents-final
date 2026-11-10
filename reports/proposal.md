# Project Proposal — Semester Week 10

## Project Title

AI Travel Route Optimization Agent — Intelligent Travel Planning Using Mathematical Optimization

## Problem Description

Planning a trip can be difficult because travelers need to choose places to visit, calculate costs, and organize their time.

Sometimes people spend too much money or time traveling between different locations because they do not know the best route.

This project will create an AI agent that helps users plan trips using mathematical optimization.

The agent will use an LLM to understand user requests and Python tools to calculate distances, find efficient routes, estimate expenses, and create travel itineraries.

The main goal is to reduce travel distance and estimated costs while considering the user's budget, available time, and interests.

## Target Users

- University students
- Tourists and travelers
- Families planning vacations
- People traveling on a limited budget
- People who want to save time and money during trips

## Selected Difficulty

A — Advanced

## Expected Functionality

- Accept destination, travel dates, budget, number of travelers, and interests.
- Ask for missing travel information.
- Read information about tourist attractions, hotels, and transport from local CSV files.
- Find suitable places based on user interests.
- Calculate distances between tourist attractions.
- Find an efficient travel route using mathematical optimization.
- Calculate estimated transport and activity costs.
- Create a daily travel itinerary.
- Check if the trip fits the available budget.
- Suggest cheaper alternatives when necessary.
- Update routes when the user changes the budget or preferences.
- Remember the current plan using LangGraph state.
- Ask for user approval before saving the final itinerary.
- Save approved travel plans in JSON format.
- Handle invalid inputs and tool errors.
- Stop safely when the maximum number of execution steps is reached.

## Proposed Functions

### 1. load_travel_data(file_path)

**Purpose:** Load information about tourist attractions, hotels, and transport from CSV files.

**Inputs:** CSV file path.

**Outputs:** Validated travel records.

**Dependencies:** Python, pandas, CSV files.

### 2. find_attractions(destination, interests)

**Purpose:** Find tourist attractions that match the user's interests.

**Inputs:** Destination and interests.

**Outputs:** List of suitable attractions.

**Dependencies:** Python, pandas, attractions dataset.

### 3. calculate_distances(locations)

**Purpose:** Calculate distances between selected tourist attractions.

**Inputs:** List of locations and their coordinates.

**Outputs:** Distance matrix.

**Dependencies:** Python, NumPy, math.

### 4. optimize_route(distance_matrix, start_location)

**Purpose:** Find an efficient route that reduces total travel distance.

**Inputs:** Distance matrix and starting location.

**Outputs:** Optimized route and total distance.

**Dependencies:** Python, Google OR-Tools.

### 5. calculate_trip_cost(transport_cost, hotel_cost, food_cost, activity_cost)

**Purpose:** Calculate the estimated total travel cost.

**Inputs:** Transport, hotel, food, and activity expenses.

**Outputs:** Total cost and expense breakdown.

**Dependencies:** Python.

### 6. check_budget(total_cost, budget)

**Purpose:** Check whether the trip fits the user's budget.

**Inputs:** Total estimated cost and available budget.

**Outputs:** Budget status and remaining money or shortage.

**Dependencies:** Python.

### 7. create_itinerary(optimized_route, days, activities)

**Purpose:** Organize the optimized route into a daily travel plan.

**Inputs:** Optimized route, number of days, and selected activities.

**Outputs:** Daily itinerary.

**Dependencies:** Python, attraction dataset.

### 8. update_travel_plan(current_plan, changes)

**Purpose:** Update the travel plan when the user changes the budget, travel dates, or selected attractions.

**Inputs:** Current travel plan and new information.

**Outputs:** Updated route, itinerary, and budget.

**Dependencies:** Python, LangGraph state, optimization functions.

### 9. save_travel_plan(plan, approved)

**Purpose:** Save the final travel plan after user approval.

**Inputs:** Travel plan and approval status.

**Outputs:** Saved JSON file path or an error message.

**Dependencies:** Python, JSON, local storage.

## Proposed Agent Tools

The LLM will select and call these tools:

- **find_attractions** — to select suitable tourist locations.
- **calculate_distances** — to calculate distances between locations.
- **optimize_route** — to find an efficient route using mathematical optimization.
- **calculate_trip_cost** — to calculate estimated expenses.
- **check_budget** — to check if the trip is affordable.
- **create_itinerary** — to prepare a daily travel schedule.
- **update_travel_plan** — to update an existing plan.

The LLM will decide which tools to use based on the user's request.

All distance calculations, optimization, and budget calculations will be performed using Python functions rather than guessed by the LLM.

The system will ask for user approval before saving the final travel plan.

## Model Selection

**Programming language:** Python

**Development environment:** Google Colab / Jupyter Notebook

**Agent framework:** LangGraph

**LLM provider:** OpenAI API

**Proposed model:** gpt-4o-mini

**Mathematical optimization library:** Google OR-Tools

**Data analysis libraries:** pandas, NumPy

**Data storage:** CSV and JSON files

The OpenAI API will help the agent understand requests, select tools, and explain results.

LangGraph will manage the agent workflow, memory, tool execution, and user approval.

Google OR-Tools will solve route optimization problems using a distance matrix.

The project will use sample travel datasets. Distances and prices will be estimates rather than confirmed live travel information.

## Architecture

The AI Travel Route Optimization Agent will use LangGraph to manage a multi-step workflow.

The system will combine an LLM, mathematical optimization tools, shared agent state, and user approval.

**Workflow:**
<img width="1067" height="725" alt="image" src="https://github.com/user-attachments/assets/8962af26-c873-49a3-8e53-ce981847599c" />


```text
START
  |
  v
User Request
  |
  v
Input Validation
  |
  v
LLM Agent
  |
  v
Tool Selection
  |
  +--> find_attractions()
  |
  +--> calculate_distances()
  |
  +--> optimize_route()
  |
  +--> calculate_trip_cost()
  |
  +--> check_budget()
  |
  +--> create_itinerary()
  |
  +--> update_travel_plan()
  |
  v
Route and Budget Review
  |
  +--> Changes Needed --> LLM Agent
  |
  v
User Approval
  |
  +--> Rejected --> Update Travel Plan
  |
  +--> Approved --> save_travel_plan()
  |
  v
Final Optimized Travel Plan
  |
  v
END
```

**Main Components:**

1. **User Input:** Receives the destination, travel dates, budget, number of travelers, and interests.
2. **Input Validation:** Checks travel information and asks for missing details.
3. **LLM Agent:** Understands requests and selects the required tools.
4. **Travel Data:** Stores attractions, coordinates, hotel prices, and estimated transport costs.
5. **Mathematical Optimization:** Calculates distances and finds efficient routes using Google OR-Tools.
6. **Budget Checker:** Calculates trip expenses and checks budget limits.
7. **LangGraph State:** Stores the current plan and previous changes.
8. **Human Approval:** Requires user confirmation before saving.
9. **Data Storage:** Saves approved plans in JSON format.

The agent will use conditional steps to decide whether to optimize the route again, suggest alternatives, or ask for approval.

The mathematical optimization will use a distance matrix to minimize total route distance for the selected locations.

The agent will have a maximum execution step limit to prevent infinite loops.

If the optimization tool cannot find a solution, the agent will show a clear message and suggest changing the travel requirements.

The system will not save any plan without user approval.

## Example Requests

### Example 1

"I want to visit Istanbul for five days. My budget is 400,000 KZT. Find interesting places and create an optimized travel route."

### Example 2

"I want to visit five tourist attractions in Almaty in one day. Find the shortest route between them."

### Example 3

"I want to travel to Dubai for four days. My budget is 600,000 KZT. Calculate my expenses and create a travel itinerary."

### Example 4

"My travel budget decreased by 100,000 KZT. Update my travel plan and suggest cheaper activities."

### Example 5

"I want to visit museums and historical places in Paris. Find suitable attractions and optimize my route."

## Evaluation Plan

The project will include at least 12 tests.

### Functional Tests

1. Load valid travel data from CSV files.
2. Find attractions for a selected destination.
3. Calculate distances correctly.
4. Generate a valid optimized route.
5. Compare the optimized route with a simple original route.
6. Calculate total travel costs correctly.
7. Detect a trip that fits the budget.
8. Detect a trip that exceeds the budget.
9. Create a daily travel itinerary.
10. Update the route after changes.
11. Process requests requiring multiple tools.

### Guardrail Tests

12. Reject negative budget values.
13. Reject invalid travel dates.
14. Handle missing or incorrect location data.
15. Prevent saving without user approval.
16. Handle optimization tool failures safely.
17. Stop when the maximum execution step limit is reached.
18. Reject instructions that try to bypass approval.

The evaluation will check:

- Correctness of mathematical calculations.
- Total route distance before and after optimization.
- Budget calculation accuracy.
- Correct tool selection.
- Itinerary quality.
- Safe handling of invalid requests and tool failures.

The project will save test results and agent tool execution traces.

## Weekly Milestones

### Week 10 — Proposal and Planning

- Choose the project topic and A level.
- Define the problem and target users.
- Select mathematical optimization methods.
- Plan functions and agent tools.
- Design the architecture.
- Complete the project proposal.

### Week 11 — Initial LLM Integration

- Set up Python and required libraries.
- Connect the OpenAI API.
- Create the first working LLM request.
- Test a simple travel planning question.

### Week 12 — Core Functionality

- Create sample travel datasets.
- Implement CSV data loading.
- Implement attraction search.
- Implement distance calculations.
- Add basic input validation and tests.

### Week 13 — Mathematical Optimization and Tools

- Implement route optimization using Google OR-Tools.
- Implement trip cost calculations.
- Register Python functions as agent tools.
- Test requests requiring several tools.

### Week 14 — Agent Workflow

- Build the multi-step LangGraph workflow.
- Add agent state and memory.
- Implement route and itinerary updates.
- Add stopping conditions.
- Select a recent research paper about AI agents.

### Week 15 — Reliability and Guardrails

- Add input and tool validation.
- Add maximum execution limits.
- Require user approval before saving plans.
- Handle missing data and optimization failures.
- Save execution traces.

### Week 16 — Evaluation and Improvement

- Run at least 12 tests.
- Compare optimized and original travel routes.
- Fix identified problems.
- Evaluate agent performance.
- Prepare the research paper presentation.
- Complete the evaluation report.

### Week 17 — Final Submission and Defense

- Complete the final project report.
- Update the README.
- Check the GitHub repository.
- Submit the final commit hash.
- Demonstrate the working AI Travel Route Optimization Agent.
- Explain the mathematical optimization, architecture, tools, and guardrails.


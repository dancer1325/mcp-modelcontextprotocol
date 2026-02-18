# Core Server Features
## Tools

* TODO: Tools enable AI applications to perform actions on behalf of users
* In a travel planning scenario, the AI application might use several tools to help book a vacation:

**Flight Search**

```
searchFlights(origin: "NYC", destination: "Barcelona", date: "2024-06-15")
```

Queries multiple airlines and returns structured flight options.

**Calendar Blocking**

```
createCalendarEvent(title: "Barcelona Trip", startDate: "2024-06-15", endDate: "2024-06-22")
```

Marks the travel dates in the user's calendar.

**Email notification**

```
sendEmail(to: "team@work.com", subject: "Out of Office", body: "...")
```

Sends an automated out-of-office message to colleagues.

## Resources

Continuing with the travel planning example, resources provide the AI application with access to relevant information:

- **Calendar data** (`calendar://events/2024`) - Checks user availability
- **Travel documents** (`file:///Documents/Travel/passport.pdf`) - Accesses important documents
- **Previous itineraries** (`trips://history/barcelona-2023`) - References past trips and preferences

The AI application retrieves these resources and decides how to process them, whether selecting a subset of data using embeddings or keyword search, or passing raw data directly to the model.

In this case, it provides calendar data, weather information, and travel preferences to the model, enabling it to check availability, look up weather patterns, and reference past travel preferences.

**Resource Template Examples:**

```json
{
  "uriTemplate": "weather://forecast/{city}/{date}",
  "name": "weather-forecast",
  "title": "Weather Forecast",
  "description": "Get weather forecast for any city and date",
  "mimeType": "application/json"
}

{
  "uriTemplate": "travel://flights/{origin}/{destination}",
  "name": "flight-search",
  "title": "Flight Search",
  "description": "Search available flights between cities",
  "mimeType": "application/json"
}
```

These templates enable flexible queries
* For weather data, users can access forecasts for any city/date combination
* For flights, they can search routes between any two airports
* When a user has input "NYC" as the `origin` airport and begins to input "Bar" as the `destination` airport, the system can suggest "Barcelona (BCN)" or "Barbados (BGI)".

## Prompts

Prompts provide structured templates for common tasks
* In the travel planning context:

**"Plan a vacation" prompt:**

```json
{
  "name": "plan-vacation",
  "title": "Plan a vacation",
  "description": "Guide through vacation planning process",
  "arguments": [
    { "name": "destination", "type": "string", "required": true },
    { "name": "duration", "type": "number", "description": "days" },
    { "name": "budget", "type": "number", "required": false },
    { "name": "interests", "type": "array", "items": { "type": "string" } }
  ]
}
```

Rather than unstructured natural language input, the prompt system enables:

1
* Selection of the "Plan a vacation" template
2
* Structured input: Barcelona, 7 days, $3000, ["beaches", "architecture", "food"]
3
* Consistent workflow execution based on the template

# Bringing Servers Together

Consider a personalized AI travel planner application, with three connected servers:

- **Travel Server** - Handles flights, hotels, and itineraries
- **Weather Server** - Provides climate data and forecasts
- **Calendar/Email Server** - Manages schedules and communications

* Complete Flow

1
* **User invokes a prompt with parameters:**

   ```json
   {
     "prompt": "plan-vacation",
     "arguments": {
       "destination": "Barcelona",
       "departure_date": "2024-06-15",
       "return_date": "2024-06-22",
       "budget": 3000,
       "travelers": 2
     }
   }
   ```

2
* **User selects resources to include:**
   - `calendar://my-calendar/June-2024` (from Calendar Server)
   - `travel://preferences/europe` (from Travel Server)
   - `travel://past-trips/Spain-2023` (from Travel Server)

3
* **AI processes the request using tools:**

   The AI first reads all selected resources to gather context - identifying available dates from the calendar, learning preferred airlines and hotel types from travel preferences, and discovering previously enjoyed locations from past trips.

   Using this context, the AI then executes a series of Tools:
   - `searchFlights()` - Queries airlines for NYC to Barcelona flights
   - `checkWeather()` - Retrieves climate forecasts for travel dates

   The AI then uses this information to create the booking and following steps, requesting approval from the user where necessary:
   - `bookHotel()` - Finds hotels within the specified budget
   - `createCalendarEvent()` - Adds the trip to the user's calendar
   - `sendEmail()` - Sends confirmation with trip details

**The result:** Through multiple MCP servers, the user researched and booked a Barcelona trip tailored to their schedule
* The "Plan a Vacation" prompt guided the AI to combine Resources (calendar availability and travel history) with Tools (searching flights, booking hotels, updating calendars) across different servers—gathering context and executing the booking
* A task that could have taken hours was completed in minutes using MCP.

 # RaceDay API (PROG6212 POE, Part 2)

API for RaceDay event management platform for road running, walking and cycling events in South Africa. Built with ASP.NET Core Web API (C#), Entity Framework Core (Code-First) and SQL Server.

## Roles
Organiser is able to manage their own events to create, update or delete their event, set categories for their event, see all the enrolments to their event, and record the finish times and positions within the event.
Participant can register and search events and categories, join an event in a category, see their own event that they are involved in, and see their results.

To secure specific parts of the API, it is secured using [Authorize(Roles = ...)]. Only events organised by author can be managed.

## Setup
1. Install the .This is for NET 8 ("LocalDB is acceptable") and SQL Server
2. Ensure the connection string is correct in RaceDay.Api/appsettings.json.
4. Open the Command Prompt and run the command “dotnet run –project RaceDay.Api”. The database is created and migrated when the application starts up.
4. Go into `/swagger`, make a POST request to `/api/auth/register`, then make a POST request to `/api/auth/login`. Then, swagger UI will send the session cookie.

## Run the tests
`dotnet test RaceDay.Tests/RaceDay.Tests.csproj`

The tests execute the real API in memory, using an in-memory database. They include Registration/Login, hashed password, Role rejection, Event management by Organiser etc., Recording Enrolment and Results.

## Endpoints
| Method | Route | Role |
|---|---|---|
| POST | /api/auth/register | Public |
| POST | /api/auth/login | Public |
| POST | /api/auth/logout | Any |
| GET, PUT | /api/profile | Any |
| GET | /api/events, /api/events/{id} | Any |
| POST | /api/events | Organiser |
The organiser (owner) PUT or DELETE methods will return the event data back to the caller.
| GET | /api/events/{eventId}/categories | Any |
Any user with the same owner as the event can update its categories.A user can update categories of an event, if the owner of the event is the same user.
PUT, DELETE | /api/categories/{id} | Organiser (owner)
| POST | /api/events/{eventId}/enrolments | Participant |
| GET | /api/enrolments/my | Participant |
/api/events/{eventId}/enrolments | Organiser (owner)
Returns a list of results from the entire enrolment.By Owner / POST /api/enrolments/{enrolmentId}/result, returns a list of results from the entire enrolment.
Retrieves results for an event.Retrieves the results for an event.
| GET | /api/results/my | Participant |

## CI/CD
The project is built and all of the unit tests are executed in each push using GitHub Actions [`.github/workflows/ci.yml`).

![CI green build](docs/ci-green-build.png)

## AI use
As per brief, briefly outline the use of AI tools (planning and generating code).

## Planning documents
ERD, endpoint plan and SQL script are in `/docs`.

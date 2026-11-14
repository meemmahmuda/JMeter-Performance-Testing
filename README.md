# JMeter Load Test – ToolsQA Web

A simple [Apache JMeter](https://jmeter.apache.org/) performance test plan that sends HTTP requests to [ToolsQA](https://www.toolsqa.com) and validates the responses. The repository also includes the HTML dashboard report generated from the test run.

## Project Structure

```

├── View_Results_Tree_Web.html/   
├── index.html                  # Generated JMeter HTML dashboard (entry point)
├── content/                    # Dashboard CSS, JS, and chart pages
├── sbadmin2-1.0.7/             # Dashboard theme/assets (Bootstrap, Flot, etc.)
```

## Test Plan Overview

| Item            | Value                                  |
| --------------- | -------------------------------------- |
| Tool            | Apache JMeter 5.6.3                    |
| Target          | `https://www.toolsqa.com`              |
| Threads (users) | 10                                     |
| Ramp-up period  | 1 second                               |
| Loop count      | 1                                      |
| On sample error | Continue                               |

### Scenarios (Transaction Controllers)

| Controller        | Method | Path                          |
| ----------------- | ------ | ----------------------------- |
| Home              | GET    | `/`                           |
| Selenium Training | GET    | `/selenium-training?q=headers`|
| About             | GET    | `/about`                      |

### Assertions

On the **Home** request:

- Response code is `200`
- Response message is `OK`

Every request also has a **View Results Tree** listener attached for inspecting results.

## Prerequisites

- Java 8 or higher
- [Apache JMeter 5.6.3](https://jmeter.apache.org/download_jmeter.cgi) (or newer)

## How to Run

### GUI mode (for debugging)

1. Open JMeter.
2. Go to **File → Open** and select `View_Results_Tree_Web.jmx`.
3. Click the green **Start** button and check results in **View Results Tree**.

### Non-GUI mode (recommended for real load tests)

```bash
jmeter -n -t View_Results_Tree_Web.jmx -l View_Results_Tree_Web.jtl
```

### Generate the HTML dashboard report

```bash
jmeter -n -t View_Results_Tree_Web.jmx -l View_Results_Tree_Web.jtl -e -o report
```

> Note: the output folder (`report`) must be empty or must not exist.

Or generate it from an existing `.jtl` file:

```bash
jmeter -g View_Results_Tree_Web.jtl -o report
```

## Viewing the Report

Open `index.html` in your browser. The dashboard includes:

- Test and report information (start/end time, source file)
- APDEX (Application Performance Index)
- Requests summary
- Statistics table
- Errors and Top 5 errors by sampler
- Charts: Over Time, Throughput, Response Times

The last run in this repo was executed on **14 Nov 2026**.

## Customizing

- **Change load:** edit *Number of Threads* and *Ramp-Up Period* in the Thread Group.
- **Add pages:** add a new Transaction/Simple Controller with an HTTP Request sampler.
- **Change target:** update the *Server Name* and *Path* in the HTTP Request samplers.

## Disclaimer

Only run load tests against systems you own or have permission to test. Keep the user count low when testing public websites.

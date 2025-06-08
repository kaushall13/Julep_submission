# Julep AI-Powered Foodie Tour Generator

## The Problem

Traditional food guides are static. They recommend outdoor dining during storms, hot soup on scorching days, and provide generic advice that ignores real-world conditions. Food tourism needs to adapt to the moment.

## Our Approach

We built an intelligent system that demonstrates the power of combining external data sources with AI workflows to create contextually aware experiences.

### 1. Custom Weather Integration Strategy

Rather than relying on pre-built weather integrations, we implemented a custom weather data pipeline using OpenWeatherMap's API. This deliberate choice allows us to:

- **Control data transformation**: We process raw weather data into contextually relevant insights ("18°C with light rain" becomes "perfect for cozy bistros")
- **Enhance prompts intelligently**: Weather data is enriched with dining implications before being passed to the AI agent
- **Maintain flexibility**: Custom integration means we can adapt the weather processing logic as our use case evolves

This approach showcases how external APIs can be seamlessly integrated into Julep workflows while maintaining full control over data processing and context enrichment.

### 2. Workflow Design Philosophy

The core insight is that the AI agent shouldn't just receive raw weather data - it should receive pre-processed, contextually relevant information. Our workflow architecture:

```python
workflow = {
    "input_schema": {
        "city": "string",
        "weather_info": "string"  # Pre-processed, context-rich weather data
    },
    "main": [{
        "prompt": "Create a tour for {{_.city}} considering {{_.weather_info}}"
    }]
}
```

This design pattern - external data enrichment before AI processing - proves more effective than having the AI interpret raw weather data within the workflow.

### 3. Agent Persona Engineering

We crafted a sophisticated system prompt that positions the AI as a "world-class food tourism expert" who naturally considers weather conditions. This persona engineering ensures consistent, high-quality outputs that feel authentic rather than algorithmic.

### 4. Structured Output Architecture

The workflow generates consistently formatted responses with clear sections (breakfast, lunch, dinner, cultural context, weather considerations). This structured approach makes the output immediately actionable for users.

## Technical Implementation

### Core Architecture
```python
def generate_foodie_tour(city):
    # Custom weather processing
    weather_info = get_current_weather(city)
    
    # Julep workflow orchestration
    workflow = create_foodie_workflow()
    
    # Execution with enriched context
    execution = client.executions.create(
        task_id=task_id,
        input={"city": city, "weather_info": weather_info}
    )
```

### Key Design Decisions

**Custom Weather Processing**: We process weather data into dining-relevant insights before passing to the AI agent, rather than asking the agent to interpret raw meteorological data.

**Workflow Simplicity**: The Julep workflow itself is intentionally simple - a single prompt step. The complexity lies in the data preparation and prompt engineering, not workflow orchestration.

**Error Handling**: Comprehensive error handling ensures graceful degradation when weather APIs are unavailable, maintaining core functionality.

## Why This Approach Works

### 1. Context Enrichment
By processing external data before AI consumption, we ensure the agent receives optimally formatted context rather than raw data that requires interpretation.

### 2. Separation of Concerns
Weather processing logic is separate from AI workflow logic, making the system more maintainable and testable.

### 3. Workflow Efficiency
Simple workflows with rich inputs are more reliable and faster than complex workflows that handle data processing internally.

### 4. Scalability Pattern
This pattern (external data enrichment + focused AI workflows) scales well to additional data sources like restaurant APIs, event data, or user preferences.

## Results

The system generates comprehensive, weather-aware food tours that feel personally crafted rather than algorithmically generated. Users receive actionable itineraries with specific restaurant recommendations, cultural context, and practical considerations based on real-time conditions.

## Technical Insights

This project demonstrates several key patterns for building production-ready AI applications:

- **External API integration** with proper error handling and fallbacks
- **Context enrichment** before AI processing for better results
- **Structured output** through careful prompt engineering
- **Workflow simplicity** combined with sophisticated data preparation

The approach shows how Julep's workflow orchestration can be the foundation for complex, context-aware applications when combined with thoughtful system design.

## Installation

```bash
pip install julep requests
```

Set environment variables:
```bash
export JULEP_API_KEY="your_key"
export OPENWEATHER_API_KEY="your_key"
export AGENT_UUID="your_agent_id"
```

Run:
```bash
python foodie_generator.py
```

## Architecture Philosophy

The key insight is that AI workflows should focus on what AI does best - creative synthesis and structured output - while delegating data processing and context enrichment to specialized functions. This separation creates more reliable, maintainable, and scalable systems.

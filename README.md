# FastAPI Chatbot with Monitoring

A FastAPI application that provides a chatbot interface powered by Ollama LLM with comprehensive monitoring using Prometheus metrics.

## Features

- **AI-Powered Chat Interface**: Uses Ollama LLM (Llama 3.2 1B model) to respond to user questions
- **Conversation Context**: Maintains conversation history for contextual responses
- **Redis Caching**: Caches responses for improved performance
- **Prometheus Metrics**: Comprehensive monitoring of:
  - Request counts and latency
  - CPU and memory usage
  - Error tracking
  - Active request monitoring
- **Logging**: Detailed logging for debugging and performance tracking

## Requirements

- Python 3.9+
- Redis server
- Ollama with Llama 3.2 1B model installed
- Docker (optional, for containerization)

## Installation

### Poetry Setup (Recommended)

This project uses Poetry for dependency management:

1. Clone this repository:
   ```
   git clone https://github.com/sattensil/FastAPIChatbotWithPrometheus.git
   cd FastAPIChatbotWithPrometheus
   ```

2. Install dependencies with Poetry:
   ```
   # If you don't have Poetry installed
   curl -sSL https://install.python-poetry.org | python3 -
   
   # Install dependencies
   poetry install
   
   # Activate the virtual environment
   poetry env use python
   poetry env activate
   ```

### Alternative: Pip Installation

1. Clone this repository:
   ```
   git clone https://github.com/sattensil/FastAPIChatbotWithPrometheus.git
   cd FastAPIChatbotWithPrometheus
   ```

2. Install dependencies:
   ```
   pip install -r requirements.txt
   ```

3. Make sure Redis is running:
   ```
   redis-server
   ```

4. Make sure Ollama is installed and the Llama 3.2 1B model is available:
   ```
   ollama pull llama3.2:1b
   ```

5. Start the application:
   ```
   # If using Poetry
   poetry run uvicorn main:app --reload
   
   # If using pip
   uvicorn main:app --reload
   ```

### Docker Setup

1. Build the Docker image:
   ```
   docker build -t my-fastapi-app .
   ```

2. Run the container:
   ```
   docker run -p 8000:8000 my-fastapi-app
   ```

### Docker Compose Setup (Recommended)

This setup includes FastAPI, Redis, Prometheus, and Grafana all configured and ready to use:

1. Start all services:
   ```
   docker-compose up
   ```

2. Access the services:
   - Chat Interface: http://localhost:8000
   - Prometheus: http://localhost:9090
   - Grafana: http://localhost:3000 (username: admin, password: admin)
   - API Documentation: http://localhost:8000/docs

## API Endpoints

- **POST /chat**: Send a question to the chatbot
  ```json
  {
    "question": "Your question here"
  }
  ```

- **GET /metrics**: Prometheus metrics endpoint for monitoring

## Monitoring

The application exposes various Prometheus metrics at the `/metrics` endpoint:

- `apiserver_request_total`: Total number of requests
- `apiserver_request_latency_seconds`: Latency of requests in seconds
- `apiserver_request_errors_total`: Total number of errors
- `apiserver_active_requests`: Number of active requests
- `apiserver_request_duration_seconds_bucket`: Duration of API server requests
- `node_cpu_usage_percent`: Total CPU usage percentage
- `node_memory_total_bytes`: Total memory in bytes
- `node_memory_free_bytes`: Free memory in bytes
- And many more system metrics

### Standalone Monitoring Setup

This project includes a complete monitoring stack setup with Prometheus and Grafana:

#### Setting up Prometheus

1. Install Prometheus (if not already installed):
   ```bash
   brew install prometheus
   ```

2. Use the provided configuration file:
   ```bash
   prometheus --config.file=prometheus_local.yml
   ```

3. Access Prometheus at: http://localhost:9090

#### Setting up Grafana

1. Install Grafana (if not already installed):
   ```bash
   brew install grafana
   ```

2. Start Grafana:
   ```bash
   brew services start grafana
   ```

3. Access Grafana at: http://localhost:3000 (default credentials: admin/admin)

4. Add Prometheus as a data source:
   - URL: http://localhost:9090
   - Name: prometheus

5. Import the provided dashboard:
   - Go to Dashboards > Import
   - Upload the `fastapi_dashboard.json` file

This setup provides comprehensive visualization of all metrics collected by your FastAPI application.

## Troubleshooting

### Common Issues

1. **Redis Connection Error**:
   - Ensure Redis is running on localhost:6379
   - Check for error messages in the logs

2. **Ollama Model Not Found**:
   - Verify the model is installed with `ollama list`
   - Pull the model if needed: `ollama pull llama3.2:1b`

3. **Port Already in Use**:
   - Change the port with `uvicorn main:app --reload --port 8001`

4. **Import Errors**:
   - Ensure all dependencies are installed correctly
   - Try reinstalling with `pip install -r requirements.txt`

## Customizing the Chatbot

The chatbot currently has a fun circus expert personality! You can customize its personality and behavior by modifying the prompt templates in `main.py`:

### Changing the Personality

1. Open `main.py` and locate the template definitions (around line 144)
2. Modify the instructions in both `template` and `template_new` variables
3. Update the welcome message in `static/index.html` to match your new theme

### Current Personality: Circus Expert

The chatbot is currently configured as "The Amazing Circusbot" with:
- Expertise in circus arts, performances, history, and culture
- A vibrant, playful personality with circus flair
- Circus-themed language and expressions
- Occasional circus facts and historical information

### Example Personality Templates

You can replace the current template with any of these examples or create your own:

#### Space Explorer
```python
template = """
You are Cosmo, an interstellar explorer with knowledge of the cosmos, space travel, and alien worlds.
Use astronomy terminology and space metaphors in your responses.
...
"""
```

#### Chef Bot
```python
template = """
You are Chef Byte, a master culinary expert with knowledge of global cuisines and cooking techniques.
Sprinkle in cooking terminology and food puns in your responses.
...
"""
```

## Project Structure

- `main.py`: Main FastAPI application with routes and middleware
- `requirements.txt`: Project dependencies
- `Dockerfile`: Docker configuration for containerization
- `setup_model.ipynb`: Notebook for model setup and testing
- `static/index.html`: Web interface for the chatbot

## Usage Examples

### Web Interface

The application includes a user-friendly web interface for chatting with the bot:

1. Access the web interface at: http://localhost:8000
2. Type your question in the input field
3. Press Enter or click the Send button
4. View the bot's response in the chat window

The web interface also includes convenient links to all monitoring dashboards.

### API Interaction

```bash
curl -X POST "http://127.0.0.1:8000/chat" \
  -H "Content-Type: application/json" \
  -d '{"question":"What can you tell me about FastAPI?"}'
```

### Viewing Metrics

Open your browser and navigate to:
```
http://127.0.0.1:8000/metrics
```

Or use curl:
```bash
curl http://127.0.0.1:8000/metrics
```

## Monitoring Stack

This project includes a complete monitoring stack with Prometheus and Grafana:

### Prometheus

Prometheus collects metrics from your FastAPI application:

- Access the Prometheus UI at: http://localhost:9090
- Query metrics using PromQL
- View targets and their health

### Grafana

Grafana provides visualizations for your metrics:

- Access Grafana at: http://localhost:3000 (default credentials: admin/admin)
- Pre-configured dashboard for FastAPI metrics
- Panels for:
  - Request counts and latency
  - CPU and memory usage
  - Error tracking
  - Active request monitoring

### Dashboard Features

The pre-configured Grafana dashboard includes:

1. **Request Metrics**:
   - Total request count
   - Request latency (95th and 50th percentiles)
   - Active requests gauge

2. **System Metrics**:
   - CPU usage percentage
   - Memory usage over time
   - Memory allocation breakdown

3. **Error Tracking**:
   - Error count over time
   - API probe duration

All metrics are updated in real-time with a 5-second refresh interval.

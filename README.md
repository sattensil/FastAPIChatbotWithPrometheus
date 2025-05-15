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

### Local Setup

1. Clone this repository:
   ```
   git clone <repository-url>
   cd my_fast_api_app
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

You can use Prometheus to scrape these metrics and Grafana for visualization.

## Project Structure

- `main.py`: Main FastAPI application with routes and middleware
- `requirements.txt`: Project dependencies
- `Dockerfile`: Docker configuration for containerization
- `setup_model.ipynb`: Notebook for model setup and testing

## License

[Your License Here]

## Contributing

[Your Contribution Guidelines Here]

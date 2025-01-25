# 1min-Relay
Relay 1min AI API to OpenAI Structure in 1 minute.

Don't forget to star this repository if you like it! 

## Features
- **bolt.diy Support**: Compatible with bolt.diy for seamless integration.
- **Conversation History**: Retain and manage conversation history effortlessly.
- **Text File Upload**: Supports text file uploads from select clients.
- **Broad Client Compatibility**: Works with most clients supporting OpenAI Custom Endpoint.
- **Fast and Reliable Relay**: Relays 1min AI API to OpenAI-compatible structure within 1 minute.
- **User-Friendly**: Easy to set up and quick to use.
- **Model Exposure Control**: Expose all or a predefined subset of 1min.ai-supported models.
- **Streaming Support**: Enables real-time streaming for faster interactions.
- **Non-Streaming Support**: Compatible with non-streaming workflows.
- **Docker Support**: Deploy easily with Docker for fast and consistent setup.
- **Document Support**: Supports document uploads for enhanced functionality. Some include .docx, .pdf, .txt, .yaml and etc.
- **Image Upload**: Supports image uploads for enhanced functionality.


## Installation:

Clone the git repo into your machine.

### Bare-metal

To install dependencies, run:
```bash
pip install -r requirements.txt
```

and then run python3 main.py

Depending on your system, you may need to run python

### Docker

#### Pre-Built images

1. Pull the Docker Image:
```bash
docker pull kokofixcomputers/1min-relay:latest
```

2. To encrease security, 1min-relay will require it's own network to be able to communicate with memcached.
To create a network, run:
```bash
docker network create 1min-relay-network
```

3. Run Memcached.
```bash
docker run -d --name memcached --network 1min-relay-network memcached
```

4. Run the 1min-relay Container:
```bash
docker run -d --name 1min-relay-container --network 1min-relay-network -p 5001:5001 \
  -e SUBSET_OF_ONE_MIN_PERMITTED_MODELS="mistral-nemo,gpt-4o-mini,deepseek-chat" \
  -e PERMIT_MODELS_FROM_SUBSET_ONLY=True \
  kokofixcomputers/1min-relay:latest
```
Environment Variables:

- `SUBSET_OF_ONE_MIN_PERMITTED_MODELS`: Specifies a subset of 1min.ai models to expose. Default: mistral-nemo,gpt-4o,deepseek-chat.
- `PERMIT_MODELS_FROM_SUBSET_ONLY`: Restricts model usage to the specified subset. Set to True to enforce this restriction or False to allow all models supported by 1min.ai. Default: True.


#### Self-Build

1. Build the Docker Image
From the project directory (where Dockerfile and main.py reside), run:

```bash
docker build -t 1min-relay:latest .
```

2. To encrease security, 1min-relay will require it's own network to be able to communicate with memcached.
To create a network, run:
```bash
docker network create 1min-relay-network
```

3. Run Memcached.
```bash
docker run -d --name memcached --network 1min-relay-network memcached
```

4. Run the 1min-relay Container:
```bash
docker run -d --name 1min-relay-container --network 1min-relay-network -p 5001:5001 \
  -e SUBSET_OF_ONE_MIN_PERMITTED_MODELS="mistral-nemo,gpt-4o-mini,deepseek-chat" \
  -e PERMIT_MODELS_FROM_SUBSET_ONLY=True \
  1min-relay:latest
```

- `-d` runs the container in detached (background) mode.
- `-p 5001:5001` maps your host’s port 5001 to the container’s port 5001.
- `--name 1min-relay-container` is optional, but it makes it easier to stop or remove the container later.
- `-e`: Specifies environment variables.
- `SUBSET_OF_ONE_MIN_PERMITTED_MODELS`: Specifies a subset of 1min.ai models to expose. Default: mistral-nemo,gpt-4o,deepseek-chat.
- `PERMIT_MODELS_FROM_SUBSET_ONLY`: Restricts model usage to the specified subset. Set to True to enforce this restriction or False to allow all models supported by 1min.ai. Default: False.


4. Verify It’s Running
Check logs (optional):

```bash
docker logs -f 1min-relay-container
```
You should see your Flask server output: “Server is ready to serve at …”

Test from your host machine:

```bash
curl -X GET http://localhost:5001/v1/models
```

5. Stopping or Removing the Container
To stop the container:

```bash
docker stop 1min-relay-container
```

To remove the container:

```bash
docker rm 1min-relay-container
```

To remove the image entirely:

```bash
docker rmi 1min-relay:latest
```

Optional: Run with Docker Compose
If you prefer Docker Compose, you can run the docker compose included with the repo:

Just run:

```bash
docker compose up -d
```
Compose will automatically do these things for you:
- Create a network
- Run Memcached
- Run the 1min-relay Container

### Environment Variables
- `SUBSET_OF_ONE_MIN_PERMITTED_MODELS`: Specifies a subset of 1min.ai models to expose. Default: mistral-nemo,gpt-4o,deepseek-chat.
- `PERMIT_MODELS_FROM_SUBSET_ONLY`: Restricts model usage to the specified subset. Set to True to enforce this restriction or False to allow all models supported by 1min.ai. Default: False.
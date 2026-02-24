# OpenClaw Agent Container
# Isolated AI agent with workspace, memory, and LLM connectivity

FROM node:20-alpine

# Install dependencies
RUN apk add --no-cache git curl bash

# Install OpenClaw globally
RUN npm install -g openclaw

# Create directory structure
RUN mkdir -p /workspace/memory /config

# Create default workspace files
COPY workspace/ /workspace/

# Set workspace as home
WORKDIR /workspace

# Environment defaults (overridable at runtime)
ENV AGENT_NAME=claw-agent \
    LLM_PROVIDER=openai \
    LLM_MODEL=gpt-4o \
    OPENCLAW_WORKSPACE=/workspace \
    OPENCLAW_CONFIG_DIR=/config

# Health check - verify OpenClaw is responsive
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD openclaw --version || exit 1

# Default command - run agent in embedded mode
# The container receives messages via AndOR Bridge or API calls
CMD ["openclaw", "agent", "--local"]

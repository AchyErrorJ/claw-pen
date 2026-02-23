# OpenClaw Agent Container
# Pre-configured for Claw Pen orchestration

FROM node:20-alpine

# Install dependencies
RUN apk add --no-cache git curl

# Install OpenClaw
RUN npm install -g openclaw

# Create workspace
RUN mkdir -p /workspace /config
WORKDIR /workspace

# Default config
COPY agent-config.yaml /config/

# Health check
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

# Expose OpenClaw port
EXPOSE 3000

# Start OpenClaw
CMD ["openclaw", "start", "--config", "/config/agent-config.yaml"]

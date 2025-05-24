# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

**Running the Application:**
```bash
uvicorn app.main:app --reload
```

**Testing:**
```bash
# Run all tests with Allure reporting
pytest --alluredir=allure-results -v -s

# Run tests in specific environment
./tests/utils/test_runners/run_all_tests.sh -e dev

# Run specific test groups
./tests/utils/test_runners/run_by_group.sh -g "API Tests"

# Run specific test file
./tests/utils/test_runners/run_by_file.sh -f tests/api/v1/test_hello.py

# View Allure test reports
allure serve allure-results
```

**Dependency Management:**
```bash
# Install dependencies
uv pip install -r requirements.txt

# Update requirements.txt from requirements.in
uv pip compile requirements.in
```

**Deployment:**
```bash
# Railway deployment
./deploy-railway.sh

# Docker build
docker build -f Dockerfile.railway -t genai-api .
```

## Architecture Overview

**FastAPI Application Structure:**
- Main app in `app/main.py` with "GenAI API" title
- Versioned API routing (`/api/v1/`) with modular includes
- Health check endpoint at `/health`
- AI service integrations for OpenAI and ElevenLabs

**Configuration Management:**
- Pydantic Settings-based configuration in `app/core/config.py`
- Environment variable support via `.env` files
- Environment-specific test configurations in `tests/config/environments/`

**Service Architecture:**
- `ImageService` for OpenAI image generation (model: "gpt-image-1")
- `VoiceService` for ElevenLabs voice synthesis (model: "eleven_multilingual_v2")
- Structured output management in `/output/` directory

## Testing Framework

**Pytest Configuration:**
- Allure reporting enabled by default
- Custom markers: `api`, `integration`, `slow`
- Environment-specific test execution (dev/uat/prod)
- FastAPI server fixtures in `conftest.py`

**Test Organization:**
```
tests/
├── ai-tests/           # AI service integration tests
├── api/v1/            # API endpoint tests
├── config/            # Environment configurations
├── utils/test_runners/ # Shell scripts for test execution
└── conftest.py        # Central fixture management
```

**Testing Patterns:**
- Use Allure decorators for test organization (`@allure.epic`, `@allure.feature`)
- HTTP session fixtures for API testing
- Automatic server startup/shutdown for integration tests
- Environment properties injection for test reports

## AI Services Integration

**OpenAI Integration:**
- Image generation with base64 handling
- API key management through settings
- Error handling and connection testing
- File output to `/output/images/`

**ElevenLabs Integration:**
- Voice synthesis with configurable settings
- MP3 audio output to `/output/audio/`
- Voice listing and model management

## Deployment Configuration

**Railway Deployment:**
- Health check integration at `/health` endpoint
- Environment variable support for PORT configuration
- Docker configuration in `Dockerfile.railway`
- Railway configuration in `railway.json`

**Environment Variables:**
- `OPENAI_API_KEY` - Required for image generation
- `ELEVENLABS_API_KEY` - Required for voice synthesis
- `PORT` - Application port (defaults to 8000)

## Development Guidelines

**Pydantic V2 Usage:**
- Use `BaseSettings` for configuration classes
- Proper optional field handling with `Optional[bool]` for nullable responses
- Environment variable integration with `Field` aliases
- Validation decorators for complex fields

**Code Organization:**
- Modular service architecture with clear separation
- API versioning with prefix routing
- Comprehensive error handling in services
- Structured output directory management
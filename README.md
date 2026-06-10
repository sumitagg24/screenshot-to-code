# Screenshot to Code

A simple tool to convert screenshots, mockups and Figma designs into clean, functional code using AI. Now supporting Gemini 3 and Claude Opus 4.5!

<p align="center">
  <a href="https://github.com/abi/lilo">
    <img src="https://raw.githubusercontent.com/abi/lilo/main/frontend/public/readme-logo.png" alt="Sponsored by Lilo" width="180" />
  </a>
  <br />
  Sponsored by <a href="https://github.com/abi/lilo">Lilo</a>
</p>

## Features

### Supported Stacks
- HTML + Tailwind
- HTML + CSS
- React + Tailwind
- Vue + Tailwind
- Bootstrap
- Ionic + Tailwind
- SVG

### Supported AI Models
- **Gemini 3 Flash and Pro** - Best models! (Google)
- **Claude Opus 4.5** - Best model! (Anthropic)
- GPT-4 variants (OpenAI)
- Other models available (less recommended)
- DALL-E 3 or Flux Schnell for image generation (via Replicate)

### Advanced Capabilities
- Video/screen recording to functional prototype conversion (experimental)
- Multi-variant code generation for comparison
- Real-time WebSocket-based generation feedback
- Custom code modifications and iterative refinement

## Getting Started

### Prerequisites
- Node.js and Yarn (frontend)
- Python 3.10+ and Poetry (backend)
- At least one AI provider API key (OpenAI, Anthropic, or Google Gemini)

### Backend Setup

1. Navigate to the backend directory:
```bash
cd backend
```

2. Install Poetry if you haven't already:
```bash
pip install --upgrade poetry
```

3. Create `.env` file with your API keys:
```bash
echo "OPENAI_API_KEY=sk-your-key" > .env
echo "ANTHROPIC_API_KEY=your-key" >> .env
echo "GEMINI_API_KEY=your-key" >> .env
```

4. Install dependencies and activate the Poetry virtual environment:
```bash
poetry install
cd backend && poetry env activate
# Run the printed command, e.g. source /path/to/venv/bin/activate
```

5. Start the backend server:
```bash
poetry run uvicorn main:app --reload --port 7001
```

### Frontend Setup

1. Navigate to the frontend directory:
```bash
cd frontend
```

2. Install dependencies:
```bash
yarn
```

3. Configure backend URL (optional, if running on different port):
```bash
# Edit frontend/.env.local
VITE_WS_BACKEND_URL=ws://localhost:7001/ws
VITE_HTTP_BACKEND_URL=http://localhost:7001
```

4. Start the development server:
```bash
yarn dev
```

5. Open http://localhost:5173 in your browser

### Using the Hosted Version

[Try the hosted version on screenshottocode.com](https://screenshottocode.com) (paid subscription required)

## Docker Setup

For quick deployment with Docker:

1. Create `.env` file in root directory:
```bash
echo "OPENAI_API_KEY=sk-your-key" > .env
```

2. Build and run with Docker Compose:
```bash
docker-compose up -d --build
```

3. Access the app at http://localhost:5173

**Note:** Docker setup is for production-like deployment. Development with Docker won't hot-reload code changes.

## Development

### Project Structure

```
screenshot-to-code/
├── backend/                    # FastAPI Python backend
│   ├── agent/                 # AI provider abstractions
│   ├── routes/                # API endpoints
│   ├── services/              # Business logic
│   ├── tests/                 # Backend tests
│   ├── main.py               # FastAPI application
│   └── pyproject.toml        # Python dependencies
├── frontend/                   # React + Vite frontend
│   ├── src/
│   │   ├── components/       # React components
│   │   ├── App.tsx           # Main app component
│   │   └── main.tsx          # Entry point
│   └── package.json          # Node dependencies
├── docker-compose.yml         # Docker configuration
└── README.md                  # This file
```

### Running Tests

#### Backend Tests
```bash
cd backend
poetry run pytest                    # Run all tests
poetry run pytest -vv               # Verbose output
poetry run pytest tests/test_*.py   # Run specific tests
poetry run pytest --cov=routes      # Coverage report
```

#### Type Checking
```bash
cd backend
poetry run pyright                   # Run type checker
```

#### Frontend Linting
```bash
cd frontend
yarn lint                            # Run ESLint
```

### Common Workflows

#### Run all backend tests and type checking after changes:
```bash
cd backend
poetry run pytest && poetry run pyright
```

#### Debug backend
Use the Poetry virtual environment:
```bash
cd backend
poetry env activate
# Then use your debugger or print statements
```

#### View prompt structure
```python
from utils import print_prompt_summary
print_prompt_summary(prompt_messages)
```

## Configuration

### API Keys

You can set API keys in two ways:

1. **Environment Variables** (in `backend/.env`):
```bash
OPENAI_API_KEY=sk-your-key
ANTHROPIC_API_KEY=your-key
GEMINI_API_KEY=your-key
REPLICATE_API_TOKEN=your-token  # For image generation
```

2. **UI Settings** (Settings dialog in frontend - click the gear icon):
- Keys are stored only in your browser, never sent to our servers
- Useful for testing different API keys without restarting

### OpenAI Proxy Configuration

If you can't access OpenAI API directly (country restrictions, etc.):

1. Set `OPENAI_BASE_URL` in `backend/.env`:
```bash
OPENAI_BASE_URL=https://xxx.xxxxx.xxx/v1
```

2. Or configure in UI settings

**Important:** The URL must include `/v1` in the path.

### Frontend Backend Connection

To connect frontend to a different backend:

Edit `frontend/.env.local`:
```bash
VITE_HTTP_BACKEND_URL=http://124.10.20.1:7001
VITE_WS_BACKEND_URL=ws://124.10.20.1:7001/ws
```

## Troubleshooting

### Getting OpenAI API Key with GPT-4 Access

1. Open [OpenAI Dashboard](https://platform.openai.com/)
2. Go to Settings > Billing
3. Add payment method (minimum $5 credit)
4. Check Settings > Limits - ensure you're "Tier 1" or higher for GPT-4 access
5. Navigate to [API Keys](https://platform.openai.com/api-keys) and create new secret key
6. Paste key in Settings dialog (gear icon) - stored only in browser

**Note:** It can take up to 30 minutes for GPT-4 access to activate after adding credits.

### UTF-8 Encoding Errors on Windows

On Windows, if you see UTF-8 errors when running backend:
1. Open `.env` file with Notepad++
2. Go to Encoding menu
3. Select "UTF-8" encoding

### Backend Setup Issues

If you encounter errors during setup:
1. Ensure Python 3.10+ is installed: `python --version`
2. Clear Poetry cache: `poetry cache clear . --all`
3. Reinstall dependencies: `poetry install --no-cache`
4. Check Poetry is using correct Python: `poetry env info`

### UTF-8 Errors When Running Backend

Some Windows systems require explicit UTF-8 encoding:
```bash
# In backend/.env, ensure file is saved as UTF-8
```

## API Documentation

### WebSocket Endpoint

The backend provides real-time code generation updates via WebSocket:

- **Endpoint:** `ws://localhost:7001/ws`
- **Protocol:** JSON-based messages with variant tracking
- **Features:** Real-time generation feedback, multi-variant support, cancellation

### REST Endpoints

Key endpoints available:

- `POST /api/generate-code` - Generate code from screenshot
- `POST /api/update-code` - Update existing code
- `POST /api/generate-image` - Generate images using DALL-E/Flux

See backend routes for complete API documentation.

## Video to Code (Experimental)

Convert screen recordings into functional prototypes:

[Learn more about video conversion](https://github.com/abi/screenshot-to-code/wiki/Screen-Recording-to-Code)

## Project Development Guidelines

### Code Style & Quality

- **Backend:**
  - Type checking required: `poetry run pyright` (no new warnings)
  - Format: Use triple-quoted strings for multi-line prompts
  - Always run tests after changes: `poetry run pytest`

- **Frontend:**
  - Lint required: `yarn lint` before committing
  - Use TypeScript for new components
  - Follow React best practices

### Testing Policy

- Always run backend tests after code changes: `cd backend && poetry run pytest`
- Always run type checking: `cd backend && poetry run pyright`
- No new type warnings allowed in modified files

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run tests and linting
5. Submit a pull request

For feedback, feature requests, or bug reports: [Open an issue](https://github.com/abi/screenshot-to-code/issues) or [ping on Twitter](https://twitter.com/_abi_)

## License

See LICENSE file for details.

## Hosted Version

The hosted version (`hosted` branch) connects to a separate SaaS backend codebase. For details, see the [hosted branch](https://github.com/abi/screenshot-to-code/tree/hosted).

## Troubleshooting & FAQs

### Q: I'm running into errors when setting up the backend
**A:** See [GitHub issue #3](https://github.com/abi/screenshot-to-code/issues/3#issuecomment-1814777959) for common solutions.

### Q: How do I get an OpenAI API key?
**A:** See the "Getting OpenAI API Key" section above.

### Q: Can I use a different port for the backend?
**A:** Yes, update `VITE_WS_BACKEND_URL` and `VITE_HTTP_BACKEND_URL` in `frontend/.env.local`.

### Q: How can I run with Ollama (open source models)?
**A:** [See this comment](https://github.com/abi/screenshot-to-code/issues/354#issuecomment-2435479853) for Ollama setup instructions.

### Q: How do I know if I have GPT-4 access?
**A:** Check your OpenAI account at Settings > Limits - you need to be Tier 1 or higher.

## Additional Resources

- [Wiki](https://github.com/abi/screenshot-to-code/wiki) - Detailed guides
- [Issues](https://github.com/abi/screenshot-to-code/issues) - Bug reports and feature requests
- [Twitter](https://twitter.com/_abi_) - Updates and announcements

---

**Video Example:**

https://github.com/user-attachments/assets/85b911c0-efea-4957-badb-daa97ec402ad

**Examples:**

**NYTimes:**
- Original: [Link](https://github.com/user-attachments/assets/6b0ae86c-1b0f-4598-a578-c7b62205b3e2)
- Replica: [Link](https://github.com/user-attachments/assets/981c490e-9be6-407e-8e46-2642f0ca613e)

**Instagram:**
https://github.com/user-attachments/assets/a335a105-f9cc-40e6-ac6b-64e5390bfc21

**Hacker News:**
https://github.com/user-attachments/assets/205cb5c7-9c3c-438d-acd4-26dfe6e077e5

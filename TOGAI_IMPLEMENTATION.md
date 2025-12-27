# Togai API Implementation

This document describes the implementation of Togai API support in TavernAI.

## Overview

Togai is an OpenAI-compatible API backend that has been integrated as a new API option in TavernAI. Users can now select "Togai" from the API dropdown and connect to Togai's chat completion endpoints.

## Features

- Full chat completion support using OpenAI-compatible format
- Configurable API URL (default: https://api.togai.ai/v1)
- API key authentication
- Model selection (gpt-4o, gpt-4o-mini, gpt-3.5-turbo)
- Temperature control (0.0 - 2.0)
- Token generation control (16 - 2048 tokens)
- Adjustable context window (default: 8192 tokens)

## Implementation Details

### Backend (server.js)

1. **API Constants**
   - `api_togai`: Default API URL
   - `api_key_togai`: Stores the API key
   - `api_url_togai`: Stores the configured API URL

2. **Endpoints**
   - `/getstatus_togai`: Checks connection status and retrieves available models
   - `/generate_togai`: Generates text completions using the chat/completions endpoint

3. **Request Format**
   - Uses OpenAI-compatible chat completion format
   - Supports system, user, and assistant messages
   - Includes parameters: model, max_tokens, temperature, top_p, presence_penalty, frequency_penalty, stop

### Frontend (public/script.js)

1. **Configuration Variables**
   - `api_url_togai`: API endpoint URL
   - `api_key_togai`: Authentication key
   - `model_togai`: Selected model
   - `temp_togai`: Temperature setting
   - `top_p_togai`: Top-p sampling
   - `pres_pen_togai`: Presence penalty
   - `freq_pen_togai`: Frequency penalty
   - `amount_gen_togai`: Maximum tokens to generate
   - `max_context_togai`: Context window size

2. **Functions**
   - `getStatusTogai()`: Polls the status endpoint
   - `resultCheckStatusTogai()`: Processes status check results
   - API button handler for connecting to Togai

3. **Integration Points**
   - Added to API selection dropdown
   - Integrated into message generation pipeline
   - Context size calculation
   - Settings persistence

### Frontend UI (public/index.html)

1. **API Selection**
   - Added "Togai" option to the main API dropdown

2. **Configuration Panel**
   - API URL input field
   - API Key input field (password type for security)
   - Model selection dropdown
   - Temperature slider
   - Token generation slider
   - Connection status indicator

## Usage

1. Select "Togai" from the API dropdown menu
2. Enter your Togai API URL (or use the default)
3. Enter your Togai API key
4. Click "Connect" to establish connection
5. Select your desired model from the dropdown
6. Adjust temperature and token settings as needed
7. Start chatting!

## Configuration

The implementation supports the following configuration options:

- **API URL**: Custom endpoint URL (default: https://api.togai.ai/v1)
- **API Key**: Your Togai authentication key
- **Model**: Choose from available models
- **Temperature**: Control randomness (0.0 = deterministic, 2.0 = very random)
- **Max Tokens**: Control response length (16 - 2048)
- **Context Size**: Maximum conversation history (default: 8192 tokens)

## Technical Notes

- Togai uses the same chat completion format as OpenAI
- The implementation follows the same pattern as other API integrations (OpenAI, Claude, Ollama)
- Settings are persisted across sessions
- Connection status is polled every 5 seconds when active
- Error handling includes network errors, authentication failures, and rate limiting

## Error Handling

The implementation handles the following error scenarios:

- 401: Invalid Authentication
- 404: Model not found
- 429: Rate limit reached
- 500: Server error
- Network errors: Connection timeouts or failures

## Future Enhancements

Potential improvements for future versions:

- Dynamic model list loading from the API
- Streaming support for real-time responses
- Advanced parameter controls (logit bias, etc.)
- Custom stop sequences configuration
- Token usage tracking and display

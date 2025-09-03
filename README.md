# OpenAI Compatible API Proxy for Z.ai GLM-4.5

This is a proxy server that provides OpenAI-compatible API interface for Z.ai GLM-4.5 model. It allows you to interact with Z.ai's GLM-4.5 model using standard OpenAI API format, supporting both streaming and non-streaming responses.

> **Note**: This project is forked from [OpenAI-Compatible-API-Proxy-for-Z](https://github.com/kbykb/OpenAI-Compatible-API-Proxy-for-Z) with additional development


## ✨ Main Features

- 🔄 **OpenAI API Compatible**: Fully compatible with OpenAI API format, no client code modification needed
- 🌊 **Streaming Response Support**: Supports real-time streaming output for better user experience
- 🔐 **Authentication**: Supports API key validation for service security
- 🛠️ **Flexible Configuration**: Flexible configuration through environment variables
- 🐳 **Docker Support**: Provides Docker image for easy deployment
- 🌍 **CORS Support**: Supports cross-origin requests for frontend integration
- 📝 **Thinking Process Display**: Intelligently processes and displays model's thinking process
- 📊 **Real-time Monitoring Dashboard**: Provides web dashboard showing real-time API forwarding status and statistics

## 🚀 Quick Start

### Requirements

- Go 1.23 or higher
- Z.ai access token

### Local Deployment

1. **Clone Repository**
   ```bash
   git clone https://github.com/kisworo/ztoapi.git
   cd ztoapi
   ```

2. **Configure Environment Variables**
   ```bash
   cp config.env .env.local
   # Edit .env.local file and set your ZAI_TOKEN
   ```

3. **Start Service**
   ```bash
   # Use startup script (recommended)
   ./start.sh
   
   # Or run directly
   go run main.go
   ```

4. **Test Service**
    ```bash
    curl http://localhost:9090/v1/models
    ```

5. **Access API Documentation**
    
    After starting the service, you can access the complete API documentation through your browser:
    ```
    http://localhost:9090/docs
    ```
    
    The API documentation provides:
    - Detailed API endpoint descriptions
    - Request parameters and response formats
    - Usage examples in multiple programming languages (Python, cURL, JavaScript)
    - Error handling explanations

6. **Access Dashboard**
   
   After starting the service, you can access the real-time monitoring dashboard through your browser:
   ```
   http://localhost:9090/dashboard
   ```
   
   The dashboard provides:
   - Real-time API request statistics (total requests, successful requests, failed requests, average response time)
   - Detailed information of the latest 100 requests (time, method, path, status code, duration, client IP)
   - Data automatically refreshes every 5 seconds

### Docker Deployment

1. **Build Image**
   ```bash
   docker build -t zto-api .
   ```

2. **Run Container**
   ```bash
   docker run -p 9090:9090 \
     -e ZAI_TOKEN=your_z_ai_token \
     -e DEFAULT_KEY=your_api_key \
     zto-api
   ```

## Render Deployment

1. Fork this repository to your GitHub account

2. Create a new Web Service on Render:
   - Connect your GitHub repository
   - Select Docker as environment
   - Set the following environment variables:
   - `ZAI_TOKEN`: Z.ai access token (optional, will use anonymous token if not provided)
   - `DEFAULT_KEY`: Client API key (optional, default: sk-your-key)
   - `MODEL_NAME`: Display model name (optional, default: GLM-4.5)
   - `PORT`: Service listening port (Render will set automatically)

3. After deployment, use the URL provided by Render as the base_url for OpenAI API

## ⚙️ Environment Variable Configuration

This project supports configuration through environment variables, providing flexible deployment and runtime options.

### 🚀 Quick Start

#### 1. Using Startup Scripts (Recommended)

**macOS/Linux:**
```bash
./start.sh
```

**Windows:**
```cmd
start.bat
```

#### 2. Manual Environment Variable Setup

**macOS/Linux:**
```bash
export ZAI_TOKEN="your_z_ai_token_here"
export DEFAULT_KEY="sk-your-custom-key"
export PORT="9090"
go run main.go
```

**Windows:**
```cmd
set ZAI_TOKEN=your_z_ai_token_here
set DEFAULT_KEY=sk-your-custom-key
set PORT=9090
go run main.go
```

#### 3. Docker Run

```bash
docker run -p 9090:9090 \
  -e ZAI_TOKEN=your_z_ai_token_here \
  -e DEFAULT_KEY=sk-your-custom-key \
  -e PORT=9090 \
  zto-api
```

### 📋 Environment Variables List

#### 🔑 Required Configuration

No required configuration. All configurations have reasonable default values.

#### ⚙️ Optional Configuration

| Variable | Description | Default | Example |
|----------|-------------|---------|---------|
| `ZAI_TOKEN` | Z.ai access token | Empty (uses anonymous token) | `eyJhbGciOiJFUzI1NiIs...` |

#### ⚙️ Optional Configuration

| Variable | Description | Default | Example |
|----------|-------------|---------|---------|
| `DEFAULT_KEY` | Client API key | `sk-your-key` | `sk-my-api-key` |
| `MODEL_NAME` | Display model name | `GLM-4.5` | `GLM-4.5-Pro` |
| `PORT` | Service listening port | `9090` | `9000` |
| `DEBUG_MODE` | Debug mode switch | `true` | `false` |
| `DEFAULT_STREAM` | Default streaming response | `true` | `false` |
| `DASHBOARD_ENABLED` | Dashboard feature switch | `true` | `false` |

#### 🔧 Advanced Configuration

| Variable | Description | Default | Example |
|----------|-------------|---------|---------|
| `UPSTREAM_URL` | Upstream API address | `https://chat.z.ai/api/chat/completions` | Custom URL |

### 📁 Configuration Files

#### Supported Configuration Files (by priority)

1. `.env.local` - Local environment configuration (recommended)
2. `.env` - Environment configuration
3. `config.env` - Configuration template

#### Configuration File Example

```bash
# Copy configuration file
cp config.env .env.local

# Edit configuration file
nano .env.local
```

### 🔐 Getting Z.ai Token

#### Method 1: Browser Developer Tools

1. Login to [Z.ai](https://chat.z.ai)
2. Open browser developer tools (F12)
3. Switch to Network tab
4. Send a message
5. Find the Bearer token in the `Authorization` header of requests

#### Method 2: Cookie Method

1. After logging into Z.ai, check Cookies in developer tools
2. Find cookies containing authentication information

#### Method 3: Anonymous Token

This project supports automatic anonymous token acquisition without manual configuration. When the `ANON_TOKEN_ENABLED` constant is `true`, the system will automatically acquire different anonymous tokens for each conversation, avoiding shared memory.

### 🎯 Usage Examples

#### Basic Configuration

```bash
# .env.local
ZAI_TOKEN=eyJhbGciOiJFUzI1NiIs...
DEFAULT_KEY=sk-my-secret-key
MODEL_NAME=GLM-4.5-Pro
PORT=9000
DEBUG_MODE=false
```

#### Production Environment Configuration

```bash
# .env.production
ZAI_TOKEN=your_production_token
DEFAULT_KEY=sk-production-key
MODEL_NAME=GLM-4.5
PORT=9090
DEBUG_MODE=false
DEFAULT_STREAM=true
```

#### Development Environment Configuration

```bash
# .env.development
ZAI_TOKEN=your_dev_token
DEFAULT_KEY=sk-dev-key
MODEL_NAME=GLM-4.5-Dev
PORT=8080
DEBUG_MODE=true
DEFAULT_STREAM=true
DASHBOARD_ENABLED=true
```

### 📊 Dashboard Features

This project provides a web dashboard for real-time monitoring of API forwarding status and statistics.

#### Features

- Real-time display of API request statistics (total requests, successful requests, failed requests, average response time)
- Shows detailed information of the latest 100 requests (time, method, path, status code, duration, client IP)
- Data automatically refreshes every 5 seconds
- Responsive design, supports access from various devices

#### Access Method

After starting the service, access through your browser:
```
http://localhost:9090/dashboard
```

#### Configuration Options

Control Dashboard feature on/off through the `DASHBOARD_ENABLED` environment variable:

```bash
# Enable Dashboard (default)
DASHBOARD_ENABLED=true

# Disable Dashboard
DASHBOARD_ENABLED=false
```

#### Use Cases

- **Development Debugging**: Real-time view of API request status for debugging and troubleshooting
- **Performance Monitoring**: Monitor API response time and success rate to evaluate system performance
- **Security Audit**: View request sources and frequency to detect abnormal access patterns

### 🔄 Restart Service

After modifying environment variables, restart the service for configuration to take effect:

```bash
# Stop current service
Ctrl+C

# Restart
./start.sh
```

### 🚨 Important Notes

1. **Token Security**: Do not commit real Z.ai tokens to code repository
2. **Configuration Files**: Recommend adding `.env.local` to `.gitignore`
3. **Permission Settings**: Ensure startup scripts have execute permissions (`chmod +x start.sh`)
4. **Port Conflicts**: Ensure configured port is not occupied by other services
5. **Anonymous Token**: When using anonymous tokens, each conversation has independent context
6. **Thinking Process**: Project automatically handles model's thinking process, display mode can be adjusted through `THINK_TAGS_MODE` constant

## 📖 API Usage Examples

### Python Example

```python
import openai

# Configure client
client = openai.OpenAI(
    api_key="your-api-key",  # corresponds to DEFAULT_KEY
    base_url="http://localhost:9090/v1"
)

# Non-streaming request
response = client.chat.completions.create(
    model="GLM-4.5",
    messages=[{"role": "user", "content": "Hello, please introduce yourself"}]
)

print(response.choices[0].message.content)

# Streaming request
response = client.chat.completions.create(
    model="GLM-4.5",
    messages=[{"role": "user", "content": "Please write a poem about spring"}],
    stream=True
)

for chunk in response:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")
```

### curl Example

```bash
# Non-streaming request
curl -X POST http://localhost:9090/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your-api-key" \
  -d '{
    "model": "GLM-4.5",
    "messages": [{"role": "user", "content": "Hello"}],
    "stream": false
  }'

# Streaming request
curl -X POST http://localhost:9090/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your-api-key" \
  -d '{
    "model": "GLM-4.5",
    "messages": [{"role": "user", "content": "Hello"}],
    "stream": true
  }'
```

### JavaScript Example

```javascript
const fetch = require('node-fetch');

async function chatWithGLM(message, stream = false) {
  const response = await fetch('http://localhost:9090/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer your-api-key'
    },
    body: JSON.stringify({
      model: 'GLM-4.5',
      messages: [{ role: 'user', content: message }],
      stream: stream
    })
  });

  if (stream) {
    // Handle streaming response
    const reader = response.body.getReader();
    const decoder = new TextDecoder();
    
    while (true) {
      const { done, value } = await reader.read();
      if (done) break;
      
      const chunk = decoder.decode(value);
      const lines = chunk.split('\n');
      
      for (const line of lines) {
        if (line.startsWith('data: ')) {
          const data = line.slice(6);
          if (data === '[DONE]') {
            console.log('\nStreaming response completed');
            return;
          }
          
          try {
            const parsed = JSON.parse(data);
            const content = parsed.choices[0]?.delta?.content;
            if (content) {
              process.stdout.write(content);
            }
          } catch (e) {
            // Ignore parsing errors
          }
        }
      }
    }
  } else {
    // Handle non-streaming response
    const data = await response.json();
    console.log(data.choices[0].message.content);
  }
}

// Usage example
chatWithGLM('Hello, please introduce JavaScript', false);
```

## 🔧 Troubleshooting

### Common Issues

1. **Connection Failed**
   - Check if service is running normally: `curl http://localhost:9090/v1/models`
   - Access API documentation: `http://localhost:9090/docs`
   - Confirm port configuration is correct

2. **Authentication Failed**
   - Check `DEFAULT_KEY` environment variable setting
   - Confirm `Authorization` header format is correct in requests

3. **Invalid Z.ai Token**
   - Check `ZAI_TOKEN` environment variable setting
   - Confirm token has not expired

4. **Thinking Process Display Issues**
   - Check if `DEBUG_MODE` is enabled
   - View service logs for detailed information

5. **Port Occupied**: Modify `PORT` environment variable or stop service occupying the port
6. **Insufficient Permissions**: Ensure startup scripts have execute permissions
7. **Configuration Not Taking Effect**: Restart service or check configuration file syntax
8. **Streaming Response Issues**: Confirm `DEFAULT_STREAM` setting is correct, check if client supports streaming response

### Debug Mode

Enable debug mode to get detailed logs:

```bash
export DEBUG_MODE=true
go run main.go
```

### Network Troubleshooting

If you encounter network connection issues, try:

1. Check firewall settings
2. Confirm `UPSTREAM_URL` is accessible
3. Test network connectivity:
   ```bash
   curl https://chat.z.ai/api/chat/completions
   ```

### Performance Optimization

1. **Reduce Log Output**: Set `DEBUG_MODE=false`
2. **Adjust Timeout**: Modify `http.Client` timeout settings in code
3. **Use Reverse Proxy**: Recommend using Nginx or similar reverse proxy in production

## 🤝 Contributing

Welcome to submit Issues and Pull Requests! Please ensure:

1. Code follows Go coding style
2. Run tests before submitting
3. Update related documentation
4. Follow project code structure and naming conventions

### Development Workflow

1. Fork this repository
2. Create feature branch: `git checkout -b feature/new-feature`
3. Commit changes: `git commit -am 'Add new feature'`
4. Push branch: `git push origin feature/new-feature`
5. Submit Pull Request

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) file for details.

## ⚠️ Disclaimer

This project is not affiliated with Z.ai official. Please ensure compliance with Z.ai's terms of service before use. Developers are not responsible for any issues arising from the use of this project.

## 📞 Contact

For questions or suggestions, please contact through:

- Submit [Issue](https://github.com/kisworo/ztoapi/issues)
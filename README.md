[![MSeeP.ai Security Assessment Badge](https://mseep.net/pr/flickleafy-mcp-github-repo-creator-badge.png)](https://mseep.ai/app/flickleafy-mcp-github-repo-creator)

# MCP GitHub Repository Creator

A **Model Context Protocol (MCP)** server that provides tools for AI applications like GitHub Copilot to analyze repositories and create GitHub repositories automatically.

## 🚀 Features

- **Repository Analysis**: Automatically analyze local git repositories to extract metadata. Includes traditional and AI approaches to analyze the repository
- **GitHub Integration**: Create private GitHub repositories with proper configuration
- **Topic Management**: Automatically add relevant topics based on project analysis
- **MCP Compatible**: Works with any MCP-compatible AI client (Copilot, Claude, etc.)
- **Automated Setup**: Complete workflow from analysis to GitHub repository creation

## 📦 Installation

1. Clone this repository:

   ```bash
   git clone https://github.com/flickleafy/mcp-github-repo-creator.git
   cd mcp-github-repo-creator
   ```

2. Run the setup script:

   ```bash
   bash setup.sh
   ```

   This will:
   - Create a Python virtual environment
   - Install the MCP SDK and dependencies
   - Set up the project for use

## 🛠 Usage

### As an MCP Server (Recommended)

The server provides the following tools for AI applications:

1. **`get_repo_analysis_instructions`** - Get detailed instructions for repository analysis
2. **`analyze_and_generate_metadata_file`** - Analyze repository and generate metadata
3. **`create_github_repo_from_metadata`** - Create GitHub repository from metadata JSON
4. **`create_github_repository`** - Create repository using existing metadata file  
5. **`full_repository_setup`** - Complete workflow: analyze → create → connect

### Starting the MCP Server

```bash
# Activate virtual environment
source venv/bin/activate

# Start the MCP server
python server.py
```

The server runs on stdio transport and is compatible with MCP clients like:

- GitHub Copilot
- Claude Desktop
- VS Code extensions
- Custom MCP clients

### MCP Client Integration

Configure your MCP client to connect to this server:

```json
{
  "name": "github-repo-creator",
  "command": "python",
  "args": ["server.py"],
  "cwd": "/path/to/mcp-github-repo-creator"
}
```

### Manual Usage (Alternative)

You can also use the underlying functionality directly:

```bash
# Activate virtual environment
source venv/bin/activate

# Interactive mode
python create_github_repo.py

# Direct repository creation
python create_github_repo.py --create

# Manage topics only
python create_github_repo.py --manage-topics
```

## 🎯 Workflow

1. **Analysis**: The AI analyzes your repository structure, README, dependencies, and code
2. **Metadata Generation**: Creates a `github_repo_metadata.json` with repository details
3. **Repository Creation**: Uses GitHub CLI to create a private repository
4. **Configuration**: Sets up topics, enables features, and connects local repository
5. **Push**: Pushes your local code to the new GitHub repository

## 🔄 Detailed Workflow Types

The MCP server supports three main workflow approaches:

### 🤝 Interactive Copilot Workflow (Recommended)

This approach gives Copilot more control and allows for customization:

1. **Request Analysis Instructions**: Ask Copilot to analyze the repository
   - Copilot uses `get_repo_analysis_instructions`
   - Gets detailed instructions on what to analyze
   - Analyzes your repository structure, README, and code

2. **Generate Metadata**: Copilot creates the metadata JSON
   - Based on its analysis, Copilot generates repository metadata
   - You can review and modify the metadata before proceeding

3. **Create Repository**: Copilot creates the GitHub repository
   - Uses `create_github_repo_from_metadata` with the generated metadata
   - Creates repository, pushes code, and configures settings

**Example Chat:**

```
"Please analyze this repository and create a GitHub repository for it. 
I want to review the metadata before you create the repo."
```

### ⚡ Full Automation Workflow

For complete automation without interaction:

1. **Single Command Setup**: Use the `full_repository_setup` tool
   - Analyzes repository automatically
   - Generates metadata file
   - Creates GitHub repository
   - Connects and pushes code
   - All in one step

**Example Chat:**

```
"Automatically set up this project on GitHub with full automation."
```

### 🛠️ Manual/Step-by-Step Workflow

For granular control over each step:

1. **Generate Metadata File**: `analyze_and_generate_metadata_file`
2. **Review/Edit** the generated `github_repo_metadata.json`
3. **Create Repository**: `create_github_repository`

**Example Chat:**

```
"First, generate a metadata file for this repository. I want to review it before creating the GitHub repo."
```

## 💬 Usage Examples with Copilot

Once configured, you can use these natural language commands with Copilot:

### Analyze Repository

```
"Analyze this repository and tell me what metadata would be generated for GitHub."
```

### Generate Metadata File

```
"Generate a github_repo_metadata.json file for this repository."
```

### Create GitHub Repository

```
"Create a GitHub repository for this local project. First analyze it, generate metadata, then create the GitHub repo and connect everything."
```

### Full Setup

```
"Set up this entire project on GitHub - analyze the code, create appropriate metadata, and create the repository."
```

### Step-by-Step Example

```
"Please create a GitHub repository for this project. Analyze the code, 
generate appropriate metadata, and set up the repository on GitHub."
```

**Copilot will:**

- Analyze your project structure and code
- Detect the programming language and frameworks
- Generate topics and description
- Create `github_repo_metadata.json`
- Create the GitHub repository
- Connect your local repo to GitHub
- Push your code

## 🚀 Copilot Integration & Installation

### Prerequisites for Copilot Integration

1. **GitHub Copilot subscription** (Individual, Business, or Enterprise)
2. **VS Code with GitHub Copilot extension**
3. **GitHub CLI** installed and authenticated

### Method 1: VS Code Copilot Integration (Recommended)

1. **Install GitHub CLI**:

   ```bash
   # macOS
   brew install gh
   
   # Ubuntu/Debian
   sudo apt install gh
   
   # Windows (using winget)
   winget install GitHub.cli
   ```

2. **Authenticate GitHub CLI**:

   ```bash
   gh auth login
   ```

3. **Clone and setup this MCP server**:

   ```bash
   git clone https://github.com/flickleafy/mcp-github-repo-creator.git
   cd mcp-github-repo-creator
   bash setup.sh
   ```

4. **Configure VS Code settings** (add to your VS Code settings.json):

   ```json
   {
     "github.copilot.enable": {
       "*": true,
       "mcp": true
     },
     "mcp.servers": {
       "github-repo-creator": {
         "command": "python",
         "args": ["server.py"],
         "cwd": "/full/path/to/mcp-github-repo-creator",
         "env": {
           "PATH": "/full/path/to/mcp-github-repo-creator/venv/bin:${env:PATH}"
         }
       }
     }
   }
   ```

### Method 2: Claude Desktop Integration

1. **Install Claude Desktop** from [claude.ai](https://claude.ai/download)

2. **Configure Claude Desktop** (edit `~/.config/claude-desktop/config.json`):

   ```json
   {
     "mcpServers": {
       "github-repo-creator": {
         "command": "python",
         "args": ["/full/path/to/mcp-github-repo-creator/server.py"],
         "env": {
           "PATH": "/full/path/to/mcp-github-repo-creator/venv/bin"
         }
       }
     }
   }
   ```

3. **Restart Claude Desktop** and start using the commands

### Automatic Installation Script

Create an easy installation script by running:

```bash
# Download and run the auto-installer
curl -sSL https://raw.githubusercontent.com/flickleafy/mcp-github-repo-creator/main/install-copilot.sh | bash
```

Or manually create the installer script in your project:

```bash
# Create installer script
cat > install-copilot.sh << 'EOF'
#!/bin/bash
echo "🚀 Installing MCP GitHub Repository Creator for Copilot..."

# Check dependencies
command -v python3 >/dev/null 2>&1 || { echo "❌ Python 3 is required but not installed."; exit 1; }
command -v git >/dev/null 2>&1 || { echo "❌ Git is required but not installed."; exit 1; }

# Install GitHub CLI if not present
if ! command -v gh &> /dev/null; then
    echo "📦 Installing GitHub CLI..."
    if [[ "$OSTYPE" == "darwin"* ]]; then
        brew install gh
    elif [[ "$OSTYPE" == "linux-gnu"* ]]; then
        sudo apt update && sudo apt install gh
    else
        echo "⚠️ Please install GitHub CLI manually: https://cli.github.com/"
        exit 1
    fi
fi

# Clone repository
INSTALL_DIR="$HOME/.mcp-servers/github-repo-creator"
echo "📁 Installing to $INSTALL_DIR..."
mkdir -p "$HOME/.mcp-servers"
git clone https://github.com/flickleafy/mcp-github-repo-creator.git "$INSTALL_DIR"

# Setup virtual environment
cd "$INSTALL_DIR"
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Create VS Code configuration
VSCODE_CONFIG="$HOME/.config/Code/User/settings.json"
echo "⚙️ Adding VS Code configuration..."
# Add MCP server configuration to VS Code settings

echo "✅ Installation complete!"
echo "🔧 Next steps:"
echo "1. Authenticate with GitHub: gh auth login"
echo "2. Restart VS Code"
echo "3. Start using Copilot with MCP commands!"
EOF

chmod +x install-copilot.sh
```

### Testing the Integration

1. **Open a git repository in VS Code**
2. **Start a chat with Copilot** and try:

   ```
   "Use the MCP GitHub Repository Creator to analyze this repository and create a GitHub repo for it."
   ```

3. **Copilot should respond** with repository analysis and offer to create the GitHub repository

## 📋 Requirements

- Python 3.8+
- Git repository (local)
- GitHub CLI (`gh`) installed and authenticated
- Internet connection for GitHub API calls

### GitHub CLI Setup

Install and authenticate GitHub CLI:

```bash
# Install GitHub CLI (see https://cli.github.com/)
# macOS
brew install gh

# Ubuntu/Debian
sudo apt install gh

# Authenticate
gh auth login
```

## 🔧 MCP Integration

This server implements the **Model Context Protocol** specification, making it compatible with various AI applications:

### Available Tools

- **Repository Analysis**: Extracts project metadata automatically
- **GitHub Repository Creation**: Creates repositories with proper settings
- **Topic Management**: Adds relevant topics based on analysis
- **Complete Workflow**: End-to-end repository setup

### Supported Transports

- **stdio** (default): Standard input/output for direct integration
- Compatible with FastMCP framework for easy deployment

## 📁 Project Structure

```
mcp-github-repo-creator/
├── server.py                    # Main MCP server using FastMCP
├── core/
│   ├── repository_analyzer.py  # Repository analysis logic
│   └── templates.py            # String templates for messages and instructions
├── create_github_repo.py       # Legacy standalone script
├── demo.py                     # Demo MCP client
├── requirements.txt            # Dependencies including MCP SDK
├── setup.sh                   # Environment setup script
├── pyproject.toml             # Optional project metadata
├── .gitignore                 # Git ignore rules
└── README.md                  # This file
```

### Core Modules

- **`server.py`**: Main MCP server that exposes tools to AI clients
- **`core/repository_analyzer.py`**: `RepositoryAnalyzer` class for analyzing repository structure and generating metadata  
- **`core/templates.py`**: Centralized template functions for all long string messages and instructions
- **`create_github_repo.py`**: Legacy standalone script (for direct usage)
- **`demo.py`**: Example client showing how to interact with the MCP server

## 📊 Example Metadata Structure

The server generates metadata in this format:

```json
{
  "repository_name": "my-awesome-project",
  "description": "🚀 A powerful tool for automating GitHub repository creation",
  "topics": ["python", "automation", "github", "mcp", "ai-tools"],
  "created_date": "2025-01-01",
  "project_type": "CLI Tool",
  "primary_language": "Python", 
  "license": "GPL-3.0",
  "features": [
    "Command-line interface",
    "GitHub integration",
    "Automated analysis"
  ]
}
```

## 🎯 Supported Project Types

The MCP server automatically detects and properly categorizes various project types:

- **AI/ML Projects**: Detects TensorFlow, PyTorch, scikit-learn, Transformers, Langchain
- **Web Applications**: React, Vue, Angular, Svelte, Flask, Django, FastAPI, Express, Next.js
- **CLI Tools**: Command-line applications and utilities
- **APIs**: RESTful services, GraphQL, and microservices  
- **Mobile Apps**: React Native, Flutter, Ionic
- **Desktop Apps**: Electron, Tauri, PyQt, Tkinter
- **Libraries**: Software packages, frameworks, and SDKs
- **Game Development**: Unity, Godot, Pygame
- **DevOps Tools**: Docker, Kubernetes, Terraform configurations
- **Data Science**: Jupyter notebooks, data analysis projects

## 🌐 Language Detection

Automatically detects and supports a wide range of programming languages:

**Primary Languages**: Python, JavaScript, TypeScript, Java, C++, C#, Go, Rust, PHP, Ruby, Swift, Kotlin, Scala, R, Shell/Bash

**Web Technologies**: HTML, CSS, Vue, React (JSX/TSX), Svelte, SCSS/Sass, Less

**Specialized**: SQL, YAML, TOML, JSON, Dockerfile, Makefile

The analyzer examines file extensions, dependencies, and project structure to accurately determine the primary language and technology stack.

## 🛡️ Security & Privacy

- **Secure Authentication**: Uses GitHub CLI for secure, token-based authentication
- **Private by Default**: Creates private repositories by default for security
- **No Data Storage**: No sensitive data stored in metadata files
- **Local Processing**: Repository analysis happens locally on your machine
- **GitHub Best Practices**: Follows GitHub's security recommendations
- **Token Scope**: Uses minimal required permissions through GitHub CLI

## ⚠️ Limitations

- **GitHub CLI Required**: Must have GitHub CLI installed and authenticated
- **Git Repository Required**: Must be run from within a git repository with commits
- **Private Repositories**: Creates private repositories only (can be changed manually after creation)
- **GitHub API Limits**: Subject to GitHub API rate limits
- **Topic Restrictions**: Limited to repositories that fit GitHub's topic requirements (20 topics max)
- **Network Dependency**: Requires internet connection for GitHub API calls

## 🔧 Error Handling

The MCP server provides comprehensive error handling with clear messages for common issues:

### Repository Errors

- **Missing git repository**: Clear instructions to initialize git
- **No commits**: Guidance to make initial commit
- **Untracked files**: Prompts to add and commit files

### Authentication Errors

- **GitHub CLI not found**: Installation instructions
- **Not authenticated**: Authentication setup guidance
- **Token expired**: Re-authentication steps

### GitHub API Errors

- **Repository name conflicts**: Suggestions for alternative names
- **Permission issues**: Troubleshooting steps
- **Rate limiting**: Wait time recommendations

## 🆘 Troubleshooting

### Common Issues and Solutions

#### "Not a git repository" Error

```bash
# Initialize git repository
git init

# Add files and make initial commit
git add .
git commit -m "Initial commit"
```

#### "GitHub CLI not authenticated" Error

```bash
# Check authentication status
gh auth status

# Re-authenticate if needed
gh auth login
```

#### "Permission denied" Error

**Solutions:**

- Check GitHub CLI authentication: `gh auth status`
- Ensure you have permission to create repositories in your account
- Verify your GitHub token has appropriate scopes
- For organization repositories, check organization permissions

#### "Repository name already exists" Error

**Solutions:**

- Choose a different repository name
- Check your GitHub account for existing repositories
- Use the suggested alternative names from the error message
- Add a suffix or prefix to make the name unique

#### "GitHub API rate limit exceeded" Error

**Solutions:**

- Wait for the rate limit to reset (usually 1 hour)
- Use authenticated requests (ensure `gh auth login` is completed)
- For high-volume usage, consider GitHub API rate limit best practices

#### "Invalid metadata format" Error

**Solutions:**

- Check the generated `github_repo_metadata.json` for syntax errors
- Ensure all required fields are present
- Validate JSON format using a JSON validator
- Re-run the metadata generation tool

#### "Network connectivity issues" Error

**Solutions:**

- Check internet connection
- Verify GitHub.com is accessible
- Check for firewall or proxy issues
- Try again after network issues are resolved

## 🤝 Contributing

1. If you like the project give a ⭐ to the repository
2. Create a feature branch, Make your changes, Submit a pull request
3. Ensure your code follows the project's coding standards
4. Add tests for new features or bug fixes
5. Update documentation as needed

## 📜 License

This project is licensed under the GNU General Public License v3.0 - see the LICENSE file for details.

## 🔗 Links

- [Model Context Protocol](https://modelcontextprotocol.io/)
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk)
- [GitHub CLI](https://cli.github.com/)

---

*Built with ❤️ using the Model Context Protocol*

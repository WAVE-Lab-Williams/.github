# Getting Started with WAVE Lab Development

Welcome to the WAVE Lab! This guide will walk you through setting up your development environment and understanding our data collection and analysis workflow. By the end of this guide, you'll be ready to run experiments, collect data, and perform analysis using our research stack.

## 🎯 What You'll Learn

- How to set up your development environment (Windows, macOS, or Linux)
- Understanding our tech stack: PostgreSQL + FastAPI backend, JavaScript/Python clients
- How to run and customize psychology experiments
- Data analysis with Python, Pandas, and NumPy

## 📋 Prerequisites

No prior programming experience required! This guide assumes you're starting from scratch. You'll need:

- A computer (Windows, macOS, or Linux)
- Basic willingness to use the command line (we'll walk you through it!)
- About 30-60 minutes for initial setup

## 🖥️ Step 1: Setting Up Your Terminal

### Windows Users: Install WSL

Windows Subsystem for Linux (WSL) gives you a Linux environment on Windows, which makes development much easier.

Follow the official installation guide: [https://learn.microsoft.com/en-us/windows/wsl/install](https://learn.microsoft.com/en-us/windows/wsl/install)

After installation, open your Ubuntu terminal and update packages:
```bash
sudo apt update && sudo apt upgrade -y
```

### macOS Users: Use Terminal

1. **Open Terminal**
   - Press `Cmd + Space`, type "Terminal", and press Enter
   - Or go to Applications → Utilities → Terminal

### Linux Users

You're already set! Open your terminal and continue to Step 2.

## 🛠️ Step 2: Install Essential Tools

### Install Node.js

Follow the official installation guide: [https://nodejs.org/en/download](https://nodejs.org/en/download)

We recommend using the LTS (Long Term Support) version.

### Install UV (Python Package Manager)

Follow the official installation guide: [https://docs.astral.sh/uv/getting-started/installation/](https://docs.astral.sh/uv/getting-started/installation/)

After installation, restart your terminal and verify:
```bash
uv --version
```

## 🧑‍💻 Step 3: Choose Your IDE

An IDE (Integrated Development Environment) makes coding much easier. Pick one:

### Option A: VS Code (Recommended for beginners)
- **Download**: [https://code.visualstudio.com/](https://code.visualstudio.com/)
- **Why we like it**: Free, excellent Python/JavaScript support, great for Jupyter notebooks
- **WSL users**: Install the "WSL" extension to work with your Linux environment

### Option B: PyCharm
- **Download**: [https://www.jetbrains.com/pycharm/](https://www.jetbrains.com/pycharm/) (Community Edition is free)
- **Why we like it**: Powerful Python features, excellent debugging
- **Best for**: People who want advanced Python development features

## 🏗️ Step 4: Understanding Our Tech Stack

Before diving in, let's understand what you'll be working with:

### The Big Picture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Experiments   │    │     Backend      │    │    Analysis     │
│  (JavaScript)   ├───▶│ PostgreSQL +     ├───▶│    (Python)     │
│   HTML + CSS    │    │     FastAPI      │    │ Pandas + NumPy  │
│ + wave-client.js│    │                  │    │ + wave-client   │
└─────────────────┘    └──────────────────┘    └─────────────────┘
```

### What Each Part Does

1. **Frontend (Experiments)**
   - **Languages**: HTML, CSS, JavaScript
   - **Purpose**: Present stimuli to participants, collect responses
   - **Tools**: JSPsych framework for psychology experiments
   - **WAVE Integration**: `wave-client.js` handles sending data to the backend

2. **Backend (Data Storage)**
   - **Languages**: Python (FastAPI framework)
   - **Database**: PostgreSQL
   - **Purpose**: Store experiment data securely, provide API endpoints

3. **Analysis (Data Science)**
   - **Languages**: Python
   - **Libraries**: Pandas (data manipulation), NumPy (numerical computing)
   - **Purpose**: Clean, analyze, and visualize experimental data
   - **WAVE Integration**: `wave-client` Python library retrieves data from the backend

### The WAVE Client Library

The **WAVE Client** is our custom library that connects experiments and analysis to our backend:

- **JavaScript version** (`wave-client.js`): Used in experiments to send data to the backend
  - Automatically extracts API keys from URL parameters
  - Handles data validation and secure transmission
  - Provides simple methods like `logExperimentData()`

- **Python version** (`wave-client`): Used in analysis to retrieve and work with data
  - Downloads experiment data as pandas DataFrames
  - Provides search capabilities across all experiments
  - Handles authentication and data formatting

## 🔬 Step 5: Understanding the Complete Workflow

Before diving into individual components, let's understand how everything fits together. The WAVE system demonstrates a complete research workflow from experiment creation to data analysis.

### Complete Integration Example

📖 **See the full workflow**: [Python-JavaScript Integration Example](https://github.com/WAVE-Lab-Williams/wave-client/blob/main/python/examples/python_javascript_integration_example.ipynb)

This interactive notebook demonstrates the complete research pipeline:

1. **Python Setup** - Create experiment schema and configuration using Python
2. **JavaScript Data Collection** - Participants complete web-based experiments that automatically log data
3. **Python Analysis** - Retrieve and analyze collected data using pandas/numpy
4. **Cleanup** - Proper data management and cleanup procedures

**Key Learning Points**:
- How experiment schemas are defined and managed
- Real-time data collection from web experiments
- Seamless integration between JavaScript frontend and Python analysis
- Proper data management and research reproducibility practices

### Setting Up Your First Experiment

Let's start with the experiment template to understand the basics:

```bash
# Navigate to your code directory
mkdir -p ~/code/wave
cd ~/code/wave

# Clone the experiment template
git clone https://github.com/WAVE-Lab-Williams/experiment-template.git
cd experiment-template

# Install dependencies and start development server
npm install
npm run dev
```

**Test Your Setup**:
1. Visit `http://localhost:8080` in your browser
2. Complete the demonstration experiment (colored circles task)
3. Observe how data flows from participant responses to console logs

This gives you the foundation - then explore the complete integration notebook above to see how this connects to our full research pipeline.

## 🐍 Step 6: Setting Up Python for Data Analysis

Now let's set up the Python environment for analyzing your data.

### Create Your Analysis Project

```bash
# Navigate back to your wave folder
cd ~/code/wave

# Create a new Python project for analysis
uv init my-wave-analysis
cd my-wave-analysis

# Add the WAVE client to your project
uv add https://github.com/WAVE-Lab-Williams/wave-client/releases/latest/download/wave_client-1.0.0-py3-none-any.whl

# Also add common data science libraries
uv add pandas numpy matplotlib seaborn jupyter
```

### Test Your Python Setup

Let's test everything works by using Python interactively:

```bash
# Activate the virtual environment
source .venv/bin/activate  # On macOS/Linux
# or
.venv\Scripts\activate     # On Windows

# Start Python
python
```

Now test your imports in the Python interpreter:

```python
import pandas as pd
import numpy as np
from wave_client import WaveClient

print(f"Pandas version: {pd.__version__}")
print(f"NumPy version: {np.__version__}")

# Create a simple DataFrame to test pandas
test_data = pd.DataFrame({
    'participant_id': [1, 2, 3, 4, 5],
    'reaction_time': [0.45, 0.52, 0.38, 0.61, 0.49],
    'accuracy': [1, 1, 0, 1, 1]
})

print(f"   Mean reaction time: {test_data['reaction_time'].mean():.3f}s")
print(f"   Overall accuracy: {test_data['accuracy'].mean():.1%}")

# Exit Python (ctrl + D also works)
exit()
```

```bash
# Deactivate the virtual environment when done
deactivate
```

**Success!** You now have a working Python data analysis environment.

## 📚 Step 7: Understanding Data Analysis Libraries

### What is Pandas? 

**Pandas** is like Excel for Python - it helps you work with data in tables (called DataFrames).

**Key concepts:**
- **DataFrame**: A table of data with rows and columns
- **Series**: A single column of data
- **Index**: Row labels (like row numbers in Excel)

**Common operations:**
```python
# Read data from a file
df = pd.read_csv('experiment_data.csv')

# Look at first few rows
df.head()

# Get basic statistics
df.describe()

# Filter data
fast_responses = df[df['reaction_time'] < 0.5]

# Group and summarize
df.groupby('condition').mean()
```

**📖 Learn more**: [Pandas Documentation](https://pandas.pydata.org/docs/) and [10 Minutes to Pandas](https://pandas.pydata.org/docs/user_guide/10min.html)

### What is NumPy?

**NumPy** handles numbers and mathematical operations - it's the foundation under pandas.

**Key concepts:**
- **Arrays**: Lists of numbers that can be manipulated efficiently
- **Mathematical functions**: sin, cos, sqrt, mean, etc.
- **Statistics**: Standard deviation, correlation, etc.

**Common operations:**
```python
# Create arrays
data = np.array([1.2, 2.3, 1.8, 2.1, 1.9])

# Basic math
np.mean(data)        # Average
np.std(data)         # Standard deviation
np.sqrt(data)        # Square root of each value

# Generate random data for simulations
np.random.normal(0, 1, 100)  # 100 random numbers from normal distribution
```

**📖 Learn more**: [NumPy Documentation](https://numpy.org/doc/) and [NumPy Quickstart](https://numpy.org/doc/stable/user/quickstart.html)

## 🏃‍♂️ Next Steps

Congratulations! You now have a complete development environment. Here's what to explore next:

### 1. Learn More About Our Tools

**Experiment Design:**
- 📖 [JSPsych Documentation](https://www.jspsych.org/) - Creating psychology experiments
- 🔗 [Experiment Template Guide](https://github.com/WAVE-Lab-Williams/experiment-template) - Our specific setup

**Data Analysis:**
- 📖 [Python for Data Analysis](https://wesmckinney.com/book/) - Comprehensive pandas/numpy guide
- 🎓 [Kaggle Learn](https://www.kaggle.com/learn) - Free data science courses

### 2. Practice with Real Projects

- **Modify the experiment template** with your own stimuli
- **Run the backend locally** to understand data flow
- **Analyze real experiment data** from the lab

### 3. Join Our Workflow

- **Ask for API credentials** from Prof. Wong
- **Set up your `.env` file** with real backend credentials
- **Connect to our shared database** for collaborative analysis

## 🆘 Getting Help

### When Things Go Wrong

1. **Read the error message carefully** - it usually tells you what's wrong
2. **Check your typing** - programming is very picky about exact spelling
3. **Ask for help!** - Everyone gets stuck, and we're here to support you

### Common Issues

**"Command not found"**
- You might need to restart your terminal
- Check if you installed the tool correctly

**"Permission denied"**
- Try adding `sudo` before the command (Linux/macOS)
- Make sure you're not in a protected directory

**"Module not found"**
- Make sure you're in the right project directory
- Try running `uv sync` to reinstall dependencies

---

*Questions? Email Prof. Wong at kww3@williams.edu or post in our lab Discord.*
# Vets Who Code: Streamlit Fundamentals

## Building Interactive Web Applications with Python

### 🎯 Lesson Objectives

After completing this lesson, you will be able to:

- Set up a Streamlit development environment
- Understand core Streamlit components and layouts
- Create interactive user interfaces
- Handle user input and state management
- Build data visualizations
- Deploy Streamlit applications

### 🛠️ Setup Instructions

1. Ensure Python 3.10 is installed:
   
```bash
python --version  # Should show Python 3.10.x
```

1. Create and activate a virtual environment:

```bash
python -m venv vwc-streamlit
source vwc-streamlit/bin/activate  # Windows: vwc-streamlit\Scripts\activate
```

1. Install required packages:

```bash
pip install streamlit==1.31.0 pandas numpy
```

### 📚 Lesson 1: Streamlit Basics

Create a file named `lesson1.py`:

```python
import streamlit as st

# Page configuration
st.set_page_config(
    page_title="VWC Streamlit Lesson 1",
    page_icon="🎖️",
    layout="wide"
)

# Basic text elements
st.title("Welcome to Streamlit! 🚀")
st.header("Let's learn the basics")
st.subheader("Text elements and markdown")

# Markdown support
st.markdown("""
    Here's what we can do with markdown:
    - **Bold text**
    - *Italic text*
    - `Code snippets`
    - [Links](https://vetswhocode.io)
""")

# Interactive elements
if st.button("Click me!"):
    st.write("Button was clicked!")

user_input = st.text_input("Enter your name")
if user_input:
    st.write(f"Hello, {user_input}!")

# Run with: streamlit run lesson1.py
```

### 📚 Lesson 2: Layouts and Containers

Create `lesson2.py`:

```python
import streamlit as st

st.title("Layout in Streamlit")

# Columns
col1, col2 = st.columns(2)

with col1:
    st.header("Column 1")
    st.write("This is the left column")
    st.button("Left Button")

with col2:
    st.header("Column 2")
    st.write("This is the right column")
    st.button("Right Button")

# Sidebar
with st.sidebar:
    st.header("Sidebar")
    option = st.selectbox(
        "Choose an option",
        ["Option 1", "Option 2", "Option 3"]
    )

# Tabs
tab1, tab2 = st.tabs(["Tab 1", "Tab 2"])

with tab1:
    st.write("This is tab 1")

with tab2:
    st.write("This is tab 2")

# Containers
with st.container():
    st.write("This is a container")
    st.write("It helps organize content")
```

### 📚 Lesson 3: State Management

Create `lesson3.py`:

```python
import streamlit as st

# Initialize session state
if "counter" not in st.session_state:
    st.session_state.counter = 0

st.title("State Management in Streamlit")

# Display counter
st.write(f"Counter value: {st.session_state.counter}")

# Buttons to modify state
if st.button("Increment"):
    st.session_state.counter += 1
    st.experimental_rerun()

if st.button("Decrement"):
    st.session_state.counter -= 1
    st.experimental_rerun()

# Form example
with st.form("my_form"):
    name = st.text_input("Name")
    age = st.number_input("Age", min_value=0, max_value=120)
    submitted = st.form_submit_button("Submit")
    
    if submitted:
        st.write(f"Name: {name}, Age: {age}")
```

### 📚 Lesson 4: Chat Interface Components

Create `lesson4.py`:

```python
import streamlit as st
from datetime import datetime

# Initialize chat history
if "messages" not in st.session_state:
    st.session_state.messages = []

st.title("Chat Interface Components")

# Chat display
for message in st.session_state.messages:
    with st.container():
        st.write(f"{message['role']}: {message['content']}")
        st.caption(f"Sent at {message['timestamp']}")

# Message input
message = st.text_input("Type a message")
if st.button("Send") and message:
    # Add message to history
    st.session_state.messages.append({
        "role": "user",
        "content": message,
        "timestamp": datetime.now().strftime("%I:%M %p")
    })
    
    # Simulate response
    st.session_state.messages.append({
        "role": "assistant",
        "content": f"You said: {message}",
        "timestamp": datetime.now().strftime("%I:%M %p")
    })
    st.experimental_rerun()

# Clear chat button
if st.button("Clear Chat"):
    st.session_state.messages = []
    st.experimental_rerun()
```

### 📚 Lesson 5: Data Visualization

Create `lesson5.py`:

```python
import streamlit as st
import pandas as pd
import numpy as np

st.title("Data Visualization in Streamlit")

# Create sample data
data = pd.DataFrame({
    'date': pd.date_range('2024-01-01', '2024-01-10'),
    'values': np.random.randn(10).cumsum()
})

# Line chart
st.subheader("Line Chart")
st.line_chart(data.set_index('date'))

# Bar chart
st.subheader("Bar Chart")
st.bar_chart(data.set_index('date'))

# Metrics
col1, col2, col3 = st.columns(3)
with col1:
    st.metric("Total", f"{data['values'].sum():.2f}")
with col2:
    st.metric("Average", f"{data['values'].mean():.2f}")
with col3:
    st.metric("Max", f"{data['values'].max():.2f}")
```

### 🔄 Practice Exercises

1. Create a todo list application using session state
2. Build a simple calculator with a form
3. Create a data dashboard with multiple visualizations
4. Implement a file uploader with preview
5. Build a multi-page application using st.sidebar

### 🚀 Mini-Projects

1. **AI Chat Interface**
   
- Implement message history
- Add user input handling
- Create styled message bubbles
- Add typing indicators

1. **Data Dashboard**
   
- Load and display data
- Add interactive filters
- Create multiple visualizations
- Implement data download

### 📝 Best Practices

1. **Performance**

```python
# Cache data loading
@st.cache_data
def load_data():
    return pd.read_csv("data.csv")

# Cache resource-intensive computations
@st.cache_resource
def load_model():
    return Model()
```

2. **Error Handling**

```python
try:
    result = process_data()
except Exception as e:
    st.error(f"An error occurred: {str(e)}")
```

1. **User Experience**

```python
with st.spinner("Processing..."):
    # Long running operation
    process_data()
st.success("Done!")
```

### 🎯 Next Steps

1. Explore Streamlit Cloud deployment
2. Learn about custom components
3. Study authentication and security
4. Practice responsive design
5. Master advanced visualizations

### 📚 Resources

- [Streamlit Documentation](https://docs.streamlit.io/)
- [Streamlit Gallery](https://streamlit.io/gallery)
- [VWC GitHub](https://github.com/Vets-Who-Code)
- [Python 3.10 Documentation](https://docs.python.org/3.10/)

### 💡 Tips

- Use `st.experimental_rerun()` sparingly
- Keep state management simple
- Test locally before deployment
- Use caching for performance
- Follow Python best practices
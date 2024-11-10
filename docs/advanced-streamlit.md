# Vets Who Code: Advanced Streamlit Development

## Taking Your Streamlit Apps to the Next Level

### 🎯 Advanced Learning Objectives

After completing this lesson, you will be able to:

- Deploy applications to Streamlit Cloud
- Create custom Streamlit components
- Implement secure authentication
- Build responsive layouts
- Create advanced data visualizations

### 📚 Lesson 1: Streamlit Cloud Deployment

#### Local Setup for Deployment

1. Create `requirements.txt`:
```txt
streamlit==1.31.0
pandas==2.0.3
numpy==1.24.3
python-dotenv==1.0.0
```

2. Create `.gitignore`:
```txt
venv/
.env
__pycache__/
*.pyc
.DS_Store
```

3. Create `config.toml` in `.streamlit` folder:
```toml
[theme]
primaryColor = "#D6372E"  # VWC Red
backgroundColor = "#FFFFFF"
secondaryBackgroundColor = "#F0F2F6"
textColor = "#262730"
font = "sans serif"

[server]
maxUploadSize = 200
enableXsrfProtection = true
```

#### Deployment Steps
1. Push to GitHub:
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin your-repo-url
git push -u origin main
```

2. Streamlit Cloud Setup:
- Visit streamlit.io/cloud
- Connect your GitHub repository
- Select main file
- Add secrets in dashboard
- Deploy

### 📚 Lesson 2: Custom Components

#### Basic Custom Component
Create `custom_components.py`:
```python
import streamlit as st
import streamlit.components.v1 as components

def custom_html_component():
    components.html(
        """
        <div style="padding: 20px; background: #f0f2f6; border-radius: 10px;">
            <h2>Custom HTML Component</h2>
            <p>This is a custom component using HTML and JavaScript.</p>
            <button onclick="alert('Hello from custom component!')">
                Click Me
            </button>
        </div>
        """,
        height=200
    )

def custom_chart_component():
    components.html(
        """
        <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
        <div id="chart"></div>
        <script>
            var data = [{
                type: 'scatter',
                x: [1, 2, 3, 4],
                y: [10, 15, 13, 17]
            }];
            Plotly.newPlot('chart', data);
        </script>
        """,
        height=400
    )

# Usage
st.title("Custom Components Demo")
custom_html_component()
custom_chart_component()
```

### 📚 Lesson 3: Authentication & Security

#### Basic Authentication
Create `auth.py`:
```python
import streamlit as st
import hmac
import hashlib
from datetime import datetime, timedelta

class Authenticator:
    def __init__(self):
        if 'authenticated' not in st.session_state:
            st.session_state.authenticated = False
            st.session_state.auth_time = None
    
    def _hash_password(self, password: str) -> str:
        return hashlib.sha256(password.encode()).hexdigest()
    
    def _check_password(self, username: str, password: str) -> bool:
        # Replace with database lookup in production
        users = {
            "demo": self._hash_password("demo123")
        }
        return username in users and users[username] == self._hash_password(password)
    
    def login_form(self):
        if not st.session_state.authenticated:
            st.title("Login")
            
            with st.form("login_form"):
                username = st.text_input("Username")
                password = st.text_input("Password", type="password")
                submitted = st.form_submit_button("Login")
                
                if submitted:
                    if self._check_password(username, password):
                        st.session_state.authenticated = True
                        st.session_state.auth_time = datetime.now()
                        st.experimental_rerun()
                    else:
                        st.error("Invalid credentials")
        
        return st.session_state.authenticated

# Usage example
auth = Authenticator()
if auth.login_form():
    st.write("Welcome to the secure area!")
```

### 📚 Lesson 4: Responsive Design

#### Responsive Layout Example
Create `responsive.py`:
```python
import streamlit as st

def get_device_type():
    # Basic device detection using viewport width
    # Note: This is a simplified approach
    return st.select_slider(
        "Preview as:",
        options=["Mobile", "Tablet", "Desktop"],
        value="Desktop",
        key="device_type"
    )

st.title("Responsive Design Demo")

device = get_device_type()

# Responsive layout based on device
if device == "Mobile":
    # Single column layout
    st.write("Mobile Layout")
    st.image("https://via.placeholder.com/300x200")
    st.write("Content below image")
    
elif device == "Tablet":
    # Two column layout
    col1, col2 = st.columns([1, 1])
    with col1:
        st.write("Tablet Layout - Left")
        st.image("https://via.placeholder.com/400x300")
    with col2:
        st.write("Tablet Layout - Right")
        st.write("Content next to image")
        
else:  # Desktop
    # Three column layout
    col1, col2, col3 = st.columns([1, 2, 1])
    with col1:
        st.write("Sidebar content")
    with col2:
        st.write("Desktop Layout - Main")
        st.image("https://via.placeholder.com/800x400")
    with col3:
        st.write("Additional content")

# Responsive CSS
st.markdown("""
    <style>
    @media (max-width: 640px) {
        .main { padding: 1rem; }
        .stImage { max-width: 100%; }
    }
    </style>
""", unsafe_allow_html=True)
```

### 📚 Lesson 5: Advanced Visualizations

#### Interactive Dashboard
Create `advanced_viz.py`:
```python
import streamlit as st
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go
from datetime import datetime, timedelta

# Generate sample data
def generate_data():
    dates = pd.date_range(
        start=datetime.now() - timedelta(days=30),
        end=datetime.now(),
        freq='D'
    )
    
    df = pd.DataFrame({
        'date': dates,
        'users': np.random.randint(100, 1000, size=len(dates)),
        'revenue': np.random.uniform(1000, 5000, size=len(dates)),
        'conversion': np.random.uniform(0.1, 0.3, size=len(dates))
    })
    return df

# Create interactive visualizations
def create_dashboard():
    st.title("Advanced Analytics Dashboard")
    
    # Load data
    df = generate_data()
    
    # Date filter
    date_range = st.date_input(
        "Select Date Range",
        value=(df['date'].min(), df['date'].max()),
        key="date_range"
    )
    
    # Filter data
    mask = (df['date'].dt.date >= date_range[0]) & (df['date'].dt.date <= date_range[1])
    filtered_df = df[mask]
    
    # Metrics row
    col1, col2, col3 = st.columns(3)
    with col1:
        st.metric(
            "Total Users",
            f"{filtered_df['users'].sum():,.0f}",
            f"{filtered_df['users'].diff().mean():+.1f}/day"
        )
    with col2:
        st.metric(
            "Total Revenue",
            f"${filtered_df['revenue'].sum():,.2f}",
            f"{filtered_df['revenue'].pct_change().mean()*100:+.1f}%"
        )
    with col3:
        st.metric(
            "Avg Conversion",
            f"{filtered_df['conversion'].mean()*100:.1f}%",
            f"{filtered_df['conversion'].diff().mean()*100:+.2f}pp"
        )
    
    # Interactive charts
    tab1, tab2 = st.tabs(["Time Series", "Analysis"])
    
    with tab1:
        # Create time series chart
        fig = go.Figure()
        fig.add_trace(go.Scatter(
            x=filtered_df['date'],
            y=filtered_df['users'],
            name="Users",
            line=dict(color="#D6372E")
        ))
        fig.add_trace(go.Scatter(
            x=filtered_df['date'],
            y=filtered_df['revenue'],
            name="Revenue",
            yaxis="y2"
        ))
        
        fig.update_layout(
            title="Users and Revenue Over Time",
            yaxis=dict(title="Users"),
            yaxis2=dict(title="Revenue", overlaying="y", side="right"),
            hovermode="x unified"
        )
        
        st.plotly_chart(fig, use_container_width=True)
    
    with tab2:
        # Create scatter plot
        fig = px.scatter(
            filtered_df,
            x='users',
            y='revenue',
            size='conversion',
            color='conversion',
            color_continuous_scale="Reds",
            title="User-Revenue Correlation"
        )
        
        st.plotly_chart(fig, use_container_width=True)

if __name__ == "__main__":
    create_dashboard()
```

### 🛠️ Practice Projects

1. **Secure Dashboard Application**

- Implement user authentication
- Create role-based access control
- Add secure data storage
- Deploy to Streamlit Cloud

1. **Responsive Chat Interface**

- Build custom chat components
- Create mobile-friendly layout
- Add real-time updates
- Implement message persistence

1. **Advanced Analytics Dashboard**

- Create interactive visualizations
- Add custom filtering
- Implement data export
- Build responsive layout

### 📝 Best Practices

1. **Security**

- Use environment variables
- Implement proper authentication
- Validate user input
- Use HTTPS
- Keep dependencies updated

1. **Performance**

- Cache expensive operations
- Optimize data loading
- Use efficient layouts
- Minimize re-runs

1. **Deployment**

- Use requirements.txt
- Configure secrets
- Set up monitoring
- Enable error tracking

1. **Design**

- Follow responsive design principles
- Maintain consistent branding
- Use appropriate color schemes
- Consider accessibility

### 🚀 Next Steps

1. Explore WebSocket integration
2. Study database connectivity
3. Learn about custom caching
4. Master component lifecycle
5. Practice CI/CD deployment
## Poetry

Install poetry:

```bash
sudo apt-get install pipx
pipx install poetry
```

## Flask Project

Initialize poetry and add flask and psutil to the project:

```bash
poetry init -n
poetry add flask
poetry add psutil
```

Create `app.py` and add the following as its contents:

```python
from flask import Flask, render_template
import psutil

app = Flask(__name__)

@app.route('/')
def stats():

    try:
        with open("/sys/class/thermal/thermal_zone0/temp", "r") as f:
            temp = float(f.read()) / 1000.0  # Convert millidegree to degree
            temp_data = f"CPU Temperature: {temp:.1f}°C"
    except FileNotFoundError:
        temp_data = "Temperature data not available."
    
    cpu_usage = psutil.cpu_percent(interval=1)
    
    memory = psutil.virtual_memory()
    
    return render_template("index.html", temp=temp, cpu_usage=cpu_usage, memory=memory)


if __name__ == '__main__':
    app.run()

```

Create a `templates` directory, then create `index.html` inside of it. Add the following as its contents:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Raspberry Pi Status</title>
    <style>
        body { font-family: Arial, sans-serif; background-color: #f4f4f9; text-align: center; margin-top: 50px; }
        .stat { font-size: 1.5em; margin: 10px; }
    </style>
</head>
<body>
    <h1>Raspberry Pi System Status</h1>
    <div class="stat">
        {% if temp %}
            <p>CPU Temperature: {{ temp }} °C</p>
        {% else %}
            <p>CPU Temperature data not available.</p>
        {% endif %}
    </div>
    <div class="stat">
        <p>CPU Usage: {{ cpu_usage }}%</p>
    </div>
    <div class="stat">
        <p>Memory Usage: {{ memory.percent }}% (Used: {{ memory.used // (1024**2) }} MB, Total: {{ memory.total // (1024**2) }} MB)</p>
    </div>
</body>
</html>
```
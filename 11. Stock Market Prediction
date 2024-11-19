from flask import Flask, render_template_string, request, redirect, url_for, flash
import sqlite3

app = Flask(__name__)
app.secret_key = "your_secret_key"  # For session management and flashing messages

# Function to initialize the SQLite database
def init_db():
    conn = sqlite3.connect('stock_market.db')
    cursor = conn.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS stocks (
                        id INTEGER PRIMARY KEY AUTOINCREMENT,
                        stock_symbol TEXT NOT NULL,
                        company_name TEXT NOT NULL,
                        prediction REAL NOT NULL,
                        confidence REAL NOT NULL
                    )''')
    conn.commit()
    conn.close()

# Homepage route
@app.route('/')
def index():
    return render_template_string('''
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Stock Market Prediction System</title>
        <style>
            body {
                font-family: Arial, sans-serif;
                background-color: #f4f4f4;
                padding: 50px;
                text-align: center;
            }
            h1 {
                color: #333;
            }
            .container {
                background-color: #fff;
                padding: 20px;
                border-radius: 8px;
                box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            }
            a {
                display: inline-block;
                margin-top: 20px;
                background-color: #4CAF50;
                color: white;
                padding: 10px 20px;
                border-radius: 4px;
                text-decoration: none;
            }
            a:hover {
                background-color: #45a049;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <h1>Welcome to the Stock Market Prediction System</h1>
            <a href="{{ url_for('dashboard') }}">Go to Dashboard</a>
        </div>
    </body>
    </html>
    ''')

# Dashboard route
@app.route('/dashboard', methods=['GET', 'POST'])
def dashboard():
    if request.method == 'POST':
        stock_symbol = request.form['stock_symbol']
        company_name = request.form['company_name']
        prediction = request.form['prediction']
        confidence = request.form['confidence']

        conn = sqlite3.connect('stock_market.db')
        cursor = conn.cursor()
        cursor.execute("INSERT INTO stocks (stock_symbol, company_name, prediction, confidence) VALUES (?, ?, ?, ?)",
                       (stock_symbol, company_name, float(prediction), float(confidence)))
        conn.commit()
        conn.close()
        flash('Stock prediction added successfully!')
        return redirect(url_for('dashboard'))

    # Fetch all stocks
    conn = sqlite3.connect('stock_market.db')
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM stocks")
    stocks = cursor.fetchall()
    conn.close()

    return render_template_string('''
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Dashboard - Stock Market Prediction</title>
        <style>
            body {
                font-family: Arial, sans-serif;
                background-color: #f4f4f4;
                padding: 50px;
                text-align: center;
            }
            h1 {
                color: #333;
            }
            .container {
                background-color: #fff;
                padding: 20px;
                border-radius: 8px;
                box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            }
            table {
                width: 100%;
                border-collapse: collapse;
                margin-top: 20px;
            }
            table, th, td {
                border: 1px solid #ddd;
                padding: 8px;
                text-align: left;
            }
            th {
                background-color: #f2f2f2;
            }
            form input, form button {
                padding: 10px;
                margin: 5px;
                border-radius: 4px;
            }
            form input {
                width: 200px;
            }
            .flash {
                background-color: #4CAF50;
                color: white;
                padding: 10px;
                margin: 20px 0;
                border-radius: 4px;
                text-align: center;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <h1>Stock Dashboard</h1>
            <form method="POST">
                <input type="text" name="stock_symbol" placeholder="Stock Symbol" required>
                <input type="text" name="company_name" placeholder="Company Name" required>
                <input type="number" step="0.01" name="prediction" placeholder="Prediction Price" required>
                <input type="number" step="0.01" name="confidence" placeholder="Confidence (%)" required>
                <button type="submit">Add Stock</button>
            </form>

            {% with messages = get_flashed_messages() %}
                {% if messages %}
                    <div class="flash">{{ messages[0] }}</div>
                {% endif %}
            {% endwith %}
            
            <h2>Stocks List</h2>
            <table>
                <tr>
                    <th>ID</th>
                    <th>Stock Symbol</th>
                    <th>Company Name</th>
                    <th>Prediction</th>
                    <th>Confidence</th>
                    <th>Action</th>
                </tr>
                {% for stock in stocks %}
                <tr>
                    <td>{{ stock[0] }}</td>
                    <td>{{ stock[1] }}</td>
                    <td>{{ stock[2] }}</td>
                    <td>{{ stock[3] }}</td>
                    <td>{{ stock[4] }}</td>
                    <td>
                        <form action="{{ url_for('delete_stock', stock_id=stock[0]) }}" method="POST">
                            <button type="submit">Delete</button>
                        </form>
                    </td>
                </tr>
                {% endfor %}
            </table>
        </div>
    </body>
    </html>
    ''', stocks=stocks)

# Route to delete a stock
@app.route('/delete/<int:stock_id>', methods=['POST'])
def delete_stock(stock_id):
    conn = sqlite3.connect('stock_market.db')
    cursor = conn.cursor()
    cursor.execute("DELETE FROM stocks WHERE id = ?", (stock_id,))
    conn.commit()
    conn.close()
    flash('Stock deleted successfully!')
    return redirect(url_for('dashboard'))

# Run the application
if __name__ == '__main__':
    init_db()  # Initialize the database
    app.run(debug=True)

give me mermaid code for this

# equimetrics.github.io
from flask import Flask, render_template, request
import yfinance as yf

app = Flask(__name__)

@app.route('/')
def index():
    return '''
        <form method="POST" action="/analyze">
            <input name="ticker" placeholder="Digite o código da ação" required>
            <button type="submit">Analisar</button>
        </form>
    '''

@app.route('/analyze', methods=['POST'])
def analyze():
    ticker = request.form['ticker']
    stock = yf.Ticker(ticker)

    # Obter dados financeiros
    financials = stock.financials.to_html(classes='table table-striped')
    balance_sheet = stock.balance_sheet.to_html(classes='table table-striped')
    dividends = stock.dividends.to_html(classes='table table-striped')

    return f'''
        <h2>Resultados para {ticker}</h2>
        <h3>Financials:</h3>{financials}
        <h3>Balance Sheet:</h3>{balance_sheet}
        <h3>Dividends:</h3>{dividends}
        <br>
        <a href="/">Voltar</a>
    '''

if __name__ == '__main__':
    app.run(debug=True)


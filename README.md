# stock-portfolio-tracker

import yfinance as yf

# Define portfolio as a dictionary: {symbol: {'shares': int, 'buy_price': float}}
portfolio = {}

def add_stock():
    symbol = input("Enter stock symbol (e.g., AAPL): ").upper()
    shares = int(input("Enter number of shares: "))
    buy_price = float(input("Enter buy price per share: "))
    portfolio[symbol] = {'shares': shares, 'buy_price': buy_price}
    print(f"Added {shares} shares of {symbol} at ${buy_price:.2f} each.")

def show_portfolio():
    total_value = 0
    total_cost = 0
    print("\n📊 Portfolio Summary")
    print("{:<10} {:<10} {:<12} {:<12} {:<12}".format("Symbol", "Shares", "Buy Price", "Current", "P/L"))

    for symbol, data in portfolio.items():
        stock = yf.Ticker(symbol)
        current_price = stock.history(period="1d")['Close'].iloc[-1]
        shares = data['shares']
        buy_price = data['buy_price']
        current_value = current_price * shares
        cost = buy_price * shares
        profit_loss = current_value - cost
        total_value += current_value
        total_cost += cost
        print(f"{symbol:<10} {shares:<10} ${buy_price:<11.2f} ${current_price:<11.2f} ${profit_loss:<11.2f}")

    print(f"\n💰 Total Value: ${total_value:.2f}")
    print(f"💸 Total Cost: ${total_cost:.2f}")
    print(f"📈 Overall P/L: ${total_value - total_cost:.2f}")

# Main loop
while True:
    print("\n1. Add Stock")
    print("2. Show Portfolio")
    print("3. Exit")
    choice = input("Choose an option: ")

    if choice == '1':
        add_stock()
    elif choice == '2':
        show_portfolio()
    elif choice == '3':
        break
    else:
        print("Invalid choice. Try again.")

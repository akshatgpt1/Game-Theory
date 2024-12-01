# Game-Theory
Developed games based on cournot and bertrand model.
[GAME THEORY.pdf](https://github.com/user-attachments/files/17969503/GAME.THEORY.pdf)


Codes that are in the pdf

Code for the game Roulett spin(cournot model)-link of code( https://drive.google.com/file/d/1BwPJAwefBsRZz9ZRFVPqrtX0tvjfqZ7B/view?usp=drive_link) 
import random
class Player:
    def __init__(self, name):
        self.name = name
        self.contribution = 0
        self.payout = 0
    def make_contribution(self):
        while True:
            try:
                contribution = float(input(f"{self.name}, enter your bet amount: "))
                if contribution > 0:
                    self.contribution = contribution
                    return contribution
                else:
                    print("Please enter a positive amount.")
            except ValueError:
                print("Please enter a valid number.")
def calculate_pot_value(total_contributions, base_value, b):
    return base_value / (1 + b * total_contributions)
def calculate_payouts(players, pot_value, total_contributions):
    for player in players:
        player.payout = (player.contribution / total_contributions) * pot_value
def determine_winner(players):
    # Each player has an equal probability of winning
    return random.choice(players)
def play_game(num_players, base_value, b):
    players = [Player(f"Player {i+1}") for i in range(num_players)]
    total_contributions = 0
    for player in players:
        contribution = player.make_contribution()
        total_contributions += contribution
    pot_value = calculate_pot_value(total_contributions, base_value, b)
    calculate_payouts(players, pot_value, total_contributions)
    # Determine the winner with equal probability
    winner = determine_winner(players)
    print(f"\nTotal contributions: {total_contributions}")
    print(f"Pot value after diminishing returns: {pot_value:.2f}\n")
    print("Payouts:")
    for player in players:
        print(f"{player.name} payout: {player.payout:.2f}")
    print(f"\nThe winner is {winner.name}!")
    print(f"{winner.name} wins {winner.payout:.2f} from the pot!")
if __name__ == "__main__":
    num_players = int(input("Enter the number of players: "))
    base_value = 1000
    b = 0.01
    play_game(num_players, base_value, b)



Code for Price war simulation

Bertrand model code link ( https://drive.google.com/file/d/1ftrMeJqHUh0a__jna03GkBAXGyIRWNg_/view?usp=drive_link )

import random
class Firm:
    def __init__(self, name, marginal_cost):
        self.name = name
        self.price = 0
        self.profit = 0
        self.market_share = 0
        self.marginal_cost = marginal_cost
    def set_price(self):
        while True:
            try:
                price = float(input(f"{self.name}, enter your price: "))
                if price >= self.marginal_cost:
                    self.price = price
                    return price
                else:
                    print(f"Price must be at least the marginal cost: {self.marginal_cost}")
            except ValueError:
               print("Please enter a valid number.")
def allocate_market_share(firms):
    min_price = min(firm.price for firm in firms)
    firms_with_min_price = [firm for firm in firms if firm.price == min_price]
    share_per_firm = 1 / len(firms_with_min_price)  # Assume total market share is 1 (100%)
    for firm in firms:
        if firm in firms_with_min_price:
            firm.market_share = share_per_firm
        else:
            firm.market_share = 0
def calculate_profits(firms):
    for firm in firms:
        firm.profit = (firm.price - firm.marginal_cost) * firm.market_share * 1000  # Assume market size is 1000 units
def play_game(num_firms, marginal_cost):
    firms = [Firm(f"Firm {i+1}", marginal_cost) for i in range(num_firms)]
    # Firms set their prices
    for firm in firms:
        firm.set_price()
    # Allocate market share based on prices
    allocate_market_share(firms)
    # Calculate profits based on market share
    calculate_profits(firms)
    print("\nResults:")
    for firm in firms:
        print(f"{firm.name} set a price of {firm.price}, got a market share of {firm.market_share * 100:.2f}%, and earned a profit of {firm.profit:.2f}.")
    # Determine the winner
    winner = max(firms, key=lambda firm: firm.profit)
    print(f"\nThe winner is {winner.name} with a profit of {winner.profit:.2f}!")
if __name__ == "__main__":
    num_firms = int(input("Enter the number of firms: "))
    marginal_cost = float(input("Enter the marginal cost of production: "))
    play_game(num_firms, marginal_cost)

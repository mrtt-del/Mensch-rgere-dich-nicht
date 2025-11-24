import random

class Board:
    def __init__(self):
        # Erstelle ein Spielfeld mit 40 Feldern
        self.board = {i: None for i in range(40)}  # Wir verwenden 40 Felder
        self.players = {1: [], 2: []}  # Spieler 1 und Spieler 2

    def add_player(self, player_number):
        # Jeder Spieler startet mit 4 Figuren an Position 0
        self.players[player_number] = [0] * 4

    def roll_dice(self):
        # Würfeln mit einem Wert zwischen 1 und 6
        return random.randint(1, 6)

    def move(self, player_number, piece_index, steps):
        # Bewege die Figur des Spielers auf dem Spielfeld
        current_position = self.players[player_number][piece_index]
        new_position = current_position + steps

        # Wenn die neue Position nicht über das Spielfeld hinausgeht
        if new_position < 40:
            self.players[player_number][piece_index] = new_position
            print(f"Spieler {player_number}, Figur {piece_index + 1} zieht auf Feld {new_position}.")
        else:
            print(f"Spieler {player_number}, Figur {piece_index + 1} kann nicht ziehen, weil das Ziel überschritten wird.")

    def show_board(self):
        # Zeigt die aktuellen Positionen der Figuren beider Spieler an
        print("\nAktueller Spielfeld-Status:")
        print(f"Spieler 1: {self.players[1]}")
        print(f"Spieler 2: {self.players[2]}")
        print("-" * 40)

class Game:
    def __init__(self):
        self.board = Board()
        self.board.add_player(1)  # Füge Spieler 1 hinzu
        self.board.add_player(2)  # Füge Spieler 2 hinzu
        self.turn = 1  # Spieler 1 beginnt

    def player_turn(self):
        # Beginne den Zug des aktuellen Spielers
        print(f"\nSpieler {self.turn} ist am Zug.")

        # Der Spieler würfelt
        dice_roll = self.board.roll_dice()
        print(f"Spieler {self.turn} würfelt eine {dice_roll}.")

        # Der Spieler wählt eine Figur (0-3)
        print("Wähle eine Figur zum Ziehen (0-3):")
        piece_index = int(input())  # Der Spieler gibt die Figur an, die bewegt werden soll

        # Die Figur entsprechend der geworfenen Zahl bewegen
        self.board.move(self.turn, piece_index, dice_roll)

        # Zeige den aktuellen Spielfeld-Status
        self.board.show_board()

        # Überprüfe, ob der Spieler gewonnen hat
        if self.check_win(self.turn):
            print(f"Spieler {self.turn} hat gewonnen!")
            return True

        # Wechseln zum anderen Spieler
        self.turn = 2 if self.turn == 1 else 1
        return False

    def check_win(self, player_number):
        # Überprüft, ob der Spieler mit allen 4 Figuren das Ziel erreicht hat (Feld 39)
        return all(position == 39 for position in self.board.players[player_number])

    def play(self):
        # Spiel läuft solange bis ein Spieler gewonnen hat
        while True:
            if self.player_turn():
                break  # Wenn ein Spieler gewonnen hat, endet das Spiel

# Hauptspiel starten
if __name__ == "__main__":
    game = Game()
    game.play()

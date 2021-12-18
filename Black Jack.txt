from random import shuffle

def createDeck():
    Deck = []

    faceValues = ["A", "J", "Q", "K"]
    for i in range(4):
        for card in range(2, 11):
            Deck.append(str(card))

        for card in faceValues:
            Deck.append(card)

    shuffle(Deck)
    return Deck

cardDeck = createDeck()

print(cardDeck)

class Player:
    def __init__(self, hand = [], money = 100):
        self.hand = hand
        self.score = self.setScore()
        self.money = money
        self.bet = 0

    def __str__(self):
        currentHand = ""

        for card in self.hand:
            currentHand += str(card) + " "

        finalStatus = currentHand + "score: " + str(self.score)

        return finalStatus

    def setScore(self):
        self.score = 0

        faceCardsDict = {"A":11, "J":10, "Q":10, "K":10, "2":2, "3":3, "4":4, "4":4, "5":5, "6":6, "7":7, "8":8, "9":9, "10":10}

        aceCounter = 0
        for card in self.hand:
            self.score += faceCardsDict[card]

        return self.score

    def hit(self, card):
        self.hand.append(card)
        self.score = self.setScore()

    def play(self, newHand):
        self.hand = newHand
        self.score = self.setScore()

    def betMoney(self, amount):
        self.money -= amount
        self.bet += amount

    def win(self, result):
        if result == True:
            if self.money == 21 and len(self.hand) == 2:
                self.money += 2.5 * self.bet
            else:
                self.money += 2 * self.bet

            self.bet = 0
        else:
            self.bet = 0

    def draw(self):
        self.money += self.bet
        self.bet = 0

    def hasBlackJack(self):
        if self.score == 21 and len(self.hand) == 2:
            return True
        else:
            return False

def printHouse(House):
    for card in range(len(House.hand)):
        if card == 0:
            print("X", end = " ")
        elif card == len(House.hand) - 1:
            print(House.hand[card])
        else:
            print(House.hand[card], end = " ")

cardDeck = createDeck()
firstHand = [cardDeck.pop(), cardDeck.pop()]
secondHand = [cardDeck.pop(), cardDeck.pop()]
PLayer1 = Player(firstHand)
House = Player(secondHand)
cardDeck = createDeck()
while (True):
    if len(cardDeck) < 20:
        cardDeck = createDeck()
    firstHand = [cardDeck.pop(), cardDeck.pop()]
    secondHand = [cardDeck.pop(), cardDeck.pop()]
    PLayer1.play(firstHand)
    House.play(secondHand)

    Bet = int(input("Please, enter your bet: "))

    PLayer1.betMoney(Bet)

    print(cardDeck)
    printHouse(House)
    print(PLayer1)

    if PLayer1.hasBlackJack:
        if House.hasBlackJack:
            PLayer1.draw()
        else:
            PLayer1.win(True)
    else:
        while(PLayer1.score < 21):
            action = input("Do you wnat another card?(y/n): ")
            if action == "y":
                PLayer1.hit(cardDeck.pop())
                print(PLayer1)
            else:
                break
        while(House.score < 16):
            House.hit(cardDeck.pop())

        if PLayer1.score > 21:
            if House.score > 21:
                PLayer1.draw()
            else:
                PLayer1.win(False)

        elif PLayer1.score > House.score:
            PLayer1.win(True)

        elif PLayer1.score == House.score:
            PLayer1.draw()

        else:
            if House.score > 21:
                PLayer1.win(True)
            else:
                PLayer1.win(False)

    print(House)
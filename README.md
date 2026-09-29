# Hangman-game#
!/usr/bin/env python3
"""Hangman - a simple terminal game."""

import random

WORDS = [
    "python", "hangman", "computer", "keyboard", "program", "function",
    "variable", "developer", "algorithm", "database", "network", "internet",
    "software", "library", "compiler", "terminal", "puzzle", "elephant",
    "mountain", "chocolate", "adventure", "umbrella", "notebook", "rainbow",
]

MAX_MISTAKES = 6

STAGES = [
    """
     -----
     |   |
         |
         |
         |
         |
    =========""",
    """
     -----
     |   |
     O   |
         |
         |
         |
    =========""",
    """
     -----
     |   |
     O   |
     |   |
         |
         |
    =========""",
    """
     -----
     |   |
     O   |
    /|   |
         |
         |
    =========""",
    """
     -----
     |   |
     O   |
    /|\\  |
         |
         |
    =========""",
    """
     -----
     |   |
     O   |
    /|\\  |
    /    |
         |
    =========""",
    """
     -----
     |   |
     O   |
    /|\\  |
    / \\  |
         |
    =========""",
]


def display(word, guessed, mistakes):
    print(STAGES[mistakes])
    shown = " ".join(c if c in guessed else "_" for c in word)
    print(f"\n  Word: {shown}")
    wrong = sorted(g for g in guessed if g not in word)
    print(f"  Wrong guesses ({mistakes}/{MAX_MISTAKES}): {' '.join(wrong) or '-'}\n")


def get_guess(guessed):
    while True:
        guess = input("Guess a letter: ").strip().lower()
        if len(guess) != 1 or not guess.isalpha():
            print("Please enter a single letter.")
        elif guess in guessed:
            print("You already guessed that letter.")
        else:
            return guess


def play_round():
    word = random.choice(WORDS)
    guessed = set()
    mistakes = 0

    while mistakes < MAX_MISTAKES and not set(word) <= guessed:
        display(word, guessed, mistakes)
        guess = get_guess(guessed)
        guessed.add(guess)
        if guess in word:
            print(f"Good job! '{guess}' is in the word.")
        else:
            mistakes += 1
            print(f"Sorry, '{guess}' is not in the word.")

    display(word, guessed, mistakes)
    if mistakes == MAX_MISTAKES:
        print(f"Game over! The word was '{word}'.")
    else:
        print(f"You win! The word was '{word}'.")


def main():
    print("=== HANGMAN ===")
    while True:
        play_round()
        again = input("\nPlay again? (y/n): ").strip().lower()
        if again != "y":
            print("Thanks for playing!")
            break


if __name__ == "__main__":
    main()
    

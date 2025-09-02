# Katzenbilder-Zahlenraten

import tkinter as tk
from PIL import Image, ImageTk
import random
import os

# Set image paths directly here
START_IMAGE_PATH = r"c:\Users\melvin.lueckel\Downloads\1000-231.png"
WRONG_IMAGE_PATH = r"c:\Users\melvin.lueckel\Downloads\51TSRCAw-uL._UF894,1000_QL80_.png"
CORRECT_IMAGE_PATH = r"c:\Users\melvin.lueckel\Downloads\Thumbs_up_cat_meme.png"
INVALID_IMAGE_PATH = r"c:\Users\melvin.lueckel\Downloads\97df8142297e40f889bb3072a0d0bf0f.png"  # <<< Set your image path here

def load_image(path):
    try:
        if path and os.path.exists(path):
            img = Image.open(path)
            img = img.resize((300, 300))  # Resize if desired
            return ImageTk.PhotoImage(img)
        else:
            return None
    except Exception as e:
        print(f"Fehler beim Laden des Bildes: {e}")
        return None

def start_game():
    global secret_number
    secret_number = random.randint(1, 10)
    update_image(START_IMAGE_PATH)
    result_label.config(text="Neues Spiel! Rate eine Zahl zwischen 1 und 10.")
    entry.config(state="normal")
    guess_button.config(state="normal")
    continue_button.pack_forget()
    stop_button.pack_forget()

def check_guess():
    try:
        guess = int(entry.get())
    except ValueError:
        result_label.config(text="Bitte gib eine gültige Zahl ein!")
        update_image(INVALID_IMAGE_PATH)  # Show your custom invalid input image
        return

    if guess < secret_number:
        result_label.config(text="Die eingegebene Zahl ist zu klein")
        update_image(WRONG_IMAGE_PATH)
    elif guess > secret_number:
        result_label.config(text="Die eingegebene Zahl ist zu hoch")
        update_image(WRONG_IMAGE_PATH)
    else:
        result_label.config(text="Gewonnen! Willst du weiterspielen?")
        update_image(CORRECT_IMAGE_PATH)
        entry.config(state="disabled")
        guess_button.config(state="disabled")
        continue_button.pack()
        stop_button.pack()

def update_image(image_path):
    img = load_image(image_path)
    if img:
        image_label.config(image=img, text="")
        image_label.image = img
    else:
        image_label.config(image="", text="Kein Bild geladen")
        image_label.image = None

def quit_game():
    root.destroy()

# Initialize window
root = tk.Tk()
root.title("Zahlenratespiel mit Bildern")

tk.Button(root, text="Spiel starten", command=start_game).pack()

prompt_label = tk.Label(root, text="Rate eine Zahl zwischen 1 und 10:")
prompt_label.pack()

entry = tk.Entry(root)
entry.pack()

guess_button = tk.Button(root, text="Rate!", command=check_guess)
guess_button.pack()

result_label = tk.Label(root, text="")
result_label.pack()

image_label = tk.Label(root, text="Kein Bild geladen")
image_label.pack()

continue_button = tk.Button(root, text="Weiterspielen", command=start_game)
stop_button = tk.Button(root, text="Beenden", command=quit_game)

start_game()

root.mainloop()

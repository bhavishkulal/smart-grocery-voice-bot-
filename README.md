# smart-grocery-voice-bot-
A Python-based voice-controlled smart grocery bot with GUI that recognizes spoken items and generates a billing list using speech recognition and gTTS.
import tkinter as tk
import speech_recognition as sr
from gtts import gTTS
import os
import time
from playsound import playsound

# Item database with prices (you can add more items)
items = {
    "apple": 30,
    "banana": 10,
    "milk": 50,
    "bread": 40,
    "rice": 60
}

recognized_items = []

def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        label.config(text="Listening...")
        window.update()
        audio = recognizer.listen(source)
        try:
            text = recognizer.recognize_google(audio).lower()
            label.config(text=f"You said: {text}")
            if text in items:
                recognized_items.append(text)
                update_list()
            else:
                speak("Item not found")
        except sr.UnknownValueError:
            label.config(text="Could not understand audio")
            speak("I didn't catch that")
        except sr.RequestError:
            label.config(text="Speech recognition error")
            speak("Network error")

def speak(text):
    tts = gTTS(text=text, lang='en')
    filename = "voice.mp3"
    tts.save(filename)
    playsound(filename)
    os.remove(filename)

def update_list():
    text = ""
    total = 0
    for item in recognized_items:
        price = items[item]
        total += price
        text += f"{item.title()} - ₹{price}\n"
    text += f"\nTotal: ₹{total}"
    bill_text.set(text)

# GUI Setup
window = tk.Tk()
window.title("Smart Grocery Voice Bot")
window.geometry("400x400

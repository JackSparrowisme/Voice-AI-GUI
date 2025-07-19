# Voice-AI-GUI
Voice AI Assistant is a GUI-based smart assistant built in Python that allows users to interact with their computer using voice commands. It supports voice input using speech_recognition and responds with text-to-speech via pyttsx3. The interface is created with Tkinter, making the assistant visually interactive and user-friendly.

import tkinter as tk
from tkinter import messagebox
import pyttsx3
import speech_recognition as sr
import datetime
import random
import webbrowser

# Initialize speech engine
engine = pyttsx3.init()
engine.setProperty('rate', 150)
engine.setProperty('volume', 1.0)

# Bot replies
jokes = [
    "Why don’t scientists trust atoms? Because they make up everything.",
    "I told my computer I needed a break, and it said no problem — it’ll go to sleep.",
    "Why was the math book sad? Because it had too many problems."
]

def speak(text):
    output_box.insert(tk.END, f"Bot: {text}\n")
    engine.say(text)
    engine.runAndWait()

def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        speak("Listening...")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)
    try:
        command = recognizer.recognize_google(audio).lower()
        output_box.insert(tk.END, f"You: {command}\n")
        return command
    except sr.UnknownValueError:
        speak("Sorry, I didn't understand that.")
    except sr.RequestError:
        speak("Sorry, my voice service is down.")
    return ""

def handle_command():
    command = listen()
    if command == "":
        return

    if "hello" in command or "hi" in command:
        speak("Hi there! How can I help you?")
    elif "your name" in command:
        speak("I am Jarvis, your AI assistant.")
    elif "time" in command:
        now = datetime.datetime.now().strftime("%I:%M %p")
        speak(f"The time is {now}")
    elif "joke" in command:
        speak(random.choice(jokes))
    elif "google" in command:
        speak("Opening Google")
        webbrowser.open("https://www.google.com")
    elif "youtube" in command:
        speak("Opening YouTube")
        webbrowser.open("https://www.youtube.com")
    elif "bye" in command or "exit" in command:
        speak("Goodbye!")
        root.destroy()
    else:
        speak("I didn't get that.")

# GUI Setup
root = tk.Tk()
root.title("Voice Assistant GUI")
root.geometry("500x400")
root.configure(bg="#1e1e1e")

heading = tk.Label(root, text="🎙️ Voice AI Assistant", font=("Helvetica", 18), fg="white", bg="#1e1e1e")
heading.pack(pady=10)

output_box = tk.Text(root, height=15, width=55, bg="#f0f0f0", font=("Courier", 10))
output_box.pack(pady=10)

listen_btn = tk.Button(root, text="🎧 Speak", font=("Helvetica", 14), command=handle_command, bg="#00adb5", fg="white", padx=20, pady=5)
listen_btn.pack(pady=10)

root.mainloop()

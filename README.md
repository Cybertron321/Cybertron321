import speech_recognition as sr
import pyttsx3

def speak(text):
    engine = pyttsx3.init()
    engine.say(text)
    engine.runAndWait()

def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)

    try:
        print("Recognizing...")
        command = recognizer.recognize_google(audio).lower()
        print(f"User: {command}")
        return command
    except sr.UnknownValueError:
        print("Sorry, I didn't catch that. Could you please repeat?")
        return listen()
    except sr.RequestError as e:
        print(f"Could not request results from Google Speech Recognition service; {e}")
        return None

def virtual_assistant(command):
    if "hello" in command:
        speak("Hello! How can I help you today?")
    elif "bye" in command:
        speak("Goodbye! Have a great day.")
        exit()
    else:
        speak("I'm not sure how to respond to that. Can you please provide more details?")

if __name__ == "__main__":
    speak("Hello! I am Cyber, your virtual assistant. How can I assist you today?")

    while True:
        command = listen()
        if command:
            virtual_assistant(command)

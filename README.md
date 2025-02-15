import streamlit as st
from nltk.chat.util import Chat, reflections

# Define the pairs for your chatbot
pairs = [
    [
        r"(.*)my name is (.*)",  # Pattern to match when the user says their name.
        ["Hello %2, How are you today?",]
    ],
    [
        r"(.*)help(.*)",  # Pattern to match when the user asks for help.
        ["I can help you",]
    ],
    [
        r"(.*) your name ?",  # Pattern to match when the user asks for the bot's name.
        ["My name is thecleverprogrammer, but you can just call me robot and I'm a chatbot.",]
    ],
    [
        r"how are you (.*) ?",  # Pattern to match when the user asks how the bot is.
        ["I'm doing very well", "I am great!"]
    ],
    [
        r"sorry (.*)",  # Pattern to match when the user apologizes.
        ["It's alright", "It's OK, never mind that",]
    ],
    [
        r"i'm (.*) (good|well|okay|ok)",  # Pattern to match when the user says they are doing well.
        ["Nice to hear that", "Alright, great!",]
    ],
    [
        r"(hi|hey|hello|hola|holla)(.*)",  # Pattern to match greetings.
        ["Hello", "Hey there",]
    ],
    [
        r"what (.*) want ?",  # Pattern to match when the user asks what the bot wants.
        ["Make me an offer I can't refuse",]
    ],
    [
        r"(.*)created(.*)",  # Pattern to match when the user asks who created the bot.
        ["Basheer created me using Python's NLTK library", "Top secret ;)",]
    ],
    [
        r"(.*) (location|city) ?",  # Pattern to match when the user asks where the bot is located.
        ['Hyderabad, India',]
    ],
    [
        r"(.*)raining in (.*)",  # Pattern to match when the user asks if it’s raining in a city.
        ["No rain in the past 4 days here in %2", "In %2 there is a 50% chance of rain",]
    ],
    [
        r"(.*)(sports|game|sport)(.*)",  # Pattern to match when the user asks about sports.
        ["I'm a very big fan of Cricket",]
    ],
    [
        r"who (.*) (Cricketer|Batsman)?",  # Pattern to match when the user asks about a cricketer.
        ["Virat Kohli"]
    ],
    [
        r"quit",  # Pattern to match when the user wants to end the conversation.
        ["Bye for now. See you soon :)", "It was nice talking to you. See you soon :)"]
    ],
    [
        r"(.*)",  # Default pattern to match any other input.
        ['Our customer service will reach you.']
    ],
]

# Create the ChatBot
chat = Chat(pairs, reflections)

# Define a function to handle conversation
def chatbot_response(user_input):
    return chat.respond(user_input)

# Set up Streamlit interface
st.title("Chatbot with Streamlit")
st.write("Hi, I'm thecleverprogrammer! I like to chat. Please type a message below.")

# Create a text input box for user to type their message
user_input = st.text_input("Your message:")

# Display the chat history
if 'conversation' not in st.session_state:
    st.session_state.conversation = []

if user_input:
    bot_response = chatbot_response(user_input)
    # Store user input and bot response in session state
    st.session_state.conversation.append(("User", user_input))
    st.session_state.conversation.append(("Bot", bot_response))

# Display the conversation
for speaker, message in st.session_state.conversation:
    if speaker == "User":
        st.write(f"**{speaker}:** {message}")
    else:
        st.write(f"**{speaker}:** {message}")

# Add a "Quit" button to exit the chat
if user_input.lower() == "quit":
    st.write("It was nice talking to you. See you soon :)")
    st.session_state.conversation = []  # Clear the conversation

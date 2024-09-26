//Initialize Necessary Libraries and APIs

Import necessary libraries:-
IMPORT discord 
IMPORT pipeline from transformers 
IMPORT spotipy

//Set up Spotify OAuth authentication with client credentials.
Initialize the Hugging Face emotion analysis model using pipeline.


//Setup Discord Bot:
DEFINE the Discord bot token.
SETUP Discord bot intents to allow reading messages.
Initialize the bot with a command prefix (!).
Define Utility Functions:

Emotion Analyzer Function:

Input: A string of text.
Process: Use the Hugging Face pipeline to analyze the text and return the dominant emotion.
Playlist Recommender Function:

Input: A detected emotion.
Process:
Map the emotion to a specific genre.
Use the Spotify API to get track recommendations based on the genre.
Create a new Spotify playlist with the recommended tracks.
Return the playlist URL.
Store Data in Database Function:

Input: User ID, user message, detected emotion, and recommended songs.
Process: Insert this data into a MongoDB collection.
Bot Event Handling:

On Bot Ready:

Print a message indicating the bot is online.
On Start Command:

Activate listening to messages when the !start command is issued.
Initialize an empty list to store messages (MESSAGES).
On Message:

Check if the message author is not the bot itself.
If the message is not a command (doesn’t start with !):
Append the message content to the MESSAGES list.
If the list length reaches 3 messages:
Concatenate the messages into a single text string.
Analyze the concatenated text to detect the dominant emotion.
Send the detected emotion as a response in the chat.
Generate a Spotify playlist based on the detected emotion.
Send the playlist URL as a response in the chat.
Store the user ID, messages, detected emotion, and playlist in the database.
Ensure the bot processes other commands.
Run the Bot:

Start the bot using the provided token.

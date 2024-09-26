Emotion-Based Music Playlist Generator
Table of Contents
Introduction
Setup
Emotion Analysis
Playlist Generation
Storage
Discord Bot
Introduction
This project generates a music playlist based on the user's emotions detected from their messages.

Setup
Environment Variables
CLIENT_ID: Spotify client ID
CLIENT_SECRET: Spotify client secret
REDIRECT_URI: Spotify redirect URI
SCOPE: Spotify scope
TOKEN: Discord bot token
CHANNEL_ID: Discord channel ID
Dependencies
discord
spotipy
transformers
pymongo
Emotion Analysis
emotion_analyser Function
Input: text (user message)
Output: emotion (detected emotion)
markdown

Verify

Open In Editor
Edit
Copy code
function emotion_analyser(text)
  # Use Hugging Face pipeline to classify text into emotions
  emotions = pipeline("text-classification", model='bhadresh-savani/distilbert-base-uncased-emotion')(text)
  return emotions[0]['label']
Playlist Generation
create_mood_playlist Function
Input: mood (detected emotion)
Output: playlist_url (generated playlist URL)
markdown

Verify

Open In Editor
Edit
Copy code
function create_mood_playlist(mood)
  # Map emotions to genres
  genres = {
    "admiration": "ambient",
    "amusement": "dance-pop",
    ...
  }
  
  # Get recommendations from Spotify
  results = sp.recommendations(seed_genres=[genres[mood]], limit=10)
  
  # Create a new playlist
  user_id = sp.current_user()['id']
  playlist = sp.user_playlist_create(user=user_id, name=f"{mood.capitalize()} Playlist")
  playlist_url = playlist['external_urls']['spotify']
  
  # Add tracks to the playlist
  sp.user_playlist_add_tracks(user=user_id, playlist_id=playlist['id'], tracks=results['tracks'])
  
  return playlist_url
Storage
store_in_db Function
Input: user_id, user_message, emotion, songs
Output: None
markdown

Verify

Open In Editor
Edit
Copy code
function store_in_db(user_id, user_message, emotion, songs)
  # Store data in MongoDB
  db.message.insert_one({
    'user': user_id,
    'message': user_message,
    'emotion': emotion,
    'recommended_songs': songs
  })
Discord Bot
on_ready Event
Triggered when the bot is ready
markdown

Verify

Open In Editor
Edit
Copy code
function on_ready()
  print("Bot is ready as " + bot.user)
start Command
Starts reading messages in the channel
markdown

Verify

Open In Editor
Edit
Copy code
function start(ctx)
  await ctx.send("I'm now listening to your messages!")
  MESSAGES = []
  
  # Activate reading messages in the channel
  function on_message(message)
    # Ignore messages from the bot itself
    if message.author == bot.user:
      return
    
    # If the message is not a command, the bot reads and responds
    if not message.content.startswith('!'):
      MESSAGES.append(message.content)
      if len(MESSAGES) == 3:
        await message.channel.send("You said: " + MESSAGES)
        emotion = emotion_analyser(MESSAGES)
        await message.channel.send("Your Dominant Emotion right now: " + emotion)
        await message.channel.send("Creating a Playlist According to your Emotion....")
        song_url = create_mood_playlist(emotion)
        await message.channel.send("Playlist: " + song_url)
        return
    await bot.process_commands(message)
Commit Messages
feat: add emotion analysis feature
feat: add playlist generation feature
feat: add storage feature
feat: add discord bot feature
fix: fix bug in emotion analysis
docs: add documentation for functions
API Documentation
emotion_analyser(text): Detects the emotion from the input text
create_mood_playlist(mood): Generates a playlist based on the input mood
store_in_db(user_id, user_message, emotion, songs): Stores data in MongoDB
on_ready(): Triggered when the bot is ready
start(ctx): Starts reading messages in the channel

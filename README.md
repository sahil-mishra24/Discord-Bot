import discord
import os
from discord import user
import requests
import json
import random

intents = discord.Intents.default()
client = discord.Client(intents=intents)


sad_words = ["sad", "depressing","bad","depressed","exhausted","hurt","crying"]

starter_encouragements = ["Ayy, don't worry, mate. A shitty day doesn't mean a shitty life!","Meh, just think you are better than everyone else and MOVE ON.","As long as you are not a fugitive or knocked up, you are good to go! "]

def get_quote():
  response = requests.get("https://zenquotes.io/api/random")
  json_data = json.loads(response.text)
  quote = json_data[0]['q']
  return(quote)

@client.event
async def on_ready():
  print('Yooooo, welcome, buddy! We have logged in as {client.user}')

@client.event
async def on_message(message):
  if message.author == client.user:
    return
  if message.content.startswith('Hello'):
    quote = get_quote()
    await message.channel.send(quote)

  if any(word in message.content for word in sad_words):
    await message.channel.send(random.choice(starter_encouragements))
    

client.run('TOKEN')

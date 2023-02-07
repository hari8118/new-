# new-
import logging
import telegram
from telegram.ext import Updater, CommandHandler, MessageHandler, Filters
from openai import OpenAI

# Initialize the OpenAI API client
openai = OpenAI(
    "YOUR_OPENAI_API_KEY"
)

# Enable logging
logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
                    level=logging.INFO)

logger = logging.getLogger(__name__)

# Define a function to handle the /start command
def start(update, context):
    context.bot.send_message(chat_id=update.effective_chat.id, text="Hi! I am a infosec bot. How can I help you today?")

# Define a function to handle all other messages
def chat(update, context):
    request = update.message.text
    response = openai.Completion.create(
        engine="text-davinci-002",
        prompt='What do you want to know about?',
        max_tokens=1024,
        n=1,
        stop=None,
        temperature=0.5,
    ).choices[0].text

    context.bot.send_message(chat_id=update.effective_chat.id, text=response)

# Define the handler functions
start_handler = CommandHandler('start', start)
chat_handler = MessageHandler(Filters.text, chat)

# Initialize the Updater and add the handlers to it
updater = Updater(token="YOUR_TELEGRAM_BOT_TOKEN", use_context=True)
dispatcher = updater.dispatcher
dispatcher.add_handler(start_handler)
dispatcher.add_handler(chat_handler)

# Start the bot
updater.start_polling()

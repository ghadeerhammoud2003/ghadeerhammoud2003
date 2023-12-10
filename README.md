from telegram.ext import Updater, CommandHandler, MessageHandler, Filters

# Define a function to handle /start command
def start(update, context):
    context.bot.send_message(chat_id=update.effective_chat.id, text="Welcome to the anonymous chat bot!")

# Define a function to handle incoming messages
def chat(update, context):
    # Get the message text
    message_text = update.message.text
    
    # Send the message to all other chat participants
    for chat_id in context.chat_data:
        if chat_id != update.effective_chat.id:
            context.bot.send_message(chat_id=chat_id, text=message_text)

# Define a function to handle errors
def error(update, context):
    print(f"Update {update} caused error {context.error}")

# Set up the bot
def main():
    # 6970097782:AAEyp5bBAWpGdVb35-nV0mJxEFZa1FO3EDg
    updater = Updater(token='6970097782:AAEyp5bBAWpGdVb35-nV0mJxEFZa1FO3EDg', use_context=True)
    dispatcher = updater.dispatcher

    # Add handlers for commands and messages
    start_handler = CommandHandler('start', start)
    chat_handler = MessageHandler(Filters.text & ~Filters.command, chat)
    dispatcher.add_handler(start_handler)
    dispatcher.add_handler(chat_handler)
    dispatcher.add_error_handler(error)

    # Start the bot
    updater.start_polling()
    updater.idle()

if __name__ == '__main__':
    main()

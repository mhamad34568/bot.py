import telebot

TOKEN = '8849078742:AAE8eAiIdDOtUZpUUn6FhG_1Ci_1yzKizTA'
bot = telebot.TeleBot(TOKEN)

@bot.message_handler(commands=['start', 'help'])
def send_welcome(message):
    bot.reply_to(message, "Hello! Bot is working.")

@bot.message_handler(func=lambda message: True)
def echo_all(message):
    bot.reply_to(message, message.text)

bot.infinity_polling()

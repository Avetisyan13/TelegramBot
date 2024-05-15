import telebot


TOKEN = '6728954039:AAFUSEdBtpCT_lW88qQl-u1zQuuyWWgRQNU'


bot = telebot.TeleBot(TOKEN)


@bot.message_handler()
def echo_test(message: telebot.types.Message):
    bot.send_message(message.chat.id, 'Hello')


    bot.polling()
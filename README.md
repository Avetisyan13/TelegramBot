import telebot
import requests
import json


TOKEN = '6728954039:AAFUSEdBtpCT_lW88qQl-u1zQuuyWWgRQNU'


bot = telebot.TeleBot(TOKEN)

keys = {
    'евро': 'EUR',
    'доллар': 'USD',
    'рубль': 'RUB',
}


@bot.message_handler(commands=['start', 'help'])
def help(message: telebot.types.Message):
    text = 'Чтобы начать введите команду боту в следующем формате:\n<имя валюты> \
<в какую валюту перевести> \
<количество переводной валюты>nУвидить список всех доступных валют: /values'
    bot.reply_to(message, text)


@bot.message_handler(commands=['values'])
def values(message: telebot.types.Message):
    text = 'Доступные валюты:'
    for key in keys.keys():
        text = '\n'.join((text, key, ))
    bot.reply_to(message, text)


@bot.message_handler(content_types=['text', ])
def convert(message: telebot.types.Message):
    quote, base, amount = message.text.split(' ')
    r = requests.get(f'https://www.cbr.ru/scripts/XML_daily.asp?fsym={keys[quote]}&tsyms={keys[base]}')
    text = json.loads(r.content)[keys[base]]
    bot.send_message(message.chat.id, text)


bot.polling()

# DesignDivision
Logo Creator


import pyowm
import telebot

owm = pyowm.OWM('5e8d359af938df914953c4f1b5054073', language = "ru" )
bot = telebot.TeleBot("1097999423:AAEB0vd2BglBmG5hCbT0BsnI502OMf_lLiU")

@bot.message_handler(content_types=['text'])
def send_echo(message):
	observation = owm.weather_at_place( message.text )
	w = observation.get_weather()
	temp = w.get_temperature('celsius')["temp"]

	answer = "В городе " + message.text + " сейчяс " + w.get_detailed_status() + "\n"
	answer += "Темпратура сейчяс в районе " + str(temp) + "\n\n"

	if temp < 10:
		answer += "Cейчяс очень холодно, Одивайся тепло!"
	elif temp < 20:
		answer += "Сейчяс холодно, одивайся потеплее."
	else:
		answer += "Темпратура норм, одевай что угодно."

	bot.send_message(message.chat.id, answer)
bot.polling( none_stop = True )
